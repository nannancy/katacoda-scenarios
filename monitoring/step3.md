# Deploy pysmarthome
## Import images into k3s

`sudo k3s ctr images import /root/pysmarthome/pysmarthome-v1.tar`{{execute}}

## Deploying pysmarthome

`touch deployment.yaml`{{execute}}

```
cat << EOF > /root/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pysmarthome
spec:
  selector:
    matchLabels:
      app: pysmarthome
  replicas: 1
  template:
    metadata:
      labels:
        app: pysmarthome
    spec:
      containers:
      - name: pysmarthome
        image: pysmarthome:v1
        imagePullPolicy: Never
        ports:
          - name: http
            containerPort: 5000
        securityContext:
          privileged: true

EOF
```{{execute}}

```
cat << EOF >> /root/deployment.yaml

---
apiVersion: v1
kind: Service
metadata:
  name: pysmarthome-service
  labels:
    app: pysmarthome
spec:
  selector:
    app: pysmarthome
  ports:
    - name: http
      protocol: "TCP"
      port: 5000
      targetPort: http
  type: LoadBalancer

EOF
```{{execute}}
Configure a ServiceMonitor:
```
cat << EOF >> /root/deployment.yaml


---
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: pysmarthome
  labels:
    release: prometheus

spec:
  selector:
    matchLabels:
      app: pysmarthome
  endpoints:
    - interval: 15s
      port: http
      path: /

EOF
```{{execute}}

`kubectl apply -f deployment.yaml`{{execute}}

Reload Prometheus:

`curl -s -XPOST localhost:9090/-/reload`{{execute}}

