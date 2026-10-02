# DaemonSets

> A DaemonSet ensures that a copy of a Pod runs on every eligible node in a Kubernetes cluster.

---

# 1. What is a DaemonSet?

A **DaemonSet** is a Kubernetes workload controller that automatically runs one Pod on every eligible Worker Node.

Whenever a new node joins the cluster, Kubernetes automatically schedules a DaemonSet Pod on that node.

If a node is removed, its DaemonSet Pod is also removed.

> 💡 **Think of a DaemonSet as "one Pod per node."**

---

# 2. Why Kubernetes Uses DaemonSets

Some applications must run on **every node** instead of only a few replicas.

Examples include:

- Log collection
- Monitoring agents
- Node metrics exporters
- Networking plugins
- Security agents

DaemonSets provide:

- Automatic deployment on every node
- Automatic deployment on newly added nodes
- Consistent node-level services
- Simplified cluster management

---

# 3. How DaemonSets Work

A DaemonSet continuously watches the cluster.

```text
DaemonSet
      │
      ▼
─────────────────────────────
│ Node 1 │  Monitoring Pod  │
─────────────────────────────
│ Node 2 │  Monitoring Pod  │
─────────────────────────────
│ Node 3 │  Monitoring Pod  │
─────────────────────────────
```

If a new node joins:

```text
New Node
    │
    ▼
DaemonSet automatically creates a Pod
```

> 💡 A DaemonSet ensures every eligible node runs exactly one Pod.

---

# 4. Common Use Cases

DaemonSets are commonly used for:

| **Application** | **Purpose** |
|-----------------|-------------|
| Fluentd | Log collection |
| Promtail | Log shipping |
| Node Exporter | Node metrics |
| CNI Plugins | Cluster networking |
| Falco | Runtime security monitoring |

---

# 5. DaemonSet vs Deployment

| **DaemonSet** | **Deployment** |
|---------------|----------------|
| One Pod per eligible node | User-defined number of replicas |
| Automatically scales with nodes | Scales using replica count |
| Used for node-level services | Used for application workloads |
| No `replicas` field | Uses `replicas` field |

---

# 6. Anatomy of a DaemonSet Manifest

## Example

```yaml
apiVersion: apps/v1
kind: DaemonSet

metadata:
  name: node-exporter

spec:
  selector:
    matchLabels:
      app: node-exporter

  template:
    metadata:
      labels:
        app: node-exporter

    spec:
      containers:
        - name: node-exporter
          image: prom/node-exporter:latest
```

---

## Manifest Fields

| **Field** | **Purpose** |
|-----------|-------------|
| `apiVersion` | Kubernetes API version |
| `kind` | Resource type |
| `metadata` | DaemonSet information |
| `selector` | Selects Pods managed by the DaemonSet |
| `template` | Pod template used on every node |

---

# 7. Node Selection

By default, a DaemonSet runs on every eligible Worker Node.

You can restrict it to specific nodes using:

- Node Selectors
- Node Affinity
- Taints and Tolerations

Example:

```yaml
nodeSelector:
  disktype: ssd
```

> 💡 This schedules DaemonSet Pods only on nodes with the matching label.

---

# 8. Creating Your First DaemonSet

## Step 1

Create the DaemonSet.

```bash
kubectl apply -f daemonset.yaml
```

---

## Step 2

Verify.

```bash
kubectl get daemonsets
```

---

## Step 3

View Pods.

```bash
kubectl get pods -o wide
```

You should see one Pod running on each eligible node.

---

# 9. Updating a DaemonSet

Update the container image.

```bash
kubectl set image daemonset/node-exporter \
node-exporter=prom/node-exporter:v1.9.1
```

Check rollout status.

```bash
kubectl rollout status daemonset node-exporter
```

---

# 10. Useful kubectl Commands

```bash
# Create a DaemonSet
kubectl apply -f daemonset.yaml

# List DaemonSets
kubectl get daemonsets

# Short form
kubectl get ds

# Describe a DaemonSet
kubectl describe daemonset node-exporter

# View Pods
kubectl get pods -o wide

# Rollout status
kubectl rollout status daemonset node-exporter

# View YAML
kubectl get daemonset node-exporter -o yaml

# Delete DaemonSet
kubectl delete daemonset node-exporter

# Explain DaemonSet
kubectl explain daemonset
```

---

# 11. Best Practices

- Use DaemonSets only for node-level services.
- Avoid running application workloads as DaemonSets.
- Use Node Selectors or Affinity when only specific nodes require the Pod.
- Monitor DaemonSet Pods regularly.
- Keep resource requests and limits appropriate for node-level agents.

---

# 12. Key Takeaways

- ✅ A DaemonSet runs one Pod on every eligible node.
- ✅ New nodes automatically receive a DaemonSet Pod.
- ✅ DaemonSets are ideal for monitoring, logging, networking, and security agents.
- ✅ DaemonSets do not use a `replicas` field.
- ✅ Node Selectors and Affinity can limit where Pods are scheduled.

---

# 📚 What's Next?

➡️ **13. StatefulSets**
