# Kubernetes Architecture

### Node

- A node is machine where pods with containers will be launched by kubernetes

- if one node fails ? our application will go down. so we need to have more than one node a cluster is set of nodes grouped
    together, this way even if one node fails you have your application still accessible from the other node
    more over multiple nodes share load as well.


### Master

- The master is another node with kubernetes installed in it and is configured as master.

- the master watches all nodes in the cluster and is responsible for actual orchestration of containers on worker nodes

### Components

- when installing kubernetes the following components will be installed.

### On Master node

- **ETCD service** : ETCD is a key value store used by kubernetes to store all data managed to store cluster. 
when you have multiple masters and multiple nodes in your cluster, ETCD stores all that information.
ETCD is responsible for implementing locks within the cluster to ensure that there are no conflicts between the masters.

- **API server** : The API servers acts as a frontend for the kubernetes, the users, management devices command line 
interfaces all talk to the API-server to interact with kubernetes cluster.

- **scheduler** : it is responsible for distributing work or containers across multiple nodes.

- **controllers** : this is brain behind  orchestration. these are responsible for noticing and responding when nodes,containers 
or end points goes down. 
  
### On Worker node

- **container runtime** : it is the underlying software that is used to run containers in nodes. like Docker, cri-o, rocket etc

- **kubelet** : this is an agent that runs on every node in the cluster. agent is responsible for making sure that the
containers are running on the nodes as expected.

**Command line tool** : kubectl is available kubernetes command line tool.
it is used to deploy and manage applications on kube cluster





