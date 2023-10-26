# GitOps cluster-config

This repo contains cluster configurations that I use in my OpenShift clusters.  It is loosely based off of Gerald Nunn's [Standards](https://github.com/gnunn-gitops/standards).

# How it works

Configuration for individual clusters is located in `bootstrap/clusters/overlays`. To deploy a cluster's configuration, set up an Argo CD application in `components/argocd/{appconfig}` targeting `bootstrap/clusters/overlays/{clustername}`.

The `components/apps` directory houses the `cluster-config` and `operators` subdirectories. These subdirectories store custom configurations for tasks such as day2 operations setup and the deployment of operators. Meanwhile, the `components/argocd` directory holds configurations that will showcase your `apps` and/or `operators` (as referenced earlier) within ArgoCD.



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



