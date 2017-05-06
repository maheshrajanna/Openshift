# Openshift


![Alt text](https://github.com/maheshrajanna/Docker/blob/master/fyile.png?raw=true "Optional Title")



# Master Node


![Alt text](https://github.com/maheshrajanna/Docker/blob/master/fyile.png?raw=true "Optional Title")


Master node is responsible for the management of Kubernetes cluster. This is the entry point of all administrative tasks. Master node is the one taking care of orchestrating the worker nodes, where the actual services are running.

Diving into each of the components of the master node.

### API server

API server is the entry points for all the REST commands used to control the cluster. It processes the rest requests, validates them, executes the bound business logic. The result state has to be persisted somewhere, and that brings us to the next component of the master node.

### etcd storage

etcd is a simple, distributed, consistent key-value store. It’s mainly used for shared configuration and service discovery. 
It provides a REST API for CRUD operations as well as an interface to register watchers on specific nodes, which enables a reliable way to notify the rest of the cluster about configuration changes.

Example of data stored by Kubernetes in etcd are jobs being scheduled, created and deployed pod/service details and state, namespaces and replication informations, etc.

### scheduler

The deployment of configured pods and services onto the nodes happens thanks to the scheduler component. 
Scheduler has the information regarding resources available on the members of the cluster, as well as the ones required for the configured service to run and hence is able to decide where to deploy a specific service.

### controller-manager

Optionally you can run different kinds of controllers inside the master node. controller-manager is a daemon embedding those. 
A controller uses apiserver to watch the shared state of the cluster and makes corrective changes to the current state to being it to the desired one. 
An example of such a controller is the Replication controller, which takes care of the number of pods in the system. The replication factor is configured by the user and that’s the controller’s responsibility to recreate a failed pod, or remove an extra-scheduled one. 
Other examples of controllers are endpoints controller, namespace controller, and serviceaccounts controller, but we will not dive into details here.


![Alt text](https://github.com/maheshrajanna/Docker/blob/master/fiyle.png?raw=true "Optional Title")


# Worker node

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





**Types*

1. Kubernetes Pod 
    ● Group of one or more containers that are always co-located, co-scheduled, and run in a shared context  
    ● Containers in the same pod have the same hostname 
    ● Each pod is isolated by ○ Process ID (PID) namespace ○ Network namespace ○ Interprocess Communication (IPC) namespace ○ Unix Time       Sharing (UTS) namespace 
    ● Alternative to a VM with multiple processes


2. Services 
    ● An abstraction to define a logical set of Pods bound by a policy by to access them 
    ● Services are exposed through internal and external endpoints 
    ● Services can also point to non-Kubernetes endpoints through a Virtual-IP-Bridge 
    ● Supports TCP and UDP 
    ● Interfaces with kube-proxy to manipulate iptables ● Service can be exposed internal or external to the cluster


3. Replication Controller 
    ● Ensures that a Pod or homogeneous set of Pods are always up and available. Always maintains desired number of Pods ○ If there           are excess Pods, they get killed and New pods are launched when they fail, get deleted, or terminated.
    ● Creating a replication controller with a count of 1 ensures that a Pod is always available. Replication Controller and Pods are         associated through "Labels".



```

```

