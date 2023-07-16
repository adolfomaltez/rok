# Install Rancher on kind k8s cluster

## Requirements:
- docker
```sh
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg

sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

usermod -aG docker $USER
```

- kind
```sh
# For AMD64 / x86_64
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.18.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```
- helm
```sh
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```
- kubectl
```sh
apt-get install kubernetes-client
```

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
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.12.2/cert-manager.crds.yaml
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
      --version v1.12.2
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
  --set hostname=rancher.192-168-1-22.sslip.io \
  --set bootstrapPassword=admin \
  --set replicas=1 \
  --version=2.7.5 \
  --set global.cattle.psp.enabled=false
```

### Alternative: letsEncrypt certificate
```sh
#helm install rancher rancher-stable/rancher \
#  --namespace cattle-system \
#  --set hostname=rancher.192-168-10-12.sslip.io \
#  --set bootstrapPassword=admin \
#  --set replicas=1 \
#  --version=2.7.4 \
#  --set global.cattle.psp.enabled=false \
#  --set ingress.tls.source=letsEncrypt \
#  --set letsEncrypt.email=user@mail.net \
#  --set letsEncrypt.ingress.class=nginx
```


## Create rancher API token
Change rancher admin password, create API token.




## References:
- https://docs.docker.com/engine/install/debian/#install-using-the-repository
- https://kind.sigs.k8s.io/docs/user/quick-start/
- https://kind.sigs.k8s.io/docs/user/quick-start/#installation
- https://kind.sigs.k8s.io/docs/user/ingress/#ingress-nginx
- https://ranchermanager.docs.rancher.com/pages-for-subheaders/install-upgrade-on-a-kubernetes-cluster
- https://helm.sh/docs/intro/install/
