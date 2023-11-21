# minio installation on k8s cluster

# Install minio helm repository
```sh
helm repo add minio https://operator.min.io/
helm repo update

helm install --namespace minio-operator --create-namespace minio-operator minio/operator

kubectl get pods -n minio-operator
kubectl apply -f - <<EOF
apiVersion: v1
kind: Secret
metadata:
  name: console-sa-secret
  namespace: minio-operator
  annotations:
    kubernetes.io/service-account.name: console-sa
type: kubernetes.io/service-account-token
EOF

kubectl -namespace  minio-operator get secret console-sa-secret -o jsonpath="{.data.token}" | base64 --decode

kubectl --namespace minio-operator port-forward svc/console 9090:9090 --address='0.0.0.0'
```


# Download and edit the values.yaml file for tenant
```sh
wget https://github.com/minio/operator/blob/master/helm/tenant/values.yaml
nano values.yaml
```


# Install minio instance for tenant
```sh
helm install --namespace tenant-ns --create-namespace tenant minio/tenant

# Dashboard
kubectl --namespace tenant-ns port-forward svc/myminio-console 9443:9443 --address='0.0.0.0'


# Create key on minio, 
kubectl create secret generic minio-rancher-key     --from-literal=accessKey=asdfasdfasdf     --from-literal=secretKey=asdfasdfasdfasdfasdfasdf

# Tenant API endpoint
kubectl --namespace tenant-ns port-forward svc/myminio-hl 9000:9000 --address='0.0.0.0'
```


# [ PENDING ] Create the ingress



# Creating a tenant
```sh
helm install --namespace tenant-ns \
  --create-namespace tenant minio/tenant -f values.yaml
```

# Reference
- https://github.com/minio/operator/tree/master/helm/operator
