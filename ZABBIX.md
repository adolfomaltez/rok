# Install zabbix agents on kubernetes downstream cluster

```sh
helm repo add zabbix-chart-7.0  https://cdn.zabbix.com/zabbix/integrations/kubernetes-helm/7.0
```

## Generate values.yaml file:
```sh
helm show values zabbix-chart-7.0/zabbix-helm-chrt > values.yaml
```

## Modify the values.yaml file variables zabbixProxy.env.ZBX_SERVER_HOST to the zabbix server IP
```sh
nano values.yaml
```

# Create namespace
```sh
kubectl create namespace zabbix
```
# Install zabbix via helm chart
```sh
helm install zabbix zabbix-chart-7.0/zabbix-helm-chrt -n zabbix  -f values.yaml
```
## Verify the zabbix installation
```sh
kubectl get pods -n zabbix
```

## Get zabbix token
```sh
kubectl get secret zabbix-service-account -n zabbix -o jsonpath={.data.token} | base64 -d
```

# Zabbix server configuration

## Create zabbix-proxy
Administration -> Proxies -> Create Proxy
```
Proxyname: zabbix-proxy
Proxy mode: Active
Proxy adress: 0.0.0.0/0
Description: Kubernetes
```

## Create host
Data Collection -> Host -> Create Host
```
Host name: cluster01
Template: Kubernetes cluster state by HTTP
Host groups: cluster01
Description: cluster01
Monitored by: Proxy
zabbix-proxy
Enabled

Add
```
## Add macros to host
On the Host Macros Tab
```
{$KUBE.API.URL}      https://10.63.16.100:6443
{$KUBE.API.TOKEN}    zabbixtokenlongstring
{$KUBE.STATE.ENDPOINT.NAME} cluster01
```


# Reference:
- https://git.zabbix.com/projects/ZT/repos/kubernetes-helm/browse?at=refs%2Fheads%2Frelease%2F7.0
- https://medium.com/@abdulemes/this-tutorial-describes-how-to-setup-monitoring-for-your-kubernetes-cluster-using-zabbix-1035d3fbbf16
- https://www.maquinasvirtuales.eu/monitorizar-cluster-kubernetes-en-zabbix-7-lts/
