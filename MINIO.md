# minio installation on k8s cluster

# Add minio helm repository
```sh
helm repo add minio https://operator.min.io/
helm repo update
```

# Install minio
```sh
helm install \
  --namespace minio-operator \
  --create-namespace \
  minio-operator minio/operator
# Run the output commands to get the token
```

# [ PENDING ] Create the ingress


# Download and edit the values.yaml file for tenant
```sh
wget https://github.com/minio/operator/blob/master/helm/tenant/values.yaml
nano values.yaml
```

# Creating a tenant
```sh
helm install --namespace tenant-ns \
  --create-namespace tenant minio/tenant -f values.yaml
```

# Reference
- https://github.com/minio/operator/tree/master/helm/operator
