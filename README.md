# Openshift





![Alt text](https://github.com/maheshrajanna/Docker/blob/master/file.png?raw=true "Optional Title")



# Master Node


![Alt text](https://github.com/maheshrajanna/Docker/blob/master/file.png?raw=true "Optional Title")


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















**Build image from docker file**

```

```

