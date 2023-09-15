# GitOps cluster-config

This repo contains cluster configurations that I use in my OpenShift clusters.  It is loosely based off of Gerald Nunn's [Standards](https://github.com/gnunn-gitops/standards).

# How it works

The `apps` directory houses the `cluster-config` and `operators` subdirectories. These subdirectories store custom configurations for tasks such as day2 operations setup and the deployment of operators. Meanwhile, the `argocd` directory holds configurations that will showcase your `apps` and/or `operators` (as referenced earlier) within ArgoCD.



# Application tiles
![alt text](https://raw.githubusercontent.com/caseyrobb/cluster-config/master/argotiles.png)

## App of Apps
![alt text](https://raw.githubusercontent.com/caseyrobb/cluster-config/master/appofapps.png)

# Install OpenShift GitOps

```
oc apply -k manual
```

# Add/Enable applications in app-of-apps

```
vi clusters/<cluster-name>/kustomization.yaml
```

# Install app-of-apps

```
oc apply -k bootstrap/clusters/overlays/<cluster-name>
```

# Updating Cluster Configuration

If you've made changes to the `kustomization.yaml` file for a specific cluster, it's essential to apply those updates to ensure the cluster configuration reflects the adjustments.

1. Navigate to the root directory of your repository:

   ```bash
   cd path_to_your_repository_root
   ```

2. Apply the changes using the following command, replacing `<cluster-name>` with the name of your cluster:

   ```bash
   oc apply -k clusters/<cluster-name>/
   ```

3. Monitor the output to ensure the resources are updated without any errors. If any issues arise, consult the error messages and troubleshoot accordingly.


