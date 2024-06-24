# Install zabbix agents on kubernetes downstream cluster

## Add repo for zabbix helm chart
```sh
helm repo add zabbix-chart-7.0  https://cdn.zabbix.com/zabbix/integrations/kubernetes-helm/7.0
helm repo update
```

## Create values.yaml file
```sh
helm show values zabbix-chart-7.0/zabbix-helm-chrt > values.yaml
```
## Modify values.yaml file

Modify the values.yaml file variables zabbixProxy.env.ZBX_SERVER_HOST
```sh
nano values.yaml
```

## Create namespace
```sh
kubectl create namespace monitoring
```

## Install zabbix using Helm chart
```sh
helm install zabbix zabbix-chart-7.0/zabbix-helm-chrt -n monitoring -f values.yaml
```


## Verify the zabbix installation
```sh
kubectl get pods -n monitoring
```

## Get zabbix token
```sh
kubectl get secret zabbix-service-account -n monitoring -o jsonpath={.data.token} | base64 -d
```


# Zabbix server config

## Create a host
```
Data Collection -> Host
Create host
hostname: master
template: Kubernetes cluster state by http
host groups: kubernetes
Add
```
## Add macros to the host
```
Data colletion -> host
Macros
{$KUBE.API.URL}  https://10.63.16.100:6443
{$KUBE.API.TOKEN} verylongstring
Update
```


# References:
- https://medium.com/@abdulemes/this-tutorial-describes-how-to-setup-monitoring-for-your-kubernetes-cluster-using-zabbix-1035d3fbbf16
- https://git.zabbix.com/projects/ZT/repos/kubernetes-helm/browse?at=refs%2Fheads%2Frelease%2F7.0
- https://www.maquinasvirtuales.eu/monitorizar-cluster-kubernetes-en-zabbix-7-lts/
