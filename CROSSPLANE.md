# Install Crossplane

```sh
kind get clusters

helm repo list
helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo update

helm install crossplane --namespace crossplane-system --create-namespace crossplane-stable/crossplane
kubectl  get all -n crossplane-system
```


# CLI installation
```sh
curl -sL "https://raw.githubusercontent.com/crossplane/crossplane/master/install.sh" | sh
sudo mv crossplane /usr/local/bin
```
## Installing http-provider
```sh
crossplane xpkg install provider xpkg.upbound.io/crossplane-contrib/provider-http:v0.2.0
```


# References
- https://docs.crossplane.io/latest/software/install
- https://docs.crossplane.io/latest/cli/
- https://blog.crossplane.io/introducing-provider-http-empowering-crossplane-with-http-interactions/
