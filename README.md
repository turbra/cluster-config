# Install OpenShift GitOps

oc apply -k manual

# Install app-of-apps

oc apply -k bootstrap/clusters/overlays/redcomet.ca/
