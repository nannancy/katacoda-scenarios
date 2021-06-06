# Install Prometheus Operator in k3s
## Install k3s
curl -sfL https://get.k3s.io | sh -
`curl -sfL https://get.k3s.io | sh -
`{{execute}}

## Install Helm
```curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 &&  chmod 700 get_helm.sh &&./get_helm.sh```{{execute}}

### Setup KubeConfig env
`export KUBECONFIG=/etc/rancher/k3s/k3s.yaml`{{execute}}
## Install Prometheus Operator
````
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
helm repo update
helm install prometheus prometheus-community/kube-prometheus-stack
```{{execute}}

