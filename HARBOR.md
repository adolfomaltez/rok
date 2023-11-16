```sh
# Add bitnami repository to helm
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

```sh
# Download values files for harbor
wget https://raw.githubusercontent.com/bitnami/charts/main/bitnami/harbor/values.yaml
```

```sh
# Edit for changes: hostname, ingress, etc.
nano values.yaml


adminPassword: "password"
externalURL: https://harbor.192-168-31-13.sslip.io
exposureType: ingress
  type: ClusterIP
    hostname: harbor.192-168-31-13.sslip.io
    tls: true
    selfSigned: true
```

```sh
# deploy harbor
helm install harbor bitnami/harbor --namespace harbor --create-namespace -f values.yaml 
```
