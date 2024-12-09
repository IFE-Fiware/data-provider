# Dataprovider Agent

## Description

This repo contains:
- a master helm chart allowing to deploy a **Dataprovider** agent using a single command.
- templates of values.yaml files used inside *Integration* environment under `app-values` folder

## Pre-Requisites

| Pre-Requisites         |     Version     | Description                                                                                                                                     |
| ---------------------- |     :-----:     | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| DNS sub-domain name    |       N/A       | This domain will be used to address all services of the agent. <br/> example: `*.dataprovider01.int.simpl-europe.eu`                            |  
| Kubernetes Cluster     | 1.29.x or newer | Other version *might* work but tests were performed using 1.29.x version                                                                        |
| nginx-ingress          | 1.10.x or newer | Used as ingress controller. <br/> Other version *might* work but tests were performed using 1.10.x version. <br/> Image used: `registry.k8s.io/ingress-nginx/controller:v1.10.0`  |
| cert-manager           | 1.15.x or newer | Used for automatic cert management. <br/> Other version *might* work but tests were performed using 1.15.x version. <br/> Image used: `quay.io/jetstack/cert-manager-controller::v1.15.3` |
| Hashicorp Vault        | 1.17.x or newer | Other version *might* work but tests were performed using 1.17.x version. <br/> Image used: `hashicorp/vault:1.17.2`                            |
| nfs-provisioner        | 4.0.x or newer  | Backend for *Read/Write many* volumes. <br/> Other version *might* work but tests were performed using 4.0.x version. <br/> Image used: `registry.k8s.io/sig-storage/nfs-provisioner:v4.0.8` |
| argocd                 | 2.11.x or newer | Used as GitOps tool . App of apps concept. <br/> Other version *might* work but tests were performed using 2.11.x version. <br/> Image used: `quay.io/argoproj/argocd:v2.11.3` |
| kube-state-metrics     | present         | Used for monitoring, Metricbeat statuses in Kibana dashboard |

## Installation

The deployment is based on master helm chart which, when applied on Kubernetes cluster, should deploy the Data Provider to it using ArgoCD. 

### Prerequisites

#### Create the Namespace
Once the namespace variable is set, you can create the namespace using the following kubectl command:

`kubectl create namespace dataprovider01`

#### Verify the Namespace
To ensure that the namespace was created successfully, run the following command:

`kubectl get namespaces`
<br/>This will list all the namespaces in your cluster, and you should see the one you just created listed.

#### Create volumes

Two volumes needs to be created manually at the moment:
* nfs-storage-pvc-xsfc
* nfs-storage-pvc-sdapibe

This will be fixed in future versions.

#### Vault related tasks

##### Preliminal activites (done once)

1. Execute shell of your vault pod `kubectl exec -it vault-0 -- /bin/sh`. In this case pod name is `vault-0`
2. Login to vault using cmd `vault login`. You will need to provide token for auth
3. Create secret engine `vault secrets enable -path=dev kv-v2` in this case name of the engine is `dev`
4. Enable kubernetes interaction with vault `vault auth enable kubernetes`
5. Add config for kubernetes `vault write auth/kubernetes/config  kubernetes_host="https://$KUBERNETES_PORT_443_TCP_ADDR:443"`
6. Write policy in vault for fetching credentials by kubernetes
```
vault policy write dev-policy - <<EOF
path "dev/data/*" {
   capabilities = ["read"]
}
EOF 
```
in this example `dev-policy` is name of policy - it can be anything, and path
`dev/data/*` needs to relate existing secret engine declared in pt 3.

7. Create role in vault that will bind policy with given service account name and service account namespace

```
vault write auth/kubernetes/role/gaiax-edc_role \
      bound_service_account_names=*-iaa \
      bound_service_account_namespaces=*-iaa \
      policies=dev_policy \
      ttl=24h
```
Explanation: `gaiax-edc_role` is a role name, it can be anything. `gaiax-edc-dev*` is a name for both service accounts
and kubernetes namespaces names of services account. In this case `*` wildcard was used so to use this role in each namespace
there should be kubernetes service account created with the name ending with `iaa` additionally this service
account need to be placed in namespace with a name ending with `iaa`. If you require other namespace naming convention
then the role need to be modified with correct namespaces names. `dev-policy` is a policy name defined in pt 6.

IMPORTANT  
Steps 1-7 need to be executed only once , if given role, policy, already exists in vault, then there is no need of configuring them again.

##### Secret for Signer

One secret is needed, its naming syntax is {{ .Release.Namespace }}-infra-adapter-simpl-backend", it should be created in created before kv secret engine.
Its content is (to be revised):

