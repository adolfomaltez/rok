# Install Elemental on Rancher mgmt cluster


## Install elemental operator on k8s cluster
```sh
helm upgrade --create-namespace -n cattle-elemental-system \
  --install elemental-operator \
  oci://registry.opensuse.org/isv/rancher/elemental/stable/charts/rancher/elemental-operator-chart
```

```sh
kubectl get pods -n cattle-elemental-system
```

## Referencias
- https://elemental.docs.rancher.com/quickstart-ui