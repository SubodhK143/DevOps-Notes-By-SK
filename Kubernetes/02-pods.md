# Pods

> Pods are the smallest deployable units in Kubernetes. They encapsulate one or more containers that share networking, storage, and lifecycle resources.

---

# 1. What is a Pod?

A **Pod** is the smallest object that Kubernetes creates and manages. It acts as a wrapper around one or more containers.

A Pod can contain:

- One container *(most common)*
- Multiple tightly coupled containers *(Sidecar pattern)*

All containers inside a Pod share:

- IP address
- Network namespace
- Storage volumes
- Lifecycle

> 💡 **Think of a Pod as a wrapper that runs one or more containers together.**

---

# 2. Why Kubernetes Uses Pods

Instead of managing individual containers, Kubernetes manages **Pods**. This abstraction simplifies deployment, scheduling, networking, and lifecycle management.

Pods provide:

- Shared networking
- Shared storage
- Consistent deployment and scheduling
- Simplified resource management
- Self-healing through higher-level controllers (Deployments and ReplicaSets)
---

# 3. Pod Architecture

A Pod consists of one or more containers running together on the same Worker Node.

Most applications run in a **single-container Pod**, while multi-container Pods are typically used for sidecar patterns such as logging or monitoring.

> 📷 **Pod Architecture Diagram**

```text
images/pod-architecture.png
```

```text
Worker Node
│
└── Pod
    ├── Application Container
    └── Sidecar Container (Optional)

Shared:
• IP Address
• Network
• Volumes
```

Containers inside the same Pod communicate through **localhost** and can share data using **Volumes**.

---

# 4. Pod Characteristics

| **Feature** | **Description** |
|------------|-----------------|
| **Smallest Deployable Unit** | Kubernetes schedules Pods, not individual containers. |
| **Shared IP Address** | All containers inside a Pod share a single IP address. |
| **Shared Network** | Containers communicate using `localhost`. |
| **Shared Storage** | Containers can access the same mounted volumes. |
| **Shared Lifecycle** | Containers start, stop, and restart together. |
| **Ephemeral** | Pods can be replaced at any time and should not store persistent data. |

---

# 5. Anatomy of a Pod Manifest

Every Kubernetes resource is defined using a YAML manifest.

## Example

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: nginx-pod
  labels:
    app: nginx

spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
```

## Manifest Fields

| **Field** | **Purpose** |
|-----------|-------------|
| `apiVersion` | Specifies the Kubernetes API version. |
| `kind` | Defines the Kubernetes resource type. |
| `metadata` | Stores resource information such as name, labels, and annotations. |
| `spec` | Defines the desired state of the Pod. |
| `containers` | Defines the containers that run inside the Pod. |

> 💡 After a resource is created, Kubernetes automatically adds fields such as `uid`, `resourceVersion`, and `creationTimestamp`.

---

# 6. Labels

Labels are key-value pairs attached to Kubernetes resources. They help organize, filter, and select resources.

## Example

```yaml
metadata:
  labels:
    app: nginx
    env: dev
```

## Common Commands

```bash
# Show labels
kubectl get pods --show-labels

# Filter Pods by label
kubectl get pods -l app=nginx

# Add a label
kubectl label pod nginx-pod env=prod

# Remove a label
kubectl label pod nginx-pod env-
```

> 💡 **Services use labels to identify which Pods should receive network traffic.**

---

# 7. Declarative vs Imperative

## Declarative (Recommended)

Uses a YAML manifest to define the desired state.

```bash
kubectl apply -f pod.yaml
```

**Best for**

- Production environments
- GitOps workflows
- CI/CD pipelines
- Version control

---

## Imperative

Creates resources directly from the command line.

```bash
kubectl run nginx --image=nginx
```

**Best for**

- Learning
- Quick testing
- Temporary resources

> 💡 **Production environments primarily use the declarative approach.**

---

# 8. Creating Your First Pod

## Step 1: Create a Manifest

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: nginx-pod

spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
```

---

## Step 2: Create the Pod

```bash
kubectl apply -f pod.yaml
```

---

## Step 3: Verify the Pod

```bash
kubectl get pods
```

---

## Step 4: Inspect the Pod

```bash
kubectl describe pod nginx-pod
```

---

# 9. Pod Lifecycle

A Pod moves through different phases during its lifetime.

```text
Pending
   │
   ▼
Running
 ├──► Succeeded
 ├──► Failed
 └──► Unknown
```

| **Phase** | **Description** |
|-----------|-----------------|
| **Pending** | Pod has been accepted but containers have not started yet. |
| **Running** | All containers are running successfully. |
| **Succeeded** | All containers completed successfully. |
| **Failed** | One or more containers terminated with an error. |
| **Unknown** | Kubernetes cannot determine the Pod's current state. |

> 💡 **Standalone Pods are not self-healing. If deleted, they are gone permanently unless managed by a Deployment or ReplicaSet.**

---

# 10. Validation (Dry Run)

Validate manifests before creating resources.

```bash
# Generate a Pod manifest without creating it
kubectl run nginx --image=nginx --dry-run=client -o yaml

# Client-side validation
kubectl apply -f pod.yaml --dry-run=client

# Server-side validation
kubectl apply -f pod.yaml --dry-run=server
```

> 💡 **Using `--dry-run` helps detect errors before resources are created.**

---

# 11. Useful kubectl Commands

```bash
# Create a Pod
kubectl apply -f pod.yaml

# List Pods
kubectl get pods

# List Pods with more details
kubectl get pods -o wide

# Watch Pod status
kubectl get pods -w

# Show detailed information
kubectl describe pod nginx-pod

# View Pod logs
kubectl logs nginx-pod

# Execute commands inside a Pod
kubectl exec -it nginx-pod -- /bin/bash

# Export Pod YAML
kubectl get pod nginx-pod -o yaml

# Explain Pod resource
kubectl explain pod

# Delete a Pod
kubectl delete pod nginx-pod

# Delete using the manifest
kubectl delete -f pod.yaml
```

---

# 12. Best Practices

- Use **Deployments** instead of standalone Pods for production workloads.
- Keep one main application container per Pod.
- Use multi-container Pods only when containers must work closely together.
- Use meaningful labels for organization and selection.
- Avoid storing important data inside Pods.
- Validate manifests before applying them.
- Prefer declarative YAML over imperative commands.

---

# 13. Key Takeaways

- ✅ A Pod is the smallest deployable unit in Kubernetes.
- ✅ Kubernetes schedules Pods, not individual containers.
- ✅ A Pod can contain one or more tightly coupled containers.
- ✅ Containers inside a Pod share networking and storage.
- ✅ YAML manifests define Kubernetes resources.
- ✅ Labels help organize and select resources.
- ✅ Standalone Pods are intended for learning and testing.
- ✅ Use Deployments to manage Pods in production.

---

# 📚 What's Next?

➡️ **03. ReplicaSets**
