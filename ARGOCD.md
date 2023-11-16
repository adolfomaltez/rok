# Install ArgoCD on Kubernetes
```sh
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## PortForwarding
```sh
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

[Localhost](http://localhost:8080/)



# References
- [ArgoCD Getting Started](https://argo-cd.readthedocs.io/en/stable/getting_started/)
- [Install ArgoCD CLI](https://argo-cd.readthedocs.io/en/stable/cli_installation/)