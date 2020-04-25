# Cloud Interoperability 

This repository contains yaml files that shall be consumed on OCP/K8 for Cloud Interoperability.
The docker images involved are custom built for this particular usecase and leverages manifest that identifies
underlying architecture(s) accordingly and pull respective docker image.

It also demonstrates ease of moving from Public Cloud traditional x86_64 (like GCP,AWS) to 
powerful IBM POWER Systems hosted in Hybrid/Private Clouds ( like Openshift Container Platform)

### Steps to deploy the MongoDB + NodeJS Application on OpenShift Container Platform v3.11/v4.3 -

```
oc create -f mong-service.yaml 
oc create -f node-service.yaml
oc create -f mong-deployment.yaml 
oc create -f node-deployment.yaml 
```

Upon Successfully applying respective Service/Deployment config you will see the Output as following -

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

### Creating Route and accessing application outside cluster

Creating a route in OCP v3.11 -
https://docs.openshift.com/container-platform/3.11/dev_guide/routes.html#creating-routes 

Creating a route in OCP v4.3 -
There are many wasy to do it, one of them is as following
https://docs.openshift.com/container-platform/4.3/networking/configuring_ingress_cluster_traffic/configuring-ingress-cluster-traffic-ingress-controller.html#configuring-ingress-cluster-traffic-ingress-controller


### Maintainers/Contacts -
Krishna Harsha Voora
Mithun H R
