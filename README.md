# Cloud Interoperability 

This repository contains service/deployment-configuration files that can be consumed "as-is" on any OpenShift Container Platform 
as well as Kubernetes Platform in order to deploy GeoSpatial Workload made up of Two-tier application consisting 
of Open Source Components ( MongoDB/NodeJS ).

The docker images involved are custom built for this particular usecase and leverages image-manifest that identifies
underlying architecture(s) and pulls respective docker image.

This also demonstrates ease of moving from Public Cloud traditional x86_64 (like GCP,AWS) to 
powerful IBM POWER Systems hosted in Hybrid/Private Clouds ( like Openshift Container Platform or IBM Cloud )

### Steps to deploy the MongoDB + NodeJS Application on OpenShift Container Platform v3.11/v4.X Command Line -

Git clone this repository to your server -

```
cd $HOME/
git clone https://github.com/krishvoor/cloud_interop
cd cloud_interop
```

Create a new project 

```
oc project mithun
```

Deploy Service and Deployment config files via

```
oc create -f mong-service.yaml 
oc create -f node-service.yaml
oc create -f mong-deployment.yaml 
oc create -f node-deployment.yaml 
```

Upon Successfully applying respective Service/Deployment config you will see something similar as below -

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

To create a secure access to your application, you need create a route and
expose the service.

```
[root@p230n134 cloud_interop]# oc get svc
NAME                    READY     STATUS    RESTARTS   AGE
NAME   TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)     AGE
mong   ClusterIP   172.30.135.8     <none>        27017/TCP   1m
node   ClusterIP   172.30.126.228   <none>        3000/TCP    1m
[root@p230n134 cloud_interop]# oc create route edge --service=node
route.route.openshift.io/node created
[root@p230n134 cloud_interop]#
```

So far you have deployed the solution and created a secure route to be accessible outside OCP cluster.
Please add this API at end of your route `/api/getInspectionsByZipCodeIteration/10100/10150/1` 
which would fetch Inspections carried between areas with pincodes `10100` and `101500` via NodeJS APIs
and from MongoDB.

For more information - please visit this website(https://google.com)

### Creating Route and accessing application outside cluster

Creating a route in OCP v3.11 -
https://docs.openshift.com/container-platform/3.11/dev_guide/routes.html#creating-routes 

Creating a route in OCP v4.3 -
There are many wasy to do it, one of them is as following
https://docs.openshift.com/container-platform/4.3/networking/configuring_ingress_cluster_traffic/configuring-ingress-cluster-traffic-nodeport.html#configuring-ingress-cluster-traffic-nodeport

### Deploying this Two Tier Application via OCP4.X WebConsole

Deploying this solution via OCP4.3 Webconsole - is pretty simple - to make your life easier we have 
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

12) Add this API at the end of your route `/api/getInspectionsByZipCodeIteration/10100/10150/1`
Which queries and returns Inspections carried between PinCode 10100 and 10150

For more information please access this https://developer.ibm.com/tutorials/mongodb-nodejs-on-openshift/

### Clean-Up/Delete your Application
To delete your application, follow below steps -

```
oc delete -f node-deployment.yaml
oc delete -f mong-deployment.yaml
oc delete -f node-service.yaml
oc delete -f mong-service.yaml 
```

### Maintainers/Contacts 
For any clarifications/queries please reach out to one of the following contacts listed below,
if found any deviations feel free to raise an issue 

Krishna Harsha Voora

Mithun H R
