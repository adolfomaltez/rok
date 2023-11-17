# Rancher Backup operator


```sh
# Add helm charte repository
helm repo add rancher-charts https://charts.rancher.io
helm repo update
```

```sh
# Install CRDs
helm install --wait \
    --create-namespace -n cattle-resources-system \
    rancher-backup-crd rancher-charts/rancher-backup-crd
```

```sh
# Install rancher-backup
helm install --wait \
    -n cattle-resources-system \
    rancher-backup rancher-charts/rancher-backup
```



# References

- https://github.com/rancher/backup-restore-operator/blob/master/README.md
