# Install Crossplane

```sh
kind get clusters

helm repo list
helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo update

helm install crossplane --namespace crossplane-system --create-namespace crossplane-stable/crossplane
kubectl  get all -n crossplane-system
```

# References
- https://docs.crossplane.io/v1.13/software/install
