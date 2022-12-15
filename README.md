# GitOps cluster-config

This repo contains cluster configurations that I use in my OpenShift clusters.  It is mostly based off of Gerald Nunn's [Standards](https://github.com/gnunn-gitops/standards).

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
