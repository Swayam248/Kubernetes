# Kubernetes Documentation

## 1. Introduction to Kubernetes

Kubernetes (K8s) is an open-source container orchestration platform used for deploying, managing, maintaining, and scaling containerized applications.

Kubernetes provides a way to abstract the underlying infrastructure and manage applications at scale while offering flexibility and portability across different environments.

### Why Do We Use Kubernetes?

- Automates the deployment and management of containerized applications.
- Scales applications up and down based on workload requirements.
- Provides service discovery and load balancing.
- Monitors application health and restarts failed containers when required.
- Supports rolling updates and rollbacks.
- Schedules Pods across available worker nodes.
- Manages containers across multiple machines that form a cluster.
- Helps maintain the desired state of applications.
- Works with container runtimes such as containerd and CRI-O.

Kubernetes does not simply provide a runtime environment for Docker containers. It orchestrates containerized workloads using a supported container runtime.

---

## 2. Architectural Overview

### Kubernetes Resource Hierarchy

```text
Kubernetes Cluster
├── Control Plane
└── Worker Nodes
    └── Deployment
        └── ReplicaSet
            └── Pods
                └── Containers
                    └── Application
```

> Note: Node groups are commonly used in managed Kubernetes environments, such as Amazon EKS. They are not a mandatory layer in every Kubernetes cluster.

### 2.1 Cluster

A Kubernetes cluster consists of a control plane and one or more worker nodes.

- **Control Plane:** Manages the cluster and makes orchestration decisions.
- **Worker Nodes:** Run the Pods that host containerized applications.

### 2.2 Node Groups

A node group is a collection of worker nodes managed together. In managed Kubernetes services, node groups help simplify the provisioning and management of worker nodes.

### 2.3 Deployment

A Deployment is a Kubernetes resource used to manage stateless applications declaratively.

It manages ReplicaSets and provides features such as:

- Rolling updates
- Rollbacks
- Scaling
- Desired-state management

The Deployment controller ensures that the actual state of the application matches the desired state.

For example, if a Deployment specifies four replicas and one Pod becomes unavailable, Kubernetes works to restore the desired number of replicas.

### 2.4 ReplicaSet

A ReplicaSet ensures that a specified number of Pod replicas are running at a given time.

For example, if a ReplicaSet is configured with four replicas, it attempts to maintain four matching Pods.

If a Pod fails or is deleted, the ReplicaSet creates a replacement when necessary.

A Deployment normally manages ReplicaSets, so we generally manage application replicas through Deployments rather than creating ReplicaSets directly.

### 2.5 Pod

A Pod is the smallest deployable unit in Kubernetes. It represents one or more containers that share certain resources.

Containers within the same Pod share:

- **Network namespace:** They share the Pod's IP address and network port space.
- **Storage volumes:** They can share volumes attached to the Pod.

Important characteristics:

- Pods are ephemeral.
- A Pod can be recreated when it fails.
- A replacement Pod may receive a different IP address.
- Containers within the same Pod communicate using `localhost`, provided they use different ports.

### 2.6 Containers

A container is an isolated execution environment for an application.

It packages an application with the dependencies needed to run it, such as libraries and runtime components.

Containers help applications behave consistently across development, testing, and production environments.

---

## 3. Autoscaling

Autoscaling helps Kubernetes handle changing workloads and sudden increases or decreases in application traffic.

### 3.1 Horizontal Pod Autoscaler (HPA)

The Horizontal Pod Autoscaler automatically adjusts the number of Pod replicas based on observed metrics.

Common metrics include:

- CPU utilization
- Memory utilization, when configured
- Custom application metrics
- External metrics

**Example:** If application traffic increases, HPA can increase the number of Pod replicas to handle the additional load.

When demand decreases, HPA can scale down the replicas.

HPA requires a suitable metrics source, such as Metrics Server for resource metrics.

### 3.2 Vertical Pod Autoscaler (VPA)

The Vertical Pod Autoscaler adjusts the CPU and memory resource requests of containers based on observed usage.

Depending on its configuration and update mode, it can recommend or apply resource changes. Applying changes may require Pod restarts.

**Difference between HPA and VPA:**

- **HPA:** Adjusts the number of Pod replicas.
- **VPA:** Adjusts or recommends the resources allocated to containers.

---

## 4. Kubernetes vs Docker Swarm

