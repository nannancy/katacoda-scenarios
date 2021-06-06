# a python app

A Katacoda scenario is a series of Markdown files, bash scripts and a JSON file to define how your scenario should be configured, the text for the scenario and any automation required.

## Create an image

Clone our example repository that contains the set of documentation with the following command:

`mkdir pysmarthome && cd pysmarthome && touch app.py{{execute}}
```
cat << EOF > /tmp/storageos-secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: "storageos-api"
  namespace: "storageos-operator"
  labels:
    app: "storageos"
type: "kubernetes.io/storageos"
EOF
```{{execute}}
```