```
{
  "ENGINE_PATH": "/opt/plugins/hashicorp-vault-provider.so",
  "HTTP_HOST": "",
  "HTTP_IDLE_TIMEOUT": "120s",
  "HTTP_PORT": "8080",
  "HTTP_READ_TIMEOUT": "10s",
  "HTTP_WRITE_TIMEOUT": "10s",
  "LOG_ENCODING": "json",
  "LOG_LEVEL": "debug",
  "VAULT_ADRESS": "http://vault-ha.vault-ha.svc.cluster.local:8200",
  "VAULT_TOKEN": "hvs.generatedtoken"
}
```

Where you need to modify:

| Variable name                 |     Example         | Description     |
| ----------------------        |     :-----:         | --------------- |
| VAULT_ADDRESS            | http://vault-ha.vault-ha.svc.cluster.local:8200 | Internal link to Vault service  |
| VAULT_TOKEN              | hvs.generatedtoken | Token to access the Vault  |

### Deployment

#### Deployment using ArgoCD

You can easily deploy the agent using ArgoCD. All the values mentioned in the sections below you can input in ArgoCD deployment. The repoURL gets the package directly from code.europa.eu.
targetRevision is the package version. 

When you create it, you set up the values below (example values)

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: 'dataprovider01-deployer'                               # name of the deploying app in argocd
spec:
  project: default
  source:
    repoURL: 'https://code.europa.eu/api/v4/projects/904/packages/helm/stable'
    path: '""'
    targetRevision: 0.3.1
    helm:
      values: |
        values:
          branch: develop                                       # branch of repo with values - this is develop by default
        secretEngine: dev-int                                   # container for your secrets in vault
        project: default
        namespaceTag: dataprovider01                            # identifier of deployment and part of fqdn
        domainSuffix: int.simpl-europe.eu                       # last part of fqdn
        argocd:
          appname: dataprovider01-iaa                           # name of generated argocd app 
          namespace: argocd                                     # namespace of your argocd
        cluster:
          address: https://kubernetes.default.svc
          namespace: dataprovider01-iaa                         # where the app will be deployed
          kubeStateHost: kube-prometheus-stack-kube-state-metrics.devsecopstools.svc.cluster.local:8080    # link to kube-state-metrics svc
        authority:
          namespaceTag: authority1                              # namespace tag of target authority
        monitoring:
          enabled: true                                         # "true" enables the deployment of ELK stack for monitoring
        hashicorp:
          service: "http://vault-ha.vault-ha.svc.cluster.local:8200"  # local service path to your vault
    chart: data-provider
  destination:
    server: 'https://kubernetes.default.svc'
    namespace: dataprovider01-iaa                               # where the package will be deployed
```

#### Manual deployment

##### Files preparation

The suggested way for deployment, is to unpack the released package to a folder on a host where you have kubectl and helm available and configured. 

There is basically one file that you need to modify - values.yaml. 
There are a couple of variables you need to replace - described below. The rest you don't need to change.

```
project: default                                  # Project to which the namespace is attached
namespaceTag: dataprovider01                      # identifier of deployment and part of fqdn
authority:
  namespaceTag: authority1                        # namespace tag of target authority
domainSuffix: int.simpl-europe.eu                 # last part of fqdn

argocd:
  appname: dataprovider01-iaa                     # name of generated argocd app 
  namespace: argocd                               # namespace of your argocd

cluster:
  address: https://kubernetes.default.svc
  namespace: dataprovider01-iaa                   # where the package will be deployed
  kubeStateHost: kube-prometheus-stack-kube-state-metrics.devsecopstools.svc.cluster.local:8080    # link to kube-state-metrics svc


secretEngine: dev-int                             # container for your secrets in vault
hashicorp:
  service: "http://vault-ha.vault-ha.svc.cluster.local:8200"  # local service path to your vault

values:
  repo_URL: https://code.europa.eu/simpl/simpl-open/development/agents/data-provider.git  # repo URL
  branch: develop                                                                         # branch of code in repo
```

##### Deployment

After you have prepared the values file, you can start the deployment. 
Use the command prompt. Proceed to the folder where you have the Chart.yaml file and execute the following command. The dot at the end is crucial - it points to current folder to look for the chart. 

`helm install data-provider .`

After it's been already deployed and you want to implement changes, you can use this command (in the same folder):

`helm upgrade data-provider .`

### Monitoring

ELK stack for monitoring is added with this release.  
Its deployment can be disabled by switch the value monitoring.enabled to false.  
When it's enabled, after the stack is deployed, you can access the ELK stack UI by https://kibana.**namespacetag**.**domainsuffix**  
Default user is "elastic", its password can be extracted by kubectl command. `kubectl get secret elastic-elasticsearch-es-elastic-user -o go-template='{{.data.elastic | base64decode}}' -n {namespace}`

### Removal

To remove the deployment you need to do two things:

* `helm uninstall data-provider` - which will remove the application from argocd
* `kubectl delete ns ${NAMESPACE}` - which will remove the namespace ${NAMESPACE} that holds all of the components.