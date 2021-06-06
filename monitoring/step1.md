# a python app

A Katacoda scenario is a series of Markdown files, bash scripts and a JSON file to define how your scenario should be configured, the text for the scenario and any automation required.

## Create an image

Clone our example repository that contains the set of documentation with the following command:

```mkdir pysmarthome && cd pysmarthome && touch app.py```{{execute}}

```
cat << EOF > /root/pysmarthome/app.py
#import Adafruit_DHT as adht
import time
import logging
from flask import Flask,Response
from prometheus_client import Gauge, generate_latest, CollectorRegistry

app = Flask(__name__)
registry = CollectorRegistry()
gauge = Gauge("my_gauge","show gauge",["dht11_sensor"],registry=registry)
@app.route("/")
def metrics():
    #h,t = adht.read_retry(adht.DHT11, 4)
    h = 33
    t = 24
    gauge.labels('temparature').set(t)
    gauge.labels('humidity').set(h)
    return Response(generate_latest(registry),mimetype='text/plain')
if __name__ == "__main__":
    app.run(host='0.0.0.0')
EOF
```{{execute}}

Install python dependencies and Run:

```echo "flask" > requirements.txt &&
echo "prometheus_client">> requirements.txt&&
python3 -m pip install --upgrade pip```{{execute}}


```pip install -r requirements.txt &&
python3 app.py```{{execute}}

## Generated Web Link

https://[[HOST_SUBDOMAIN]]-5000-[[KATACODA_HOST]].environments.katacoda.com


Create a Dockerfile to package the app:

```
touch Dockerfile && cat << EOF > /root/pysmarthome/Dockerfile
FROM python:3.7
MAINTAINER Xinyuan

# Creating Application Source Code Directory
RUN mkdir -p /smarthome/src

# Setting Home Directory for containers
WORKDIR /smarthome/src

# Installing python dependencies
COPY requirements.txt /smarthome/src
RUN pip install --no-cache-dir -r requirements.txt
# Copying src code to Container
COPY . /smarthome/src

# Application Environment variables
ENV APP_ENV development

# Exposing Ports
EXPOSE 5000

# Setting Persistent data
VOLUME ["/app-data"]
EOF
```{{execute}}

Build Docker image:

```docker build -f Dockerfile -t pysmarthome:v1 . ```{{execute}}

Zip the image for next step use:
```docker save --output pysmarthome:v1.tar pysmarthome:v1```{{execute}}

