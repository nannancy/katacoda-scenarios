# Introduction

The goal of this scenario is to run a prometheus-installed kubernetes cluster, getting visualised cluster-related metrics as well as custom metrics. To start with, we will write a python app to scrape a custom metric from, for example, a sensor which detects home temperature and humidity. Next we configure a kubernetes cluster to deploy the application and prometheus integrated with Grafana. The sub goal of this scenario is getting us
familiar with the following technologies:
 1. package application using Docker
 2. publishing prometheus-style metrics using prometheus-client python library
 3. configure prometheus operator in a kubernetes cluster [ref: prometheus-operator github repository](https://github.com/prometheus-operator/prometheus-operator)
 4. visualisation using Grafana
