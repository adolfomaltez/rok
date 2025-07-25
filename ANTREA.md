

# Installing k8s cluster witn kind and Antrea CNI

## Clone Antrea git repository
```sh
git clone https://github.com/antrea-io/antrea.git
git checkout release-2.4
```

# Add to ci/kind/kind-setup.sh
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

## Pull Antrea docker image
```sh
docker pull antrea/antrea-agent-ubuntu:v2.4.0
docker pull antrea/antrea-controller-ubuntu:v2.4.0
```

## Create k8s cluster
```sh
# Create k8s cluster using kind and push antrea image to nodes
./ci/kind/kind-setup.sh create cluster
kind load docker-image antrea/antrea-agent-ubuntu:v2.4.0  --name cluster
kind load docker-image antrea/antrea-controller-ubuntu:v2.4.0  --name cluster
```

## Install Antrea on k8s cluster
```sh
kubectl apply -f https://github.com/antrea-io/antrea/releases/download/v2.4.0/antrea.yml
```

## Label worker nodes (for nginx ingress)
```sh
kubectl get nodes --show-labels
kubectl label nodes cluster-control-plane  ingress-ready=true
```

## Install nginx ingress
```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

## Install cert-manager CRDs
```sh
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.18.2/cert-manager.crds.yaml
```

### Add the Jetstack Helm repository
```sh
helm repo add jetstack https://charts.jetstack.io
```

### Update your local Helm chart repository cache
```sh
helm repo update
```

## Install certmanager
```sh
kubectl create namespace cert-manager
helm install cert-manager jetstack/cert-manager \
      --namespace cert-manager \
      --version v1.18.2
```

## Install Rancher

### Add the Rancher Helm repository
```sh
helm repo add rancher-latest https://releases.rancher.com/server-charts/latest
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
helm install rancher rancher-latest/rancher \
  --namespace cattle-system \
  --set hostname=rancher.192-168-31-13.sslip.io \
  --set bootstrapPassword=admin \
  --set replicas=1 \
  --version=2.11.3 \
  --set global.cattle.psp.enabled=false
```

## Open Rancher on Web Browser
- [Rancher Web Interface](https://rancher.192-168-31-13.sslip.io)
