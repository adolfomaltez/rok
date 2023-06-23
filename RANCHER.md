# Install Rancher on kind k8s cluster

## Requirements:
- kind
- helm
- kubectl

## Create kind cluster with extra paths.

```sh
kind create cluster --name=rancher --config=cluster.yaml
```
## Apply CRDs: nginx, cert-manager

```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

## Install cert-manager (helm)


### Apply CRDs
```sh
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.12.1/cert-manager.crds.yaml
```

### Add the Jetstack Helm repository
```sh
helm repo add jetstack https://charts.jetstack.io
```

### Update your local Helm chart repository cache
```sh
helm repo update
```

### Install the cert-manager Helm chart
```sh
kubectl create namespace cert-manager
helm install cert-manager jetstack/cert-manager \
      --namespace cert-manager \
      --version v1.12.1
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
  --set hostname=rancher.192-168-10-12.sslip.io \
  --set bootstrapPassword=admin \
  --set replicas=1 \
  --version=2.7.4 \
  --set global.cattle.psp.enabled=false
```

### Alternative: letsEncrypt certificate
#helm install rancher rancher-stable/rancher \
#  --namespace cattle-system \
#  --set hostname=192-168-10-12.sslip.io \
#  --set bootstrapPassword=admin \
#  --set replicas=1  \
#  --version=2.7.4   \
#  --set global.cattle.psp.enabled=false   \
#  --set ingress.tls.source=letsEncrypt \
#  --set letsEncrypt.email=adolfomaltez@gmail.com \
#  --set letsEncrypt.ingress.class=nginx



## Create rancher API token
Change rancher admin password, create API token.




## References:
- https://kind.sigs.k8s.io/docs/user/quick-start/
- https://kind.sigs.k8s.io/docs/user/ingress/#ingress-nginx
- https://ranchermanager.docs.rancher.com/pages-for-subheaders/install-upgrade-on-a-kubernetes-cluster
