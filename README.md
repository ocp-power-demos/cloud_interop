# Cloud Interoperability 

This repository contains yaml files that shall be consumed on OCP/K8 for Cloud Interoperability.
The docker images involved are custom built for this particular usecase and leverages manifest that identifies
underlying architecture(s) accordingly and pull respective docker image.

It also demonstrates ease of moving from Public Cloud traditional x86_64 (like GCP,AWS) to 
powerful IBM POWER Systems hosted in Hybrid/Private Clouds ( like Openshift Container Platform)

### Steps to deploy the MongoDB + NodeJS Application on OpenShift Container Platform v3.11-
```
oc create -f mong-service.yaml 
oc create -f node-service.yaml
oc create -f mong-deployment.yaml 
oc create -f node-deployment.yaml 
```
Upon Successfully creating respective Service/Deployment you will see the Output Similar to this -

```
[root@p230n134 cloud_interop]# oc create -f mong-service.yaml 
service/mong created
[root@p230n134 cloud_interop]# oc create -f node-service.yaml 
service/node created
[root@p230n134 cloud_interop]# oc create -f mong-deployment.yaml 
deployment.extensions/mong created
[root@p230n134 cloud_interop]# oc create -f node-deployment.yaml 
deployment.extensions/node created
[root@p230n134 cloud_interop]# 
```

To check the status of pods in your desired namespace issue the following
```
[root@p230n134 cloud_interop]# oc get po -n mithun
NAME                    READY     STATUS    RESTARTS   AGE
mong-6f6cbff4fb-q4ds6   1/1       Running   0          1m
node-b6f55bdb9-rpszs    1/1       Running   0          1m
[root@p230n134 cloud_interop]#
```
Where `-n mithun` is our targeted namespace

#####Todo
How to create a route and test how the endpoint exposed from NodeJS Container hits the Mongodb Container
and indeed pulls in the data.
