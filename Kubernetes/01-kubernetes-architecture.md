# Kubernetes Architecture

> Kubernetes is a container orchestration platform that automates the deployment, scaling, networking, and self-healing of containerized applications.

---

# 1. Why Kubernetes Exists

Docker allows you to build and run containers, but it focuses on running containers on a **single machine**. As applications grow across multiple servers, managing containers manually becomes challenging.

Kubernetes solves this by automatically:

- Scaling applications based on demand
- Scheduling containers across multiple nodes
- Restarting failed containers
- Managing networking and service discovery
- Maintaining the desired state of applications

> 💡 **Docker runs containers. Kubernetes manages containers at scale.**

---

# 2. History

| **Topic** | **Details** |
|-----------|-------------|
| **Created By** | Google |
| **Open-Sourced** | 2014 |
| **Maintained By** | Cloud Native Computing Foundation (CNCF) |
| **Inspired By** | Google's Borg system |
| **Abbreviation** | K8s (8 letters between K and S) |

---

# 3. Kubernetes Architecture

A Kubernetes cluster consists of two major parts:

- **Control Plane** – Manages the cluster and makes decisions.
- **Worker Nodes** – Run containerized applications.

> 📷 **Architecture Diagram**

```text
images/kubernetes-cluster-architecture.png
```

![Kubernetes Architecture](images/kubernetes-cluster-architecture.png)

---

## 3.1 Control Plane Components

The **Control Plane** is the brain of Kubernetes. It manages the cluster, schedules workloads, and ensures the cluster's desired state is maintained.

| **Component** | **Responsibility** |
|--------------|--------------------|
| **API Server** | Entry point for all Kubernetes API requests and cluster communication. |
| **Scheduler** | Assigns newly created Pods to the most suitable Worker Node. |
| **Controller Manager** | Continuously monitors the cluster and maintains the desired state. |
| **etcd** | Distributed key-value store that holds the cluster's configuration and state. |

---

## 3.2 Worker Node Components

A **Worker Node** runs application workloads. It hosts Pods and contains the components required to execute and manage containers.

| **Component** | **Responsibility** |
|--------------|--------------------|
| **kubelet** | Node agent that manages Pods and communicates with the API Server. |
| **kube-proxy** | Handles networking and enables communication between Pods and Services. |
| **Container Runtime** | Pulls container images and runs containers (e.g., **containerd**, **CRI-O**). |

---

# 4. Kubernetes Request Flow

When you execute:

```bash
kubectl apply -f pod.yaml
```

Kubernetes processes the request through the following workflow.

> 📷 **Request Flow Diagram**

```text
images/kubernetes-request-flow.png
```

![Kubernetes Request Flow](images/kubernetes-request-flow.png)

| **Step** | **Action** |
|----------|------------|
| **1** | `kubectl` sends the request to the API Server. |
| **2** | API Server authenticates and authorizes the request. |
| **3** | The manifest is validated. |
| **4** | Desired state is stored in **etcd**. |
| **5** | Scheduler selects the best Worker Node. |
| **6** | kubelet receives the Pod assignment. |
| **7** | Container Runtime pulls the image (if needed). |
| **8** | The container starts, and the Pod status becomes **Running**. |

> 💡 **Every Kubernetes operation passes through the API Server.**

---

# 5. Failure Recovery

One of Kubernetes' most powerful features is **self-healing**.

> 📷 **Failure Recovery Diagram**

```text
images/kubernetes-failure-recovery.png
```

![Kubernetes Failure Recovery](images/kubernetes-failure-recovery.png)

## 5.1 If the API Server Fails

- ❌ New `kubectl` commands cannot be processed.
- ❌ No new deployments or updates can be made.
- ✅ Existing Pods continue running on Worker Nodes.

---

## 5.2 If a Worker Node Fails

- Kubernetes detects the failed node.
- The node is marked as **NotReady**.
- Scheduler selects another healthy Worker Node.
- Pods are recreated automatically.
- Applications continue running with minimal downtime.

> 💡 Kubernetes continuously works to match the **desired state** with the **actual state**.

---

# 6. kubeconfig

`kubeconfig` is the configuration file used by **kubectl** to connect to Kubernetes clusters.

It stores:

- Cluster information
- User credentials
- Contexts
- Certificates

**Default Location**

```bash
~/.kube/config
```

Useful commands:

```bash
kubectl config current-context
kubectl config get-contexts
kubectl config view
```

---

# 7. Built-in Namespaces

Kubernetes provides several built-in namespaces to organize and manage cluster resources.

| **Namespace** | **Purpose** |
|--------------|-------------|
| `default` | Default namespace for user-created resources. |
| `kube-system` | Contains core Kubernetes system components. |
| `kube-public` | Stores publicly readable resources. |
| `kube-node-lease` | Stores node heartbeat (lease) information. |

> 💡 **Best Practice:** Create your own namespaces for applications instead of deploying workloads in `kube-system`.

---

# 8. Common kubectl Commands

```bash
# Display cluster information
kubectl cluster-info

# List all worker nodes
kubectl get nodes

# List all Pods
kubectl get pods -A

# List Pods in the kube-system namespace
kubectl get pods -n kube-system

# Show the current context
kubectl config current-context

# List all available contexts
kubectl config get-contexts

# View the kubeconfig file
kubectl config view
```

---

# 9. Key Takeaways

- ✅ Kubernetes follows a **Control Plane + Worker Node** architecture.
- ✅ The **API Server** is the central communication hub.
- ✅ **etcd** stores the cluster's configuration and desired state.
- ✅ The **Scheduler** decides where Pods should run.
- ✅ **kubelet** manages Pods on each Worker Node.
- ✅ Kubernetes automatically detects failures and recreates workloads.
- ✅ Self-healing and desired state management are core Kubernetes principles.

---

# 📚 What's Next?

Continue learning with the next topics:

- **02. Pods**
- **03. ReplicaSets**
- **04. Deployments**
- **05. Services**
- **06. Namespaces**
- **07. ConfigMaps & Secrets**
