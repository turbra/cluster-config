# GitOps cluster-config

This repository contains the configurations I utilize for my OpenShift clusters, taking cues from Gerald Nunn's [Standards](https://github.com/gnunn-gitops/standards). 

For more insights into OpenShift GitOps and Kustomize, check out Mark Roberts blog post [Your Guide to Continuous Delivery with OpenShift GitOps and Kustomize](https://cloud.redhat.com/blog/your-guide-to-continuous-delivery-with-openshift-gitops-and-kustomize).

# How it works

Configuration for individual clusters is located in `bootstrap/clusters/overlays`. To deploy a cluster's configuration, set up an Argo CD application in `components/argocd/{appconfig}` targeting `bootstrap/clusters/overlays/{clustername}`.

The `components/apps` directory houses the `cluster-config` and `operators` subdirectories. These subdirectories store custom configurations for tasks such as day2 operations setup and the deployment of operators. Meanwhile, the `components/argocd` directory holds configurations that will showcase your `apps` and/or `operators` (as referenced earlier) within ArgoCD.


# Install OpenShift GitOps

```
oc apply -k manual
```
## Workflow summary
  
  When you run the command `oc apply -k manual`, the following happens:

  1. `oc` (OpenShift CLI) recognizes the `-k` flag and invokes Kustomize on the `manual/` directory.
  2. Kustomize reads the `kustomization.yaml` file to determine which resources to include and any modifications to make.
  3. The resources defined in `openshift-gitops-crb.yaml` and `openshift-gitops-subscription.yaml` are processed and prepared for application to the cluster.
  4. The processed resources are applied to the OpenShift cluster. This results in:
     - The necessary permissions being granted via the ClusterRoleBinding.
     - The GitOps Operator being installed or updated based on the Subscription.

# Add/Enable applications in app-of-apps

```
vi clusters/{cluster-name}/kustomization.yaml
```
## Workflow summary

  When you apply the configurations using Kustomize:

  - **Initialization**: Kustomize begins by reading the `kustomization.yaml` file in the `clusters/{cluster-name}/` directory.

  - **Resource Aggregation**: Based on the `resources` list in the `kustomization.yaml` file, Kustomize aggregates configurations from multiple locations:
    ```yaml 
    ../../components/argocd/acs # This includes configurations related to the deployment of the ACS Operator.
    ../../components/argocd/banner/overlays/{cluster-name} # This brings in configurations specific to the `{cluster-name}` overlay for banners.
    ../../components/argocd/console-link/overlays/{cluster-name} # This integrates configurations for console links in the `{cluster-name}` overlay.
    ../../components/argocd/defrag-etcd # This adds configurations related to defragmenting etcd.
    ```

  - **Configuration Processing**: Kustomize processes all the aggregated configurations, potentially applying patches, variable substitutions, and other transformations as defined elsewhere in 
  the `kustomization.yaml` or associated files.

  - **Application**: Once processed, these configurations are ready to be applied to the OpenShift cluster, setting up the specific components and configurations for the `{cluster-name}` 
  cluster as defined.


# Install app-of-apps

```
oc apply -k bootstrap/clusters/overlays/{cluster-name}
```
## Workflow Summary

When you run the above command for a specific `{cluster-name}`:

- **Initialization**: The `oc` (OpenShift CLI) detects the `-k` flag, prompting it to use Kustomize on the `bootstrap/clusters/overlays/{cluster-name}/` directory.

- **Resource Aggregation**: Kustomize reads the `kustomization.yaml` file located in the `bootstrap/clusters/overlays/{cluster-name}/` directory. This file will list resources, patches, and other configurations specific to the chosen cluster.

- **Configuration Processing**: Based on the `resources` list and other directives in the `kustomization.yaml` file, Kustomize aggregates and processes configurations. This might involve:
  - Incorporating base configurations and applying overlays specific to `{cluster-name}`.
  - Applying patches, variable substitutions, and other transformations as defined in the `kustomization.yaml` or associated files.

- **Application**: The processed configurations are then applied to the OpenShift cluster. This sets up the cluster with configurations tailored for `{cluster-name}`, ensuring it has the necessary components and settings.


# Application tiles
![alt text](https://github.com/turbra/cluster-config/blob/lab/docs/img/argotiles.png)

## App of Apps
![alt text](https://github.com/turbra/cluster-config/blob/lab/docs/img/appofapps.png)
