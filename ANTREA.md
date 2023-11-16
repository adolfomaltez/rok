

# Patch on kind-setup.sh
```yaml
- role: control-plane
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: TCP
  - containerPort: 443
    hostPort: 443
    protocol: TCP
```

# Download Antrea docker image
```sh
docker pull projects.registry.vmware.com/antrea/antrea-ubuntu:v1.13.1
./kind-setup.sh --images projects.registry.vmware.com/antrea/antrea-ubuntu:v1.13.1 create cluster-01
```

# Install antrea
```sh
kubectl apply -f https://github.com/antrea-io/antrea/releases/download/v1.13.1/antrea.yml
```

# Label nodes
```sh
kubectl get nodes --show-labels
kubectl label nodes cluster-01-worker ingress-ready=true
kubectl label nodes cluster-02-worker ingress-ready=true
```

# Install nginx
```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

# Install cert-manager
```sh
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.1/cert-manager.crds.yaml
```

### Add the Jetstack Helm repository
```sh
helm repo add jetstack https://charts.jetstack.io
```

### Update your local Helm chart repository cache
```sh
helm repo update
```

# Install certmanager
```sh
kubectl create namespace cert-manager
helm install cert-manager jetstack/cert-manager \
      --namespace cert-manager \
      --version v1.13.1
```

## Install Rancher

### Add the Rancher Helm repository
```sh
helm repo add rancher-stable https://releases.rancher.com/server-charts/stable
```

### Update your local Helm chart repository cache
```sh
helm repo update
```

### Create namespace
```sh
kubectl create namespace cattle-system
```

### Install the Rancher Helm chart
```sh
helm install rancher rancher-stable/rancher \
  --namespace cattle-system \
  --set hostname=192-168-31-13.sslip.io \
  --set bootstrapPassword=admin \
  --set replicas=2 \
  --version=2.7.6 \
  --set global.cattle.psp.enabled=false
```
