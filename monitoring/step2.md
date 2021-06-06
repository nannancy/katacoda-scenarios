# Install Prometheus Operator in k3s
## Install k3s
`sudo su - && curl -sfL https://get.k3s.io | sh -
`{{execute}}

## Install Helm
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```{{execute}}

### Setup KubeConfig env
`export KUBECONFIG=/etc/rancher/k3s/k3s.yaml`{{execute}}
## Install Prometheus Operator
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
helm repo update
helm install prometheus prometheus-community/kube-prometheus-stack
```{{execute}}
# Visit prometheus and Grafana Web UI
`kubectl get deployment`{{execute}}

See the following:

````
NAME                                  READY   UP-TO-DATE   AVAILABLE   AGE
prometheus-kube-prometheus-operator   1/1     1            1           2d5h
prometheus-kube-state-metrics         1/1     1            1           2d5h
prometheus-grafana                    1/1     1            1           2d5h
pysmarthome                           1/1     1            1           2d9h
````

prometheus-grafana is deployed.

`nohup kubectl port-forward deployment/prometheus-grafana 3000 $`

## Generated Web Link
To enter grafana, we need the following information:
Default user: admin
password: prom-operator
https://[[HOST_SUBDOMAIN]]-3000-[[KATACODA_HOST]].environments.katacoda.com

There is also Prometheus UI:
`nohup kubectl port-forward prometheus-prometheus-kube-prometheus-prometheus-0  9090`{{execute}}
https://[[HOST_SUBDOMAIN]]-9090-[[KATACODA_HOST]].environments.katacoda.com

In next step, we deploy our container image `pysmarthome` into the cluster and create a service monitor, making Prometheus pulling metrics from `pysmarthome`



