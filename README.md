# orchestrator

## Setup

In order to be able to run this application you need to have the following
programs installed on your machine:

- [Vagrant](https://developer.hashicorp.com/vagrant/docs/installation).
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads).

- [Postman](https://www.postman.com/downloads/), or any other tool to
  programmatically test API endpoints.
- [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl) to be able to
  interact with the Kubernetes cluster from your machine. Check [this cheat
  sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) to get an
  idea of some useful commands.

To run and manage the Kubernetes cluster on the VM cluster:
- Change `.env.example` to `.env`

To run `kubectl` on your machine to interact with the Kubernetes cluster, you
need to `export KUBECONFIG=./k3s/k3s.yaml` in `.bashrc` or `.zshrc`.

RUN
```bash
./orchestrator.sh create
./orchestrator.sh start
```

# Questions
### What is container orchestration, and what are its benefits?
```
Container orchestration is the automated process of managinh the lifecycle of containers, including deployment, 
scaling, networking, and availability. Tools like Kubernetes are commonly used for this purpose.

Benefits :
- Scalability 
- High availability
- Efficient Resource Utilization
- Automation
- Portability
```
---

### What is Kubernetes, and what is its main role?
```
Kubernetes(K8s) is an open-source container orchestration platform that automates the deployment, scaling, 
and management of containerized applications.

Main Role :
It ensures that desired state of an application (e.g., number of replicas, ressource allocation)
is maintained through declarative configuration and automation.
```
---
### What is K3s, and what is its main role?
```
K3s is a lightweight Kubernetes distribution that is easy to install and use. It is designed for edge computing,
IoT, and other resource-constrained environments.

Main Role :
It provides a lightweight way to run Kubernetes clusters in production environments.
```
---
### What is infrastructure as code and what are the advantages of it?
```
infrastructure as Code (IaC) involves managing and provisionning infrastructure through code rather than
manual processes.

Advantages:
- Consistency
- Automation
- Scalability
- Version Control
- Reduced Risk
```
---
### Explain what is a K8s manifest
```
a K8s manifest is a YAML file that defines the desired state of a K8s object. It contains the configuration
of the object, such as the number of replicas, the image to use, the ports to expose, etc.
```
---
### Explain each K8s manifests.
```
- Pod : 
  A pod is the smallest deployable unit in K8s. It represents a single instance of a running process in the cluster.

- Deployment : 
  A deployment is a higher-level abstraction that manages pods. It ensures that a specified number of pod replicas are running at any given time.

- Service : 
  A service is an abstraction that defines a logical set of pods and a policy by which to access them. It provides a stable endpoint for pods to communicate with each other.

- ConfigMap :
   A ConfigMap is an API object used to store non-confidential data in key-value pairs. It can be used to store configuration data that is consumed by other parts of the system.

- Secret : 
  A Secret is an API object used to store sensitive data, such as passwords, OAuth tokens, and SSH keys. It is similar to a ConfigMap but is base64 encoded and stored in a separate location.

- PersistentVolumeClaim : 
  A PersistentVolumeClaim is an API object used to request storage resources from a PersistentVolume. It allows pods to consume storage resources without needing to know the details of the underlying storage infrastructure.

- StatefulSet :   
  A StatefulSet is an API object used to manage stateful applications in K8s. It provides guarantees about the ordering and uniqueness of pod names, as well as stable network identifiers.
```
---
### What is StatefulSet in K8s?
```
StatefulSet is a Kubernetes workload designed for managing stateful applications. It ensures Pods maintain a stable identity and persistebt storage across restarts.
```
---
### What is deployment in K8s?
```
A Deployement in K8s is a declarative ressource used to manage stateless applications.
It ensures the desired number of Prods are running and facilitates rolling updates and rollbacks.
```
---
### What is the difference between deployment and StatefulSet in K8s?

| **Criteria**                  | **Deployment**                                    | **StatefulSet**                                   |
|-------------------------------|--------------------------------------------------|-------------------------------------------------|
| **Application Type**          | **Stateless** applications                       | **Stateful** applications                        |
| **Pod Identity**              | Pods are identical and interchangeable.          | Each pod has a **unique identity** (e.g., `app-0`). |
| **Storage**                   | Generally ephemeral or shared storage.           | Each pod uses a **dedicated persistent volume**. |
| **Startup Order**             | No specific order for starting pods.             | Pods are started in a strict order.             |
| **Shutdown Order**            | Pods can be deleted in any order.                | Ordered shutdown to preserve state.             |
| **Pod Names**                 | Dynamically generated names without persistence. | Fixed and persistent names (e.g., `app-0`, `app-1`). |
| **Typical Use Cases**         | Web servers, APIs, microservices, workers.       | Databases, Kafka, ZooKeeper, Redis with state.  |
| **Example Volume**            | Temporary volumes or shared external storage.    | **PersistentVolumeClaim** (PVC) unique per pod. |
---
- **Stateless**: An application where each instance is interchangeable and does not retain data between requests. If a pod is deleted or recreated, it does not affect the application since data is usually stored externally (e.g., external databases).
- **Stateful**: An application where each instance has a unique identity and retains local data necessary for its operation. Losing or recreating a pod can impact the application's overall state.

Use a **Deployment** for stateless applications where pods can be replaced or recreated without impact, and use a **StatefulSet** for stateful applications that require unique identities and persistent data.

---
### What is scaling, and why do we use it?
```
Scaling is the process of increasing or decreasing the number of instances of a service based on demand.

We use it to handle varying workloads efficiently and ensure high availability.
```
---
### What is a load balancer, and what is its role?
```
A load balancer distributes network traffic across multiple servers or Pods to ensure no single resource is overwhelmed.

Role:
	- Improves performance by balancing traffic.
	- Enhances fault tolerance by redirecting traffic in case of failures.
```
---
### Why we don't put the database as a deployment?
```
Databases are stateful by nature, requiring persistent storage and stable network identity, which is better managed by StatefulSet rather than a Deployment.
```
---
### Can the learner explain the K8s components in less than 15 minutes?

### **1. Control Plane**
- **API Server**: Main interface to manage the cluster.  
- **Etcd**: Stores all cluster data (state, configuration).  
- **Controller Manager**: Maintains the desired state (e.g., monitors nodes).  
- **Scheduler**: Assigns pods to nodes based on resource availability.

### **2. Node Components**
- **Kubelet**: Ensures pods are running correctly on each node.  
- **Kube-proxy**: Manages network rules to connect pods and services.  
- **Container Runtime**: Runs containers (Docker, containerd, CRI-O).

### **3. Additional Components**
- **Pods**: Basic unit containing one or more containers.  
- **Services**: Provide stable access points to pods.  
- **Ingress**: Manages external access (HTTP/HTTPS).  
- **ConfigMaps/Secrets**: Handle configurations and sensitive data.  
- **Volumes**: Provide persistent storage to pods.
---
## Commands for Postgres
```
psql -U <user-name>
psql -U <user-name> -d <db-name>
```

## Add to docker hub
```
sudo docker tag <name-of-repo>:<tag> <userdocker>/<choose-name-image>:<tag>
```