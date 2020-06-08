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


### Deploying Cloud Interop via OCP4.3 WebConsole

Deploying this repository via OCP4.3 Webconsole - is pretty simple and efficient - to make your life easier we have 
encapsulated our two tier application via yaml files - that can be consumed on any Kubernetes compatible platform in our case it will be OpenShift Container Platform.

We will now look through series of steps that can be followed to deploy this application via OCP4.3's WebConsole, 
the steps listed presume you have working OCP4.3 Setup and is accessible via WebConsole.

1) Git clone this repository to your local laptop -

```
cd $HOME/Desktop/
git clone https://github.com/krishvoor/cloud_interop
cd cloud_interop
```

2) Navigate to your OCP4.3 WebConsole and Switch to Developer Mode -
![Image of Switching_Admin_Dev](https://github.com/krishvoor/cloud_interop/blob/master/pics/Switching_Admin_Dev.png)

3) Click on Add --> Select highlighted YAML icon
![Image of Selecting YAML](https://github.com/krishvoor/cloud_interop/blob/master/pics/Click_add_yaml_view.png)

4) From your Cloned Directory drop mong-service.yaml file into the WebConsole  -
![Image of Mongo_Service](https://github.com/krishvoor/cloud_interop/blob/master/pics/mongo-service.png)
Once loaded click "Create"

5) Repeat Steps outlined in Step3), drop node-service.yaml file into the WebConsole
![Image of Node_Service](https://github.com/krishvoor/cloud_interop/blob/master/pics/node-service-yaml.png)
Once loaded click "Create"

6) Repeat Steps outlined in Step3), drop mong-deployment.yaml file into the WebConsole
![Image of Mongo_Deployment](https://github.com/krishvoor/cloud_interop/blob/master/pics/mongo-deployment-yaml.png)
Once loaded click "Create"

7) Repeat Steps outlined in Step3), drop node-deployment.yaml file into the WebConsole
![Image of Node_Deployment](https://github.com/krishvoor/cloud_interop/blob/master/pics/node-deployment.png)
Once loaded click "Create"

8) Switch to Topology Section to have an overlook of your application 
![Image of Topology](https://github.com/krishvoor/cloud_interop/blob/master/pics/topology_view.png)

9) Switch to Administrator View -- 
![Image of Switching_Dev_Admin](https://github.com/krishvoor/cloud_interop/blob/master/pics/Switching_Admin_Dev.png)

10) Creating route -- From Administrator View --> Select Networking --> Routes --> Fill in the fields as neccessary and Click Create 
![Image of Creating_Route](https://github.com/krishvoor/cloud_interop/blob/master/pics/Create_route_details.png)

11) When accessed the newly created link - you will initial success page as here -

![Image of Initial Success](https://github.com/krishvoor/cloud_interop/blob/master/pics/Initial_Success.png)

12) Add this URI at the end of your route `/api/getInspectionsByZipCodeIteration/10100/10150/1`
Which queries and returns Inspections carried between PinCode 10100 and 10150

For more information please access this https://developer.ibm.com/tutorials/mongodb-nodejs-on-openshift/

### Maintainers/Contacts 
For any clarifications/queries please reach out to one of the following contacts listed below,
if found any deviations feel free to raise an issue 

Krishna Harsha Voora

Mithun H R
