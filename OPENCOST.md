# OpenCost installation on kubernetes cluster

## Install prometheus
```sh
helm install prometheus --repo https://prometheus-community.github.io/helm-charts prometheus \
  --namespace prometheus-system --create-namespace \
  --set prometheus-pushgateway.enabled=false \
  --set alertmanager.enabled=false \
  -f https://raw.githubusercontent.com/opencost/opencost/develop/kubernetes/prometheus/extraScrapeConfigs.yaml
```

## Install OpenCost
```sh
kubectl create namespace opencost
wget https://raw.githubusercontent.com/opencost/opencost-helm-chart/main/charts/opencost/values.yaml
helm install opencost --repo https://opencost.github.io/opencost-helm-chart opencost \
  --namespace opencost -f values.yaml

```

## Acess UI
```sh
kubectl port-forward --namespace opencost service/opencost 9003 9090
```

Open URL on your web browser:
- http://localhost:9090/


## References
- https://www.opencost.io/docs/configuration/on-prem
- https://www.opencost.io/docs/installation/install
