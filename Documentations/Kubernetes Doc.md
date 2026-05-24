# KUBERNETES DOC

K8S is used for deploying, maintaining and scaling applications.

K8S provides a way to abstract the underlying infrastructure and manage applications at scale, while also offering flexibility and portability.

K8S is an orchestration engine and open-source platform for managing containerized applications.

Kubernetes provides runtime env for Docker containers.

Scales, descales and load balances DCs.

Monitors and health checks the containers.

Update containers(rolling update) and also rollbacks if needed.

Adds functionality to Docker.

Manages set of Docker hosts, forming a Cluster.

Takes care of container scheduling

---------------------------------------------------------------
### Architectural Overview

### Cluster -> Node Groups -> Deployment -> Replica Sets -> Pods -> Containers -> Application


**_Cluster_** consists of a worker node(control plane) including a set of worker nodes on which containerize applications run.

_**Node Groups**_ is a collection of multiple nodes(physical machines). Control plane manages these nodes.

**_Deployment_** is a resource object for managing Pods and ReplicaSets via a declarative configuration.

The Deployment Controller works to ensure the actual state matches desired state, such as by replacing a failed pod.

A **_ReplicaSet_** is a controller that ensures a specified number of replicas (identical copies) of a pod are running in a cluster at all times, automatically scales the number of pod replicas up or down in response to changes in demand or hardware failures.

**_Pod_** is the smallest deployable unit that represents a single instance of a running process in a cluster, these are the smallest deployable units in k8s, consisting of one or more containers that share the same network namespace.

Represents one or more containers that share:

Network namespace (same IP and port space).

Storage volumes if defined.

Pods are ephemeral: if a Pod dies, Kubernetes replaces it to maintain desired state.

Ephemeral; IP changes on restart

Containers are lightweight, portable packages which consists of everything needed to run an application(code, runtime, libraries) which behave consistently across different environments.

----------------------------------------------------------------------
**_Autoscaling_**

It's useful for handling variable workloads or sudden spikes in traffic.

_**Horizontal Pod Autoscaler**_

It is a feature in Kubernetes that automatically scales the number of replicas of a pod based on the current demand for the workload it is running.
This helps to ensure that the workload can handle increases in traffic and demand without overloading the resources of the cluster.

_**Vertical Pod Autoscaler**_

Vertical Pod Autoscaler (VPA) is a Kubernetes feature that automates the process of adjusting resource limits for containers in pods. Unlike Horizontal Pod Autoscaler (HPA), which scales the number of replicas of a pod, VPA scales the resources allocated to a pod's containers.

------------------------------------------------------------------------
_**Kubernetes vs Docker Swarm**_


Cluster is strong / not so strong

Installation is complicated / very simple

Scales fast / scales 5x faster than K8s

Auto-scales / do not auto-scales

Rolling update and auto rollbacks / RU but no auto RB

Can share storage volumes only with other cont in same pods / with any other containers

No need to worry about NW and communication because K8S will automatically assign IP addresses to conts and a single DNS name for a set of conts that can load balance traffic inside the cluster.

Conts get their own IP so you can put a set of conts behind a single DNS name for load balancing.

------------------------------------------------------------------------
_**Kubernetes Cluster**
_

Kubernetes Cluster is a set of physical or virtual machines and other infrastructure resources that are needed to run your containerized apps.
Each machine in a K8S cluster is called a Node.

2 types of Nodes - Master Node and Worker Node.

-master node(control plain node)  - database gets stored
-worker node - pod are stored here

