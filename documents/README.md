# Dataprovider Agent

<!-- TOC -->
* [Dataprovider Agent](#dataprovider-agent)
  * [Description](#description)
  * [Pre-Requisites](#pre-requisites)
    * [Onboarding](#onboarding-)
    * [Tools](#tools)
  * [Installation](#installation)
    * [Prerequisites](#prerequisites)
      * [Create the Namespace](#create-the-namespace)
      * [Verify the Namespace](#verify-the-namespace)
      * [Vault related tasks](#vault-related-tasks)
        * [Secret engine for Signer](#secret-engine-for-signer)
        * [Secret for Signer](#secret-for-signer)
        * [Secret for Contract](#secret-for-contract)
        * [Secret for Infrastructure-BE](#secret-for-infrastructure-be)
        * [Secret for EDC](#secret-for-edc)
    * [Deployment](#deployment)
      * [Deployment using ArgoCD](#deployment-using-argocd)
      * [Manual deployment](#manual-deployment)
        * [Files preparation](#files-preparation)
        * [Deployment](#deployment-1)
  * [Additional steps](#additional-steps)
    * [Monitoring](#monitoring)
* [Troubleshooting](#troubleshooting-)
<!-- TOC -->

## Description

This repo contains:
- a master helm chart allowing to deploy a **Dataprovider** agent using a single command.
- templates of values.yaml files used inside *Integration* environment under `app-values` folder

## Pre-Requisites

### Onboarding 
In the current version, the automatic onboarding process has already been implemented using: init-participant-job. 
For this reason, manual onboarding activities are no longer necessary.

### Tools

| Pre-Requisites      |     Version     | Description                                                                                                                                                                                  |
|---------------------|:---------------:|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DNS sub-domain name |       N/A       | This domain will be used to address all services of the agent. <br/> example: `*.dataprovider01.int.simpl-europe.eu`                                                                         |  
| Kubernetes Cluster  | 1.29.x or newer | Other version *might* work but tests were performed using 1.29.x version                                                                                                                     |
| nginx-ingress       | 1.10.x or newer | Used as ingress controller. <br/> Other version *might* work but tests were performed using 1.10.x version. <br/> Image used: `registry.k8s.io/ingress-nginx/controller:v1.10.0`          |
| cert-manager        | 1.15.x or newer | Used for automatic cert management. <br/> Other version *might* work but tests were performed using 1.15.x version. <br/> Image used: `quay.io/jetstack/cert-manager-controller::v1.15.3`    |
| nfs-provisioner     | 4.0.x or newer  | Backend for *Read/Write many* volumes. <br/> Other version *might* work but tests were performed using 4.0.x version. <br/> Image used: `registry.k8s.io/sig-storage/nfs-provisioner:v4.0.8` |
| argocd              | 2.11.x or newer | Used as GitOps tool . App of apps concept. <br/> Other version *might* work but tests were performed using 2.11.x version. <br/> Image used: `quay.io/argoproj/argocd:v2.11.3`            |

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

#### Vault related tasks

##### Secret engine for Signer

Go to Vault UI and define new transit secret engine with path `transit/simpl` create encryption key `gaia-x-key1` with type `ed25519`.

##### Secret for Signer

One secret is needed, its naming syntax is "{{ .Release.Namespace }}-infra-adapter-simpl-backend", it should be created in created before kv secret engine.
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
  "VAULT_ADRESS": "http://vaultservice.vaultnamespace.svc.cluster.local:8200",
  "VAULT_TOKEN": "hvs.generatedtoken"
}
```

Where you need to modify:

| Variable name                 |     Example         | Description     |
| ----------------------        |     :-----:         | --------------- |
| VAULT_ADDRESS            | http://vaultservice.vaultnamespace.svc.cluster.local:8200 | Internal link to Vault service  |
| VAULT_TOKEN              | hvs.generatedtoken | Token to access the Vault  |

##### Secret for Contract

One secret is needed, its naming syntax is "{{ .Release.Namespace }}-contract", it should be created in created before kv secret engine.
Its content is:

```
{
  "API_KEY": "apikey",
  "DBPASSWORD": "contract",
  "DB_URL": "jdbc:postgresql://postgresql.consumer01.svc.cluster.local:5432/contract",
  "DB_USER": "contract",
  "KAFKA_CLIENT_PASSWORDS": "contract"
}
```

Where you need to modify:

| Variable name           |     Example         | Description     |
| ----------------------  |     :-----:         | --------------- |
| DB_USER                 | contract | User for contract database |
| DBPASSWORD              | contract | Password for contract database  |
| DB_URL                  | jdbc:postgresql://postgresql.consumer01.svc.cluster.local:5432/contract | Link to datasource |
| KAFKA_CLIENT_PASSWORDS  | password | Password for kafka connection |

##### Secret for Infrastructure-BE

One secret is needed, its name is "*namespace*-infrastructure-be", it should be created in created before kv secret engine.
Its content is:
```
{
  "kafka.sasl.enabled": true,
  "kafka.sasl.password": 
  "kafka.sasl.username":
  "spring.datasource.password": "infrabe",
  "spring.datasource.username": "infrabe",
  "spring.mail.password": "pass",
  "spring.mail.username": "user"
}
```
Where you need to modify:

| Variable name                |     Example         | Description       |
| ----------------------       |     :-----:         | ---------------   |
| kafka.sasl.enabled           | true | If kafka authentication is enabled |
| kafka.sasl.password          | kafkapass | Kafka connection password   |
| kafka.sasl.username          | kafkauser | Kafka connection username   |
| spring.datasource.password   | infrabe | Password for infrabe database |
| spring.datasource.username   | infrabe | Username for infrabe database |
| spring.mail.password         | password | Password to ionos smtp       |
| spring.mail.username         | infrabe | Username to ionos smtp        |

##### Secret for EDC

One secret is needed, its name is "*namespace*-simpl-edc", it should be created in created before kv secret engine.

```
{
  "contractmanager_apikey": "apikey",
  "edc_datasource_default_password": "edc",
  "edc_datasource_policy_password": "edc",
  "edc_ionos_access_key": "accesskeystring",
  "edc_ionos_endpoint": "s3-eu-central-1.ionoscloud.com",
  "edc_ionos_endpoint_region": "de",
  "edc_ionos_secret_key": "secretkeystring",
  "edc_ionos_token": "tokenstring"
}
```

| Variable name                    |     Example         | Description              |
| ----------------------           |     :-----:         | ---------------          |
| contractmanager_apikey           | apikey              | Apikey string            |
| edc_datasource_default_password  | edc                 | Password for infrabe database  |
| edc_datasource_policy_password   | edc                 | Link to datasource       |
| edc_ionos_access_key             | accesskeystring     | Access key for S3        |
| edc_ionos_endpoint               | s3-eu-central-1.ionoscloud.com | S3 server url |
| edc_ionos_endpoint_region        | de                  | Two letter country code  |
| edc_ionos_secret_key             | secretkeystring     | Secret key for S3        |
| edc_ionos_token                  | tokenstring         | Token for S3 access      |

### Deployment

#### Deployment using ArgoCD

You can easily deploy the agent using ArgoCD. All the values mentioned in the sections below you can input in ArgoCD deployment. The repoURL gets the package directly from code.europa.eu.
targetRevision is the package version. 

When you create it, you set up the values below (example values)

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: 'dataprovider01-deployer'           # name of the deploying app in argocd
spec:
  project: default
  source:
    repoURL: 'https://code.europa.eu/api/v4/projects/904/packages/helm/stable'
    path: '""'
    targetRevision: 1.2.2                   # version of package
    helm:
      values: |
        values:
          branch: v1.2.2                    # branch of repo with values - for released version it should be the release branch
        project: default
        namespaceTag: dataprovider01        # identifier of deployment and part of fqdn
        domainSuffix: int.simpl-europe.eu   # last part of fqdn
        argocd:
          appname: dataprovider01           # name of generated argocd app 
          namespace: argocd                 # namespace of your argocd
        cluster:
          address: https://kubernetes.default.svc
          namespace: dataprovider01         # where the app will be deployed
          commonToolsNamespace: common      # namespace where main monitoring stack is deployed
          issuer: dev-prod                  # issuer of certificates
        hashicorp:
          service: "http://vault-common.common.svc.cluster.local:8200"  # local service path to your vault
          token: "hvs.string"               # token to access the vault
          tokenEncoded: "!@#$%^&*()qwertyuiopasdfghjklzxcvbnm!@#$%^&*()"  # the same token but base64 encoded
          role: dev-int-role                # role created in vault for access
          secretEngine: dev-int             # secret engine name created in vault
        authority:
          namespaceTag: authority1          # namespace tag of target authority
        crossplane:
          enabled: true                     # if infrastructure components should be deployed (there can be only one instance per cluster)
          gitea:
            username: user                  # username for gitea
            password: pass                  # password for gitea
          kafka:
            username: user                  # username to connect to kafka
            password: pass                  # password to connect to kafka
          ionos:
            SMTPpassword: "pass"            # password for smtp in ionos
            token: "tokenstring"            # token to access ionos
          infrastructure_be:
            database:
              username: infrabe             # username to access postgres for infra-be
              password: infrabe             # password to access postgres for infra-be
    chart: data-provider
  destination:
    server: 'https://kubernetes.default.svc'
    namespace: dataprovider01               # where the package will be deployed
```

#### Manual deployment

##### Files preparation

Another way for deployment, is to unpack the released package to a folder on a host where you have kubectl and helm available and configured. 

There is basically one file that you need to modify - values.yaml. 
There are a couple of variables you need to replace - described below. The rest you don't need to change.

```
project: default                    # Project to which the namespace is attached
namespaceTag: dataprovider01        # identifier of deployment and part of fqdn
authority:
  namespaceTag: authority1          # namespace tag of target authority
domainSuffix: int.simpl-europe.eu   # last part of fqdn

argocd:
  appname: dataprovider01           # name of generated argocd app 
  namespace: argocd                 # namespace of your argocd

cluster:
  address: https://kubernetes.default.svc
  namespace: dataprovider01         # where the package will be deployed
  commonToolsNamespace: common      # namespace where main monitoring stack is deployed
  issuer: dev-prod                  # issuer of certificates

hashicorp:
  service: "http://vault-ha.vault-ha.svc.cluster.local:8200"  # local service path to your vault
  token: "hvs.string"               # token to access the vault
  tokenEncoded: "!@#$%^&*()qwertyuiopasdfghjklzxcvbnm!@#$%^&*()"  # the same token but base64 encoded
  role: dev-int-role                # role created in vault for access
  secretEngine: dev-int             # secret engine name created in vault

crossplane:
  enabled: true                     # if infrastructure components should be deployed (there can be only one instance per cluster)
  gitea:
    username: user                  # username for gitea
    password: pass                  # password for gitea
  kafka:
    username: user                  # username to connect to kafka
    password: pass                  # password to connect to kafka
  ionos:
    SMTPpassword: "pass"            # password for smtp in ionos
    token: "tokenstring"            # token to access ionos
  infrastructure_be:
    database:
      username: infrabe             # username to access postgres for infra-be
      password: infrabe             # password to access postgres for infra-be

values:
  repo_URL: https://code.europa.eu/simpl/simpl-open/development/agents/data-provider.git  # repo URL
  branch: v1.2.2                    # branch of repo with values - for released version it should be the release branch
```

##### Deployment

After you have prepared the values file, you can start the deployment. 
Use the command prompt. Proceed to the folder where you have the Chart.yaml file and execute the following command. The dot at the end is crucial - it points to current folder to look for the chart. 

Now you can deploy the agent:

`helm install data-provider . `

## Additional steps

In the current version, the automatic onboarding process has already been implemented using: init-participant-job.
For this reason, manual onboarding activities are no longer necessary.

### Monitoring

Filebeat components for monitoring are included in this release.   
Their deployment can be disabled by switching the value monitoring.enabled to false.

# Troubleshooting
If you encounter issues during deployment, check the following:

- Ensure that ArgoCD is properly set up and running.
- Verify that the namespace exists in your Kubernetes cluster.
- Check the ArgoCD application logs and Helm error messages for specific issues.
