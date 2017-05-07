http://blog.arungupta.me/openshift-v3-getting-started-javaee7-wildfly-mysql/

A project is a Kubernetes namespace with additional annotations




Each project scopes its own set of:

Objects : Pods, services, replication controllers, etc.

Policies : Rules for which users can or cannot perform actions on objects.

Constraints : Quotas for each kind of object that can be limited.

Service accounts : Service accounts act automatically with designated access to objects in the project.

$ oc new-project <project_name> \
    --description="<description>" --display-name="<display_name>"
    
    $ oc get projects
    $ oc project <project_name> : to change the project
    
    $ oc policy add-role-to-user admin mahesh -n project name
    $ oc status : The oc status command provides a high-level overview of the current project.
    



# Service Account

When a person uses the OpenShift Enterprise CLI or web console, their API token authenticates them to the OpenShift API. However, when a regular user’s credentials are not available, it is common for components to make API calls independently. For example:

    Replication controllers make API calls to create or delete pods.

    Applications inside containers could make API calls for discovery purposes.

    External applications could make API calls for monitoring or integration purposes.

Service accounts provide a flexible way to control API access without sharing a regular user’s credentials.




Three service accounts are automatically created in every project:

    builder is used by build pods. It is given the system:image-builder role, which allows pushing images to any image stream in the project using the internal Docker registry.

    deployer is used by deployment pods and is given the system:deployer role, which allows viewing and modifying replication controllers and pods in the project.

    default is used to run all other pods unless they specify a different service account.



https://docs.openshift.com/enterprise/3.1/dev_guide/service_accounts.html



# Openshift


