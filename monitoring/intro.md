# Introduction

This scenario is aimed to publish custom metrics using Prometheus operator, Grafana on Kubernetes. To start with, we will write a python app to scrape a custom metric from, for example, a sensor which detects home temperature and humidity. Next we configure a kubernetes cluster to deploy the application and prometheus integrated with Grafana. The sub goal of this scenario is getting us
familiar with the following technologies:
 1. package application using Docker
 2. publish prometheus-style metrics using prometheus-client python library
 3. configure prometheus operator in a kubernetes cluster [ref: prometheus-operator github repository](https://github.com/prometheus-operator/prometheus-operator)
 4. show metrics in a dashboard using Grafana