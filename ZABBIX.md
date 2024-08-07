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
{$KUBE.API.URL}      https://kubernetes.default.svc.cluster.local:443
{$KUBE.API.TOKEN}    zabbixtokenlongstring
{$KUBE.STATE.ENDPOINT.NAME} cluster01
```

## Using zabbix API to create proxy
Create API token (User settings -> API tokens)
```sh
# Export ZABBIX TOKEN
export AUTHORIZATION_TOKEN="123456789abcdef"
```
Example query
```sh
# Authentication
curl --request POST   --url 'http://192.168.31.94/api_jsonrpc.php' --header 'Content-Type: application/json-rpc'  --data '{"jsonrpc":"2.0","method":"user.login","params":{"username":"Admin","password":"zabbix"},"id":1}'

# Authorization
curl --request POST --url 'http://192.168.31.94/api_jsonrpc.php'   --header 'Authorization: Bearer 123456123456'

# Create the zabbix-proxy
curl --request POST --url 'http://192.168.31.94/api_jsonrpc.php'  --header "Authorization: Bearer ${AUTHORIZATION_TOKEN}" --header 'Content-Type: application/json-rpc' --data '{"jsonrpc":"2.0","method":"proxy.create","params":{"name":"zabbix-proxy","operating_mode":"0", "allowed_addresses":"0.0.0.0/0", "description":"kubernetes cluster01 proxy"},"id":1}'

# Create host group
curl --request POST --url 'http://192.168.31.94/api_jsonrpc.php'  --header "Authorization: Bearer ${AUTHORIZATION_TOKEN}" --header 'Content-Type: application/json-rpc' --data '{"jsonrpc":"2.0","method":"hostgroup.create","params":{"name":"cluster01"},"id":1}'

# Create the host: Cluster State
curl --request POST --url 'http://192.168.31.94/api_jsonrpc.php'  --header "Authorization: Bearer ${AUTHORIZATION_TOKEN}" --header 'Content-Type: application/json-rpc' --data '{"jsonrpc":"2.0","method":"host.create","params":{"host":"cluster01-state","groups":[{"groupid":"22"}], "templates":[{"templateid":"10510"}], "description":"cluster01 k8s state", "monitored_by":1, "proxyid":"1"},"id":1}'

# Create the host: Nodes
curl --request POST --url 'http://192.168.31.94/api_jsonrpc.php'  --header "Authorization: Bearer ${AUTHORIZATION_TOKEN}" --header 'Content-Type: application/json-rpc' --data '{"jsonrpc":"2.0","method":"host.create","params":{"host":"cluster01-nodes","groups":[{"groupid":"22"}], "templates":[{"templateid":"10507"}], "description":"cluster01 nodes state", "monitored_by":1, "proxyid":"1"},"id":1}'
```



# Reference:
- https://git.zabbix.com/projects/ZT/repos/kubernetes-helm/browse?at=refs%2Fheads%2Frelease%2F7.0
- https://www.youtube.com/watch?v=2dPFvJrK9Mw
- https://medium.com/@abdulemes/this-tutorial-describes-how-to-setup-monitoring-for-your-kubernetes-cluster-using-zabbix-1035d3fbbf16
- https://www.zabbix.com/documentation/current/en/manual/api
- https://www.zabbix.com/documentation/current/en/manual/api/reference/proxy/object#proxy