| Feature | Kubernetes | Docker Swarm |
|---|---|---|
| Architecture | More extensive orchestration architecture | Simpler orchestration architecture |
| Setup | Generally more complex | Generally easier to set up |
| Scaling | Supports automated scaling with additional configuration | Supports service replica scaling; native autoscaling capabilities are more limited |
| Auto-scaling | Supports HPA and other autoscaling mechanisms | Does not provide an equivalent built-in HPA mechanism |
| Rolling updates | Supported | Supported |
| Rollbacks | Deployment rollbacks are supported | Rollback support is available for services, with different behavior and configuration |
| Networking | Provides Pod networking, Services, DNS-based discovery, and network policy integrations | Provides service discovery and overlay networking |
| Storage | Supports persistent volumes and multiple storage integrations | Supports volumes and storage plugins |
| Ecosystem | Extensive ecosystem and integrations | Simpler ecosystem and operational model |

### Networking Comparison

**Kubernetes:**

- Each Pod receives its own IP address through the cluster's networking implementation.
- Services provide stable access to matching Pods.
- DNS allows applications to discover Services by name.
- Service traffic can be distributed across eligible backend Pods.

**Docker Swarm:**

- Provides service discovery and overlay networking.
- Services can distribute traffic across their replicas.
- Tasks and containers communicate through Swarm networking mechanisms.

The exact performance and scaling characteristics depend on the workload, configuration, and environment. Avoid assuming that either platform is always faster.

---

## 5. Kubernetes Cluster

A Kubernetes cluster is a collection of physical or virtual machines and supporting infrastructure used to run containerized applications.

Each machine participating in the cluster is called a **Node**.

There are two main node roles:

### 5.1 Control Plane Node

The control plane manages the cluster's desired state, scheduling decisions, and orchestration operations.

It includes components responsible for:

- Exposing the Kubernetes API
- Scheduling Pods
- Maintaining the desired state
- Storing cluster data

### 5.2 Worker Node

Worker nodes run application workloads inside Pods.

They contain components responsible for:

- Running containers
- Reporting node and Pod status
- Implementing Service networking
- Communicating with the control plane

### Kubernetes Architecture References

