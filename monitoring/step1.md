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
@app.route("/metrics")
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

Install python dependencies:

```echo "flask" >> requirements.txt \ 
echo "prometheus_client">> requirements.txt \ 
python3 -m pip install --upgrade pip && pip install requirements.txt```{{execute}}

Run the web service:

```python3 app.py```{{execute}}