![Alt text](https://github.com/maheshrajanna/Openshift/blob/master/DKO.png?raw=true "Optional Title")



# Openshift Architecture

   
![Alt text](https://github.com/maheshrajanna/Openshift/blob/master/Architecture.png?raw=true "Optional Title")



# Master Node

![Alt text](https://github.com/maheshrajanna/Openshift/blob/master/Master.png?raw=true "Optional Title")

Master node is responsible for the management of Kubernetes cluster. This is the entry point of all administrative tasks. Master node is the one taking care of orchestrating the worker nodes, where the actual services are running.

Diving into each of the components of the master node.

### API server

API server is the entry points for all the REST commands used to control the cluster. It processes the rest requests, validates them, executes the bound business logic. The result state has to be persisted somewhere, and that brings us to the next component of the master node.

### scheduler

The deployment of configured pods and services onto the nodes happens thanks to the scheduler component. 
Scheduler has the information regarding resources available on the members of the cluster, as well as the ones required for the configured service to run and hence is able to decide where to deploy a specific service.

### controller-manager

Optionally you can run different kinds of controllers inside the master node. controller-manager is a daemon embedding those. 
A controller uses apiserver to watch the shared state of the cluster and makes corrective changes to the current state to being it to the desired one. 
An example of such a controller is the Replication controller, which takes care of the number of pods in the system. The replication factor is configured by the user and that’s the controller’s responsibility to recreate a failed pod, or remove an extra-scheduled one. 
Other examples of controllers are endpoints controller, namespace controller, and serviceaccounts controller, but we will not dive into details here.


### etcd 

etcd is a simple, distributed, consistent key-value store. It’s mainly used for shared configuration and service discovery. 
It provides a REST API for CRUD operations as well as an interface to register watchers on specific nodes, which enables a reliable way to notify the rest of the cluster about configuration changes.

Example of data stored by Kubernetes in etcd are jobs being scheduled, created and deployed pod/service details and state, namespaces and replication informations, etc.

https://coreos.com/etcd/docs/latest/getting-started-with-etcd.html


# Worker node

![Alt text](https://github.com/maheshrajanna/Openshift/blob/master/Master.png?raw=true "Optional Title")


The pods are run here, so the worker node contains all the necessary services to manage the networking between the containers, communicate with the master node, and assign resources to the containers scheduled.

### Docker

Docker runs on each of the worker nodes, and runs the configured pods. It takes care of downloading the images and starting the containers.

### kubelet

kubelet gets the configuration of a pod from the apiserver and ensures that the described containers are up and running. This is the worker service that’s responsible for communicating with master ndoe. 
It also communicates with etcd, to get information about services and write the details about newly created ones.

### Supervisord (kubelet and Docker)


### kube-proxy

kube-proxy acts as a network proxy and a load balancer for a service on a single worker node. It takes care of the network routing for TCP and UDP packets.

### kubectl

And final bit – a command line tool to communicate with API service and send commands to the master node.


### Fluentd

Collecting Docker Log Files with Fluentd and Elasticsearch.

https://github.com/kubernetes/kubernetes/tree/master/cluster/addons/fluentd-elasticsearch/fluentd-es-image
 
### POD

Group of one or more containers that are always co-located, co-scheduled, and run in a shared context 

### Additional Addons 

DNS and UI etc.



# Understanding Objects

1. POD 
    * Group of one or more containers that are always co-located, co-scheduled, and run in a shared context  
    * Containers in the same pod have the same hostname 
    * Each pod is isolated by Process ID (PID) namespace, Network namespace, Interprocess Communication (IPC) namespace
      and Unix Time Sharing (UTS) namespace 
    * Alternative to a VM with multiple processes

2. SERVICES 
    * An abstraction to define a logical set of Pods bound by a policy by to access them 
    * Services are exposed through internal and external endpoints/cluster 
    * Services can also point to non-Kubernetes endpoints through a Virtual-IP-Bridge 
    * Supports TCP and UDP 
    * Interfaces with kube-proxy to manipulate iptables 
    * Service will understand the PODS with Labels 

3. REPLICATION CONTROLLER 
    * Ensures that a Pod or homogeneous set of Pods are always up and available. Always maintains desired number of Pods, 
      If there are excess Pods, they get killed and New pods are launched when they fail, get deleted, or terminated.
    * Creating a replication controller with a count of 1 ensures that a Pod is always available. Replication Controller and
      Pods are associated through "Labels".

4. INGRESS/ROUTE
    * An OpenShift route exposes a service at a host name, like www.example.com, so that external clients can reach it by name.
    * DNS resolution for a host name is handled separately from routing; your administrator may have configured a cloud domain
      that will always correctly resolve to the OpenShift router, or if using an unrelated host name you may need to modify its
      DNS records independently to resolve to the router.

5. LABELS AND SELECTORS
    * Labels are used to organize, group, or select API objects. For example, pods are "tagged" with labels, and then services
      use label selectors to identify the pods they proxy to. This makes it possible for services to reference groups of pods,
      even treating pods with potentially different Docker containers as related entities.
      https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/




Multi-Container Pods in OPenshift


Web Pod has a Python Flask container and a Redis container
DB Pod has a MySQL container
When data is retrieved through the Python REST API, it first checks within Redis cache before accessing MySQL
Each time data is fetched from MySQL, it gets cached in the Redis container of the same Pod as the Python Flask container
When the additional Web Pods are launched manually or through a Replica Set, co-located pairs of Python Flask and Redis containers are scheduled together




Build a Docker image from existing Python source code and push it to Docker Hub. Replace DOCKER_HUB_USER with your Docker Hub username.

https://github.com/maheshrajanna/Openshift/tree/master/Build

```
cd Build
docker build . -t <DOCKER_HUB_USER>/py-red-sql
docker push <DOCKER_HUB_USER>/py-red-sql
Deploy the app to Kubernetes

``` 


https://github.com/maheshrajanna/Openshift/tree/master/Deploy

```
cd ../Deploy
oc create -f db-pod.yml
oc create -f db-svc.yml
oc create -f web-pod-1.yml
oc create -f web-svc.yml
Check that the Pods and Services are created

```

```
oc get pods
oc get svc
Get the IP address of one of the Nodes and the NodePort for the web Service. Populate the variables with the appropriate values

```

```

oc get nodes
oc describe svc web

oc get nodes
export NODE_IP=<NODE_IP>
export NODE_PORT=<NODE_PORT>
```
Initialize the database with sample schema

```
curl http://$NODE_IP:$NODE_PORT/init
Insert some sample data

```
```
curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "1", "user":"Mahesh Raj"}' http://$NODE_IP:$NODE_PORT/users/add
curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "2", "user":"Best Test"}' http://$NODE_IP:$NODE_PORT/users/add
Access the data
```

````
curl http://$NODE_IP:$NODE_PORT/users/1
The second time you access the data, it appends '(c)' indicating that it is pulled from the Redis cache
````
```
curl http://$NODE_IP:$NODE_PORT/users/1
Create 10 Replica Sets and check the data
```
```
oc create -f web-rc.yml
curl http://$NODE_IP:$NODE_PORT/users/1
```





