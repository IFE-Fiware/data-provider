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

## Installation

The deployment is based on master helm chart which, when applied on Kubernetes cluster, should deploy the Data Provider to it using ArgoCD. 

### Files preparation

The suggested way for deployment, is to unpack the released package to a folder on a host where you have kubectl and helm available and configured. 

There is basically one file that you need to modify - values.yaml. 
There are a couple of variables you need to replace. 

| Variable name          |     Example         | Description     |
| ---------------------- |     :-----:         | --------------- |
| ${NAMESPACE}           | dataprovider01-iaa  | Namespace on the cluster, also used as ArgoCD application name |
| ${NAMESPACETAG}        | dataprovider01      | Namespace tag - that will be in FQDN differentiating the deployment, used for example in participant.be.**dataprovider01**.int.simpl-europe.eu  |
| ${DOMAINSUFFIX}        | int.simpl-europe.eu | Domain suffix in FQDN, used for example in participant.be.dataprovider01.**int.simpl-europe.eu** |

You might also need to modify (if needed) the values in keys:

| Variable key           |     Example         | Description     |
| ---------------------- |     :-----:         | --------------- |
| values.repo_URL        | https://....git     | URL for repo with app-values folder |
| values.branch          | develop             | Branch of this repo to use          |

### Manual steps

Two volumes needs to be created manually at the moment:
* nfs-storage-pvc-xsfc
* nfs-storage-pvc-sdapibe

This will be fixed in future versions.

In values of TLS Gateway you need to input the base64 encoded keystore and truststore, with their passwords, in those keys:

```
ssl:
  keyStore:
    base64: "yourbase64keystore"
    password: "yourpassword"
  trustStore:
    base64: "yourbase64truststore"
    password: "yourpassword"
```

### Deployment

After you have prepared the values file, you can start the deployment. 
Use the command prompt. Proceed to the folder where you have the Chart.yaml file and execute the following command. The dot at the end is crucial - it points to current folder to look for the chart. 

`helm install data-provider .`

After it's been already deployed and you want to implement changes, you can use this command (in the same folder):

`helm upgrade data-provider .`

### Removal

To remove the deployment you need to do two things:

* `helm uninstall data-provider` - which will remove the application from argocd
* `kubectl delete ns ${NAMESPACE}` - which will remove the namespace ${NAMESPACE} that holds all of the components.