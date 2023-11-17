```sh
# Add harbor repository to helm
helm repo add harbor https://helm.goharbor.io
helm repo update
```

```sh
# Download values files for harbor
https://raw.githubusercontent.com/goharbor/harbor-helm/v1.13.1/values.yaml
```

```sh
# Edit for changes: hostname, ingress, etc.
nano values.yaml
```

```sh
# deploy harbor
helm install harbor harbor/harbor --version=1.13.1 --namespace harbor --create-namespace -f values.yaml 
```

# References
- https://raw.githubusercontent.com/goharbor/harbor-helm/v1.13.1/values.yaml 
