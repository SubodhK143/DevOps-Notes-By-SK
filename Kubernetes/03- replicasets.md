# ReplicaSets

> A ReplicaSet ensures that the desired number of identical Pods are always running in a Kubernetes cluster.

---

# 1. What is a ReplicaSet?

A **ReplicaSet (RS)** is a Kubernetes controller that maintains a specified number of Pod replicas.

If a Pod is:

- Deleted
- Crashes
- Fails
- Becomes unavailable

The ReplicaSet automatically creates a replacement Pod.

> 💡 **Think of a ReplicaSet as a Pod manager that continuously maintains the desired number of running Pods.**

---

# 2. Why Kubernetes Uses ReplicaSets

Pods are **ephemeral**, meaning they can fail or be deleted at any time.

ReplicaSets provide:

- High availability
- Self-healing
- Automatic Pod recreation
- Desired state management
- Horizontal scaling

> 💡 Without a ReplicaSet, deleting a standalone Pod removes it permanently.

---

# 3. How ReplicaSets Work

A ReplicaSet continuously monitors the number of running Pods and compares it with the desired number of replicas.

If the number of running Pods changes, the ReplicaSet automatically creates or removes Pods until the desired state is restored.

For example:

- Desired replicas: **3**
- Running Pods: **2**
- ReplicaSet automatically creates **1 new Pod**

ReplicaSets identify and manage Pods using **Labels** and **Selectors**.

> 💡 A ReplicaSet only manages Pods whose labels match its selector.

---

# 4. Anatomy of a ReplicaSet Manifest

Every ReplicaSet is defined using a YAML manifest.

## Example

```yaml
apiVersion: apps/v1
kind: ReplicaSet

metadata:
  name: nginx-rs

spec:
  replicas: 3

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
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
| `metadata` | Stores the ReplicaSet name and metadata. |
| `replicas` | Specifies the desired number of Pod replicas. |
| `selector` | Identifies which Pods the ReplicaSet manages. |
| `template` | Defines the Pod template used to create new Pods. |

---

# 5. Labels and Selectors

ReplicaSets use **Labels** and **Selectors** to identify which Pods they manage.

## ReplicaSet Selector

```yaml
selector:
  matchLabels:
    app: nginx
```

## Pod Labels

```yaml
metadata:
  labels:
    app: nginx
```

The labels in the Pod template must match the selector. Otherwise, the ReplicaSet will not manage those Pods.

---

# 6. Scaling a ReplicaSet

Increase the number of replicas:

```bash
kubectl scale replicaset nginx-rs --replicas=5
```

Decrease the number of replicas:

```bash
kubectl scale replicaset nginx-rs --replicas=2
```

View ReplicaSets:

```bash
kubectl get rs
```

> 💡 Scaling changes the number of running Pods without modifying the application.

---

# 7. Self-Healing

Suppose a ReplicaSet manages **3 Pods**.

If one Pod is deleted:

```bash
kubectl delete pod <pod-name>
```

The ReplicaSet detects that only **2 Pods** are running and immediately creates a new Pod to restore the desired count.

> 💡 This automatic recovery mechanism is called **self-healing**.

---

# 8. Creating Your First ReplicaSet

## Step 1: Save the Manifest

Save the YAML file as:

```text
replicaset.yaml
```

---

## Step 2: Create the ReplicaSet

```bash
kubectl apply -f replicaset.yaml
```

---

## Step 3: Verify

```bash
kubectl get rs

kubectl get pods
```

---

## Step 4: Test Self-Healing

Delete one of the Pods:

```bash
kubectl delete pod <pod-name>
```

Run the following command again:

```bash
kubectl get pods
```

A replacement Pod is created automatically.

---

# 9. Useful kubectl Commands

```bash
# Create a ReplicaSet
kubectl apply -f replicaset.yaml

# List ReplicaSets
kubectl get rs

# Show detailed information
kubectl describe rs nginx-rs

# Scale a ReplicaSet
kubectl scale rs nginx-rs --replicas=5

# List Pods
kubectl get pods

# Export ReplicaSet YAML
kubectl get rs nginx-rs -o yaml

# Explain ReplicaSet resource
kubectl explain replicaset

# Delete a ReplicaSet
kubectl delete rs nginx-rs

# Delete using the manifest
kubectl delete -f replicaset.yaml
```

---

# 10. Best Practices

- Use **Deployments** instead of creating ReplicaSets directly.
- Use meaningful labels and selectors.
- Keep selectors unique to avoid conflicts.
- Use declarative YAML manifests.
- Validate manifests before applying them.
- Use ReplicaSets only when you need direct control over Pod replicas.

---

# 11. Key Takeaways

- ✅ ReplicaSets maintain the desired number of Pod replicas.
- ✅ ReplicaSets automatically recreate failed or deleted Pods.
- ✅ Labels and selectors determine which Pods are managed.
- ✅ Scaling is achieved by changing the replica count.
- ✅ ReplicaSets provide self-healing capabilities.
- ✅ In production, ReplicaSets are typically managed by Deployments.

---

# 📚 What's Next?

➡️ **04. Deployments**
