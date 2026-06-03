# Data Provider — Helm CLI Deployment

## Document Information

| | |
|---|---|
| **Scope** | Instructions for deploying the SIMPL-Open Middleware Data Provider agent from the command line using Helm and kubectl. |
| **Audience** | Platform engineers and DevOps engineers with shell access to a host configured with Helm and kubectl. |

---

> For deployment through the ArgoCD graphical interface, see [ARGOCD_DEPLOYMENT.md](ARGOCD_DEPLOYMENT.md).

## Prerequisites

Before proceeding, ensure the following requirements are met:

- **Helm 3.x** is installed. See <https://helm.sh/>.
- **kubectl** is installed and configured to communicate with the target Kubernetes cluster. See <https://kubernetes.io/docs/reference/kubectl/>.
- **ArgoCD** is installed. See <https://argo-cd.readthedocs.io/en/stable/>.
- The **Common Components** have been deployed and are healthy.
- The **Governance Authority** agent has been deployed and is operational.
- All [preliminary tasks](README.md#preliminary-tasks) (OpenBao secrets, Minio configuration) have been completed.
- The required DNS entries listed in the [main deployment guide](README.md#dns-entries) have been provisioned.

> **ArgoCD** is still required, because the manual deployment only bypasses adding the Deployer app through ArgoCD UI. ArgoCD is still necessary for deployment. The chart used in this repo still is an ArgoCD App-of-Apps generator, not a standalone Helm chart.

## Deployment Procedure

### Step 1 — Prepare the values file

Unpack the released Helm chart package to a local directory on a host where `kubectl` and `helm` are available and configured.

The primary file to modify is `values.yaml`. Replace the placeholder values listed in the table below with values specific to your environment; other fields can remain at their defaults.

> **WARNING — All values below are example placeholders.**
> They **MUST** be replaced with values specific to your environment before deploying. Deploying with the example values as-is will fail or produce an incorrect configuration.

### Values that must be replaced

| Value in example | Field(s) | What to set |
|---|---|---|
| `<dataprovider-namespace>` | `namespaceTag.dataprovider`, `argocd.appname`, `cluster.namespace` | Your chosen namespace identifier for this data provider agent |
| `<authority-namespace>` | `namespaceTag.authority` | The namespace identifier of your Governance Authority deployment |
| `<common-namespace>` | `namespaceTag.common`, `cluster.commonToolsNamespace` | The namespace identifier of your Common Components deployment |
| `<your-domain>` | `domainSuffix` | Your actual domain name |
| `default` | `project` | The ArgoCD project to which this deployment belongs |
| `v3.1.3` | `values.branch` | The Git branch corresponding to your release version |
| `example` | `secrets.secretEngine` | The name of the KV secret engine configured in your OpenBao |
| `example-role` | `secrets.role` | The name of the role configured in your OpenBao |
| `<your-issuer>` | `cluster.issuer` | Your certificate issuer name |
| `pass` | `crossplane.kafka.password` | Your Kafka password (username format: `{namespace}_infrabe`; password from `{common-namespace}-kafka-credentials` OpenBao secret) |
| `pass` | `crossplane.gitea.password` | Your Gitea password (password can be any value of your choice), username is hardcoded to **gitops_test** |

### Example values.yaml

```yaml
values:
  branch: v3.1.3                               # branch of repo with values - for released version it should be the release branch
project: default                               # project to which the namespace is attached
namespaceTag:
  dataprovider: <dataprovider-namespace>       # identifier of deployment and part of fqdn for this agent
  authority: <authority-namespace>             # identifier of deployment and part of fqdn for authority
  common: <common-namespace>                   # identifier of deployment and part of fqdn for common components
domainSuffix: <your-domain>                    # last part of fqdn
resourcePreset: default                        # set to "low" to disable requests of resources
argocd:
  appname: <dataprovider-namespace>            # name of generated argocd app
  namespace: argocd                            # namespace of your argocd
cluster:
  address: https://kubernetes.default.svc
  namespace: <dataprovider-namespace>          # where the app will be deployed
  commonToolsNamespace: <common-namespace>     # namespace where main monitoring stack is deployed
  issuer: <your-issuer>                        # issuer of certificates
secrets:
  role: example-role                           # role created in OpenBao for access
  secretEngine: example                        # secret engine name created in OpenBao
crossplane:
  kafka:
    password: pass                             # password of user {namespace}_infrabe from {common-namespace}-kafka-credentials OpenBao secret
  gitea:
    password: pass                             # password for Gitea (set to your preference)
dataprovider_infrastructure:                   
  enabled: false                               # set to true to deploy infrastructure components - that stack can be deployed only once per cluster
dataprovider_monitoring:
  enabled: true                                # set to false to disable monitoring
dataprovider_iaa:
  extraValues:
    eidas:
      enabled: true                            # enable eIDAS configuration in Keycloak, you can set it to false if not needed
```

> As there are also other "enabled" switches in values - they are only for development purposes. Monitoring is the only stack that can be disabled without causing harm to the environment, that's the reason it's the only one that's listed above.
> On the other end, dataprovider_infrastructure can be deployed only once per cluster, so if you have more dataproviders than one, be sure to have it enabled in only one of them. 

Also, additionally to what is described in the snippet above, all of the following apps deployment can be disabled, but it will affect out-of-the-box functionality. 
To do so, add the following keys to with value "false" in spec.source.helm.values:

| Field | Description |
|---|---|
| `dataprovider_iaa.extraValues.keycloak.enabled` | Disable Keycloak |
| `dataprovider_iaa.extraValues.tier1_gateway.enabled` | Disable Tier1 Gateway |
| `dataprovider_iaa.extraValues.tier2_gateway.enabled` | Disable Tier2 Gateway |
| `dataprovider_iaa.extraValues.tier2_proxy.enabled` | Disable Tier1 Proxy |
| `dataprovider_iaa.extraValues.redis.enabled` | Disable Redis |
| `dataprovider_iaa.extraValues.users_roles.enabled` | Disable Users Roles |
| `dataprovider_iaa.extraValues.users_roles_fe.enabled` | Disable Users Roles frontend |
| `dataprovider_iaa.extraValues.auth_provider.enabled` | Disable Authentication Provider |
| `dataprovider_iaa.extraValues.authentication_provider_fe.enabled` | Disable Authentication Provider frontend |
| `dataprovider_gaia_x_edc.extraValues.signer.enabled` | Disable Signer |
| `dataprovider_gaia_x_edc.extraValues.simpl_edc.enabled` | Disable EDC |
| `dataprovider_gaia_x_edc.extraValues.simpl_catalogue_client.enabled` | Disable Catalogue Client |
| `dataprovider_gaia_x_edc.extraValues.sd_ui.enabled` | Disable SD UI |
| `dataprovider_data1.extraValues.edc_connector_adapter.enabled` | Disable EDC Connector Adapter |
| `dataprovider_data1.extraValues.files.enabled` | Disable Files app |
| `dataprovider_data1.extraValues.schema_sync_adapter.enabled` | Disable Schema Sync Adapter |
| `dataprovider_data1.extraValues.sdtooling_api_be.enabled` | Disable SDTooling API |
| `dataprovider_data1.extraValues.sdtooling_validation_api_be.enabled` | Disable SDTooling Validation API |
| `dataprovider_data1.extraValues.xfsc_advsearch_be.enabled` | Disable Advsearch |
| `dataprovider_contract_billing.extraValues.contract.enabled` | Disable Contract |
| `dataprovider_contract_billing.extraValues.stubs.enabled` | Disable Stubs |
| `dataprovider_contract_billing.extraValues.signingService.enabled` | Disable Signing Service |
| `dataprovider_orchestration_platform.extraValues.dagster.enabled` | Disable Dagster |
| `dataprovider_orchestration_platform.extraValues.assetorchestrator.enabled` | Disable Asset Orchestrator |
| `dataprovider_infrastructure.extraValues.crossplane.enabled` | Disable Crossplane |
| `dataprovider_infrastructure.extraValues.infrastructure_be.enabled` | Disable Infrastructure Backend |
| `dataprovider_infrastructure.extraValues.infrastructure_fe.enabled` | Disable Infrastructure Frontend |
| `dataprovider_infrastructure.extraValues.gitea.enabled` | Disable Gitea |
| `dataprovider_infrastructure.extraValues.argocd.enabled` | Disable Infra's ArgoCD |
| `dataprovider_infrastructure.extraValues.argoevents.enabled` | Disable Argo Events |
| `dataprovider_infrastructure.extraValues.argoworkflows.enabled` | Disable Argo Workflows |
| `dataprovider_infrastructure.extraValues.fluxcd.enabled` | Disable FluxCD |
| `dataprovider_infrastructure.extraValues.provisionerresources.enabled` | Disable Provisioner |
| `dataprovider_infrastructure.extraValues.tfcontroller.enabled` | Disable Tofu Controller |
| `dataprovider_infrastructure.extraValues.eso.enabled` | Disable External Secrets |

### Step 2 — Run the Helm install command

Navigate to the directory containing the `Chart.yaml` file and execute:

```bash
helm install data-provider .
```

> **Important:** The trailing `.` is required — it tells Helm to use the chart definition in the current directory.

After starting the deployment, the expected ArgoCD applications will be created and resources will begin synchronising. Depending on your cluster configuration, this step can take **up to 30 minutes**.

## Verification

After the deployment has completed, verify that all resources are healthy:

1. Confirm the Helm release is deployed:
   ```bash
   helm list -n <dataprovider-namespace>
   ```

2. Verify that all pods are running:
   ```bash
   kubectl get pods -n <dataprovider-namespace>
   ```
   All pods should report a `Running` or `Completed` status. Investigate any pods in `CrashLoopBackOff`, `Error`, or `Pending` states.

3. Proceed with the [Onboarding](README.md#onboarding) steps described in the main deployment guide.

> **Note:** The tier2-proxy component will not become healthy until the post-deployment onboarding is complete.

## See Also

- [Main Deployment Guide (README)](README.md) — prerequisites, preliminary tasks, troubleshooting, and onboarding procedures.
- [ArgoCD UI Deployment Guide](ARGOCD_DEPLOYMENT.md) — alternative deployment method using the ArgoCD graphical interface.