K8S cluster architecture
Refer [Kubernetes Architecture - Medium](https://medium.com/@salikehassan93/kubernetes-architecture-a-beginners-guide-to-master-and-worker-nodes-c316512c9dc).

Refer [Kubernetes Architecture - Paras Thakur](https://www.linkedin.com/posts/theparasthakur_kubernetes-is-not-a-product-its-an-operating-activity-7401621204295630848-DkXR?utm_source=social_share_send&utm_medium=member_desktop_web&rcm=ACoAADpNd9kBsBTNZcEl2mXdsc8PCsW-BN0yhrs)

Kubectl is the command line admin tool

**3 components of worker node**

_container-runtime_: it is a software which is responsible for managing the lifecycle of the containers. K8S supports a variety of container runtimes such as Docker, Containerd, CRI-O.(docker,podman,crio,containerd,rocket,drawbrdgr)

_kube-proxy_ -> Ensures nw traffic is routed properly to internal and external services, based on the rules defined by nw policies in kube-controller-manager, makes service IPs work, It runs on each node in a K8S cluster.
When a user creates a service, kubelet on eaxh worker node sends a request to kube-proxy to create networking rules.

_kubelet_ -> agent which is responsible for interacting with master node to provide health info about worker node, carries out actions requested by master on worker nodes.

**5 comp of control plane node**

container-runtime
_kubeapiserver_ -> core component server that exposes the Kubernetes HTTP API, helps communicate between two nodes, communicates with kubelet, executes user's REST commands, main access-point to the control plane.

_kubecontroller manager_-> brain behind orchestration, notice and responds when nodes, conts or endpoints goes down, decides to bring new conts in such cases.

_kube scheduler_ -> Watches for new Pods that have no assigned node and selects the best Worker Node for them to run on.

_etcd cluster_ -> database provided by k8s, It is a highly available distributed key-value stored for holding cluster's state, stores all cluster data.

_**Role-Based Access Control (RBAC)**
_

Role-Based Access Control (RBAC) is a method of controlling access to Kubernetes resources based on the roles assigned to users or groups.
Roles are defined as a set of rules that determine what actions can be performed on specific resources.
By assigning roles to users or groups, access to Kubernetes resources can be restricted .

_**Scheduling**
_

-> Scheduling involves assigning pods to worker nodes based on criteria such as resource availability, labels, affinity/anti-affinity rules.

-> The scheduler is responsible for assigning pods to nodes, while labels are used for matching. Affinity and anti-affinity rules dictate how pods are scheduled based on their relationships with other pods or nodes. QoS is used to prioritize pod scheduling based on their resource requirements.

-> Taints and tolerations are used in Kubernetes to restrict or allow pods to be scheduled on certain nodes based on labels.

-> A taint is a label that is applied to a node to indicate certain limitations or requirements. A toleration is a label applied to a pod to indicate that it can tolerate certain taints. When a node has a taint, only pods with the corresponding tolerations can be scheduled on that node, This feature is useful for various purposes, such as ensuring separation of critical and non-critical workloads, reserving nodes for certain tasks, and protecting nodes from overloading.

_**Logs**
_

Logs are generated by containerized applications running on nodes within the cluster. These are records of events and actions that occur within your Kubernetes cluster. Logs are essential for troubleshooting, monitoring and debugging our applications. You can access these logs using the kubectl logs command followed by the name of the pod. By default, this command shows the logs from the most recent container in the pod, but you can specify a specific container within the pod by adding the container name to the command. It also lets us know what happens inside Control plane, Pods, Nodes.

_**Services**
_

A stable networking abstraction that exposes a set of Pods, Provides stable access to Pods
Single DNS name and IP for clients, even as Pods come and go.
Load balancing across multiple Pods.
Common types:
_ClusterIP:_ Internal-only access within the cluster.
_NodePort:_ Exposes service on each node’s IP at a static port.
_LoadBalancer:_ External access via cloud provider LB.
_ExternalName:_ Maps service to an external DNS name.

**Persistent endpoint** (IP/DNS)
If a Pod crashes and is replaced, the Service still points to the new Pod.

_**Namespace**
_

A Namespace is a logical partition inside a Kubernetes cluster.

It helps organize and isolate resources (Pods, Services, Deployments)

Think of it as virtual clusters within one physical cluster.

Default namespaces:

default: For user workloads if no namespace is specified.

kube-system: For system components (DNS, scheduler, etc.).

kube-public: Readable by all users; often for public cluster info

_**Labels**
_

Labels are key-value pairs attached to Kubernetes objects (Pods, Services, Deployments, Nodes, etc.).
Purpose:
Identify and group related resources.

**Selectors**
Selectors are queries that match labels to filter objects.
Purpose:
Services use selectors to route traffic to Pods.
Deployments use selectors to manage Pods.

**Ingress:**
Manages external access to services.

**Ingress Controller**
Manages ingress resources to provide HTTP and HTTPS routing
It is an advanced version of Load Balancer
It has path based routing - It means routing traffic to different Services based on the URL path.
Also acts as a reverse proxy
Without Ingress, you’d need one Load Balancer per Service → costly and complex.
With Ingress(Layer-7(http/https)), you can:
Use one Load Balancer + one Ingress Controller to expose many Services under a single IP/domain.
Reduce cost and simplify SSL management.
Ingress Controller often uses a Load Balancer as its entry point:
This Load Balancer forwards traffic to the Ingress Controller, which then applies routing rules to multiple Services.

### User → Load Balancer → Ingress Controller → Correct Service → Pods

**DaemonSets**
A DaemonSet ensures one Pod runs on every node.
When a new node joins → DaemonSet adds a Pod.
When a node leaves → Pod is removed.
DaemonSets are not for scaling apps, but for system-wide agents.

_**Jobs and Cron Jobs**
_

A **Job** runs Pods to completion (one-time tasks).
Creates Pods until the task finishes successfully.
Jobs exit after completion, unlike Deployments that keep Pods running.
Used for Batch processing, Database migration.

A **CronJob** runs Jobs on a schedule
CronJobs = Jobs + time-based scheduling.
Used for Backups, Report generation, Cleanup tasks.
Use CronJobs for recurring tasks.