- [Kubernetes Architecture — Medium](https://medium.com/@salikehassan93/kubernetes-architecture-a-beginners-guide-to-master-and-worker-nodes-c316512c9dc)
- [Kubernetes Architecture — Paras Thakur on LinkedIn](https://www.linkedin.com/posts/theparasthakur_kubernetes-is-not-a-product-its-an-operating-activity-7401621204295630848-DkXR)

### Kubectl

`kubectl` is the Kubernetes command-line tool used to communicate with the Kubernetes API server and manage cluster resources.

Examples:

```bash
kubectl get nodes
kubectl get pods
kubectl get deployments
kubectl describe pod <pod-name>
kubectl logs <pod-name>
```

---

## 6. Worker Node Components

The main worker node components are the container runtime, kubelet, and kube-proxy.

### 6.1 Container Runtime

The container runtime is responsible for running containers and managing their lifecycle.

Examples include:

- containerd
- CRI-O

Docker Engine can also be used in container environments, but modern Kubernetes versions communicate with runtimes through the Container Runtime Interface (CRI). Docker Engine itself is not directly CRI-compatible without an additional integration such as `cri-dockerd`.

### 6.2 Kubelet

Kubelet is an agent that runs on each worker node.

Responsibilities:

- Communicates with the Kubernetes control plane.
- Ensures that containers described in assigned Pod specifications are running.
- Reports node and Pod status.
- Performs container health checks using configured probes.
- Reports relevant runtime information.

Kubelet does not independently decide which worker node should receive a Pod. That decision is generally made by the scheduler.

### 6.3 Kube-proxy

Kube-proxy implements Service networking rules on nodes in clusters that use it.

Responsibilities:

- Helps route traffic to Service backends.
- Implements Service virtual IP behavior through networking rules.
- Supports traffic distribution across eligible endpoints.

Depending on the cluster's networking implementation, kube-proxy may be replaced by another component that provides equivalent Service networking functionality.

> Important: NetworkPolicy enforcement is generally implemented by a compatible network plugin, not by kube-proxy itself.

---

## 7. Control Plane Components

The control plane contains several components that work together to manage the Kubernetes cluster.

### 7.1 Kube-apiserver

The API server exposes the Kubernetes HTTP API and acts as the main entry point for cluster management operations.

Responsibilities:

- Receives API requests from `kubectl`, controllers, and other clients.
- Authenticates and authorizes requests.
- Validates resource specifications.
- Coordinates access to cluster state stored in etcd.
- Exposes APIs used by other Kubernetes components.

### 7.2 Kube-controller-manager

The controller manager runs multiple controllers that continuously compare the actual cluster state with the desired state.

Examples:

- Deployment controller
- ReplicaSet controller
- Node controller
- Job controller

If a Pod is missing from a Deployment's desired replica count, the relevant controllers work to restore the desired state.

### 7.3 Kube-scheduler

The scheduler watches for newly created Pods that do not yet have an assigned node.

It selects a suitable worker node based on factors such as:

- Available resources
- Resource requests
- Node selectors
- Affinity and anti-affinity rules
- Taints and tolerations
- Scheduling constraints

The scheduler assigns the Pod to a node. The kubelet on that node then works to start its containers.

### 7.4 etcd

etcd is a distributed key-value store used by Kubernetes to persist cluster state.

It stores information such as:

- Kubernetes resource definitions
- Desired and observed cluster state
- Configuration and metadata

A highly available control plane can use multiple etcd members to improve availability.

Backups of etcd are important for disaster recovery because it contains critical cluster state.

### 7.5 Container Runtime

The control plane can also run containers for its own components, depending on the cluster setup.

However, the container runtime is not normally counted as one of the four core control plane components.

The four commonly identified core components are:

1. kube-apiserver
2. kube-controller-manager
3. kube-scheduler
4. etcd

---

## 8. Role-Based Access Control (RBAC)

Role-Based Access Control is a Kubernetes authorization mechanism that determines what actions a user, group, or service account can perform on cluster resources.

RBAC uses rules that specify:

- **API groups:** The API group containing the resource.
- **Resources:** For example, Pods, Deployments, and Services.
- **Verbs:** Actions such as `get`, `list`, `create`, `update`, and `delete`.

### Main RBAC Resources

- **Role:** Defines permissions within a specific namespace.
- **ClusterRole:** Defines permissions that can apply across the cluster or to namespaced resources when appropriately bound.
- **RoleBinding:** Grants a Role or ClusterRole to subjects within a namespace.
- **ClusterRoleBinding:** Grants a ClusterRole across the cluster.

RBAC follows the principle of least privilege: grant only the permissions required to perform a task.

---

## 9. Scheduling

Kubernetes scheduling is the process of assigning Pods to suitable worker nodes.

The scheduler considers resource requirements, node conditions, and scheduling constraints.

### 9.1 Labels

Labels are key-value pairs attached to Kubernetes objects.

They help identify, organize, and select related resources.

Example:

```yaml
labels:
  app: nginx
  environment: production
```

### 9.2 Node Selectors

A `nodeSelector` constrains a Pod to nodes with specified labels.

Example:

```yaml
spec:
  nodeSelector:
    disktype: ssd
```

This Pod can be scheduled only on a node with the matching label, assuming the other scheduling requirements are also satisfied.

### 9.3 Affinity and Anti-affinity

- **Affinity:** Encourages or requires Pods to be scheduled near selected nodes or other Pods.
- **Anti-affinity:** Encourages or requires Pods to be scheduled away from selected nodes or other Pods.

These rules can help improve availability, workload isolation, and resource placement.

### 9.4 Taints and Tolerations

Taints and tolerations control which Pods can be scheduled on particular nodes.

**Taint:** A restriction applied to a node.

**Toleration:** A rule on a Pod that allows it to tolerate a matching taint.

Example use cases:

- Reserving nodes for critical workloads
- Isolating specialized hardware
- Keeping ordinary workloads away from dedicated nodes

A toleration does not guarantee that a Pod will be scheduled on the tainted node. It only allows the Pod to tolerate the matching taint, subject to the remaining scheduling rules.

### 9.5 Quality of Service (QoS)

Kubernetes assigns Pods to QoS classes based on their CPU and memory requests and limits.

The main classes are:

- `Guaranteed`
- `Burstable`
- `BestEffort`

QoS affects resource-pressure behavior, including eviction decisions. It is not itself a scheduler priority system.

---

## 10. Logs

Logs are records of events and messages generated by applications and system components.

They are useful for:

- Troubleshooting application failures
- Debugging container behavior
- Investigating errors
- Monitoring application activity

### Common Commands

View logs from a Pod:

```bash
kubectl logs <pod-name>
```

View logs from a specific container in a multi-container Pod:

```bash
kubectl logs <pod-name> -c <container-name>
```

Follow logs continuously:

```bash
kubectl logs -f <pod-name>
```

View logs from a previous instance of a container:

```bash
kubectl logs <pod-name> -c <container-name> --previous
```

View logs from all Pods matching a label:

```bash
kubectl logs -l app=nginx
```

To inspect resource events, use:

```bash
kubectl get events
```

To inspect the details and recent events for a resource, use:

```bash
kubectl describe pod <pod-name>
```

`kubectl logs` primarily displays container stdout and stderr. Accessing control plane or node system logs may require tools such as `journalctl`, a cloud provider's logging service, or a centralized logging platform.

---

## 11. Services

A Kubernetes Service provides a stable networking abstraction for accessing a group of Pods.

Pods can be replaced and receive new IP addresses. A Service provides a stable virtual IP or DNS name for clients to use.

### Why Do We Need Services?

- Provide stable access to changing Pods.
- Distribute traffic across eligible backend endpoints.
- Support service discovery through DNS.
- Decouple clients from individual Pod IP addresses.

### Types of Services

#### 11.1 ClusterIP

The default Service type.

It provides an internal virtual IP that is generally reachable from within the cluster.

#### 11.2 NodePort

Exposes a Service on a port across the cluster's node IP addresses.

It can provide access from outside the cluster when the network and firewall configuration allow it.

#### 11.3 LoadBalancer

Requests an external load balancer through a compatible cloud provider or load-balancer implementation.

It is commonly used to expose applications externally.

#### 11.4 ExternalName

Maps a Service name to an external DNS name using a DNS CNAME record.

It does not create a normal Service virtual IP or proxy traffic to Pod endpoints.

### Persistent Endpoint

A Service maintains a stable access point even when its backend Pods change.

For example:

```text
Client
  |
  v
Kubernetes Service
  |
  +----> Pod 1
  |
  +----> Pod 2
  |
  +----> Pod 3
```

The Service selects its backend endpoints according to its configuration and selector rules.

---

## 12. Namespaces

A Namespace is a logical partition within a Kubernetes cluster.

Namespaces help organize resources and scope access control, resource quotas, and other policies.

They can be thought of as separate logical workspaces within one cluster, although they do not provide complete security isolation by themselves.

### Common Namespaces

- **default:** Used for resources when no other namespace is specified.
- **kube-system:** Contains many Kubernetes system components.
- **kube-public:** A namespace that is publicly readable by default, commonly used for information that should be discoverable.
- **kube-node-lease:** Contains Lease objects used for node heartbeats.

### Useful Commands

List namespaces:

```bash
kubectl get namespaces
```

List Pods in a specific namespace:

```bash
kubectl get pods -n kube-system
```

Create a namespace:

```bash
kubectl create namespace dev
```

Deploy or inspect resources in a specific namespace:

```bash
kubectl get deployments -n dev
```

---

## 13. Labels and Selectors

### Labels

Labels are key-value pairs attached to Kubernetes objects, including Pods, Services, Deployments, and Nodes.

Purpose:

- Identify and group related resources.
- Organize workloads by application or environment.
- Support filtering and selection.

Example:

```yaml
metadata:
  labels:
    app: backend
    environment: production
```

### Selectors

Selectors are queries that identify Kubernetes objects based on their labels.

Common uses:

- Services use selectors to identify backend Pods.
- Deployments use selectors to manage the Pods associated with their ReplicaSets.
- `kubectl` uses selectors to filter resources.

Example:

```bash
kubectl get pods -l app=backend
```

Labels identify objects; selectors match objects based on their labels.

---

## 14. Ingress and Ingress Controller

### 14.1 Ingress

Ingress is a Kubernetes API resource that defines rules for routing HTTP and HTTPS traffic from outside the cluster to internal Services.

It can support:

- Host-based routing
- Path-based routing
- TLS configuration

An Ingress resource defines routing rules, but it does not implement them by itself.

### 14.2 Ingress Controller

An Ingress Controller watches Ingress resources and implements their routing rules.

It commonly acts as a reverse proxy and can route traffic to different Services based on hostnames or URL paths.

Examples of Ingress Controller implementations include NGINX-based controllers and cloud-provider-specific controllers.

### Why Do We Need Ingress?

Without a shared ingress solution, multiple applications may require separate externally exposed load balancers.

With Ingress, multiple Services can often share an external entry point, helping simplify routing and reduce infrastructure costs.

The exact cost savings depend on the cluster and load-balancer architecture.

### Traffic Flow

```text
User
  |
  v
External Load Balancer
  |
  v
Ingress Controller
  |
  +----> Service A ----> Pods
  |
  +----> Service B ----> Pods
  |
  +----> Service C ----> Pods
```

The Ingress Controller applies the routing rules and forwards traffic to the appropriate Service.

> Note: Ingress is an established API, but the Kubernetes project recommends Gateway API for many new advanced traffic-management use cases. Ingress remains widely used.

---

## 15. DaemonSets

A DaemonSet ensures that a Pod runs on each eligible node, according to its scheduling rules.

Typical behavior:

- When a new eligible node joins, a Pod is created for that node.
- When a node is removed, its corresponding DaemonSet Pod is no longer needed.
- The DaemonSet controller maintains the desired node-level Pod coverage.

### Common Use Cases

- Log collection agents
- Monitoring agents
- Node-level networking components
- Security agents

DaemonSets are intended for node-level workloads, not ordinary application replica scaling.

---

## 16. Jobs and CronJobs

### 16.1 Job

A Job creates and manages Pods that perform a task until the configured completion requirements are met.

Unlike a Deployment, which generally maintains continuously running application Pods, a Job is intended for finite tasks.

### Common Use Cases

- Database migrations
- Batch processing
- Data imports and exports
- One-time administrative tasks

Example:

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: example-job
spec:
  template:
    spec:
      containers:
        - name: task
          image: busybox:1.36
          command: ["sh", "-c", "echo Task completed"]
      restartPolicy: Never
  backoffLimit: 3
```

Apply the Job:

```bash
kubectl apply -f job.yaml
```

Check its status:

```bash
kubectl get jobs
kubectl get pods
```

### 16.2 CronJob

A CronJob creates Jobs on a recurring schedule.

CronJobs are useful for:

- Backups
- Report generation
- Scheduled data processing
- Cleanup tasks

Example:

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: example-cronjob
spec:
  schedule: "*/5 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: task
              image: busybox:1.36
              command: ["sh", "-c", "echo Scheduled task executed"]
          restartPolicy: OnFailure
```

The schedule `"*/5 * * * *"` means the task is scheduled every five minutes.

Apply the CronJob:

```bash
kubectl apply -f cronjob.yaml
```

Check its status:

```bash
kubectl get cronjobs
kubectl get jobs
kubectl get pods
```

CronJobs create Jobs according to a schedule. Actual execution can be affected by settings such as concurrency policy, missed schedules, and starting deadlines.

---

## 17. Quick Revision

| Concept | Main Purpose |
|---|---|
| Cluster | Provides the environment for running Kubernetes workloads |
| Control Plane | Manages cluster state and orchestration |
| Worker Node | Runs application Pods |
| Deployment | Manages application rollouts and ReplicaSets |
| ReplicaSet | Maintains the desired number of Pod replicas |
| Pod | Smallest deployable unit in Kubernetes |
| Container Runtime | Runs containers |
| Kubelet | Ensures assigned Pods run on a node |
| Kube-proxy | Implements Service networking rules when used |
| API Server | Main Kubernetes API entry point |
| Controller Manager | Runs controllers that reconcile desired state |
| Scheduler | Assigns unscheduled Pods to suitable nodes |
| etcd | Stores persistent Kubernetes cluster state |
| HPA | Scales the number of Pod replicas |
| VPA | Adjusts or recommends container resource requests |
| RBAC | Controls authorization for Kubernetes API resources |
| Service | Provides stable access to backend Pods |
| Namespace | Organizes and scopes cluster resources |
| Labels | Identify and group objects |
| Selectors | Match objects using labels |
| Ingress | Defines HTTP/HTTPS routing rules |
| Ingress Controller | Implements Ingress routing rules |
| DaemonSet | Runs Pods on eligible nodes |
| Job | Runs finite tasks to completion |
| CronJob | Schedules recurring Jobs |
| Taints and Tolerations | Control Pod eligibility for scheduling on tainted nodes |
| Affinity and Anti-affinity | Influence or constrain workload placement |
| QoS | Classifies Pods according to resource requests and limits |

---

## 18. Official Documentation

- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Kubernetes Architecture](https://kubernetes.io/docs/concepts/architecture/)
- [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
- [Pods](https://kubernetes.io/docs/concepts/workloads/pods/)
- [Services](https://kubernetes.io/docs/concepts/services-networking/service/)
- [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/)
- [Horizontal Pod Autoscaling](https://kubernetes.io/docs/concepts/workloads/autoscaling/horizontal-pod-autoscale/)
- [RBAC Authorization](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)
- [Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/job/)
- [CronJobs](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)
