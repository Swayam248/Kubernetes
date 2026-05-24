# Kubernetes CKA Topics

## 1. Cluster Architecture, Installation & Configuration

### Kubernetes Architecture (Control Plane + Worker Nodes)

#### Components:
- API Server  
- Scheduler  
- Controller Manager  
- etcd  
- Kubelet  
- Kube-proxy  

### Cluster Setup:
- kubeadm init  
- kubeadm join  

- Cluster upgrades  

### etcd:
- Backup & Restore  
- Managing Certificates  

### Other:
- API access & kubeconfig  
- Static Pods  

---

## 2. Workloads & Scheduling 

- Pod creation & lifecycle  

### Multi-container Pods:
- Sidecar containers  
- Init containers  

### Controllers:
- Deployments (rolling updates, rollbacks)  
- DaemonSets  
- StatefulSets  
- Jobs  
- CronJobs  

### Metadata:
- Labels  
- Selectors  
- Annotations  

### Scheduling Concepts:
- NodeSelector  
- Node Affinity / Anti-Affinity  
- Pod Affinity / Anti-Affinity  
- Taints & Tolerations  
- Resource Requests & Limits  
- Manual Scheduling  

---

## 3. Services & Networking 

- Cluster Networking basics  
- Pod-to-Pod communication  

### Services:
- ClusterIP  
- NodePort  
- LoadBalancer  

### DNS:
- DNS in Kubernetes  
- CoreDNS  

### Routing:
- Ingress  
- Ingress Controllers  

### Networking:
- Network Policies  
- CNI Plugins (basic understanding)  

---

## 4. Storage 

### Volumes:
- emptyDir  
- hostPath  

### Persistent Storage:
- Persistent Volumes (PV)  
- Persistent Volume Claims (PVC)  
- Storage Classes  
- Dynamic Provisioning  

### Volume Concepts:
- Volume Mounts  
- Access Modes:
  - RWO (ReadWriteOnce)  
  - RWX (ReadWriteMany)  

---

## 5. Troubleshooting (IMPORTANT)

### Application Failures:
- CrashLoopBackOff  
- ImagePullBackOff  

### Pod Issues:
- Logs & events  
  - kubectl logs  
  - kubectl describe  

### Node Issues:
- Node NotReady  

### Networking Issues:
- DNS not resolving  
- Service not reachable  

### Control Plane:
- Control Plane troubleshooting  
- etcd failure scenarios  

### Debugging Commands:
- kubectl exec  
- kubectl logs  
- kubectl describe  

### Cluster Health:
- Checking cluster health  

---

## 6. Security (Basic for CKA)

(Not as deep as CKS, but still important)

### RBAC:
- Roles  
- ClusterRoles  
- RoleBindings  

### Identity:
- Service Accounts  

### Secrets:
- Secrets  

### Other:
- TLS basics  
- Security Contexts (basic usage)  

---

## 7. Cluster Maintenance

- Drain nodes  
- Cordon nodes  
- Uncordon nodes  

- Cluster upgrades  
- OS upgrades impact  
- Backup & restore  

---

## 8. Configuration

- ConfigMaps  
- Secrets  

### Usage:
- Environment variables  
- Command & args in containers  

---

## 9. kubectl Mastery (IMPORTANT)

### Imperative Commands:
- kubectl run  
- kubectl create  
- kubectl expose  

### YAML Generation:
- --dry-run=client -o yaml  

### Editing Resources:
- kubectl edit  

### Queries:
- JSONPath queries  

---

## 10. Misc 

- Namespaces  
- Resource Quotas  
- LimitRanges  
- PodDisruptionBudgets (basic)  
- Init Containers  
- Sidecar Containers  
