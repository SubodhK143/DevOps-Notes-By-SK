# StatefulSets

> A StatefulSet is a Kubernetes workload controller designed for stateful applications that require stable identities, persistent storage, and ordered deployment.

---

# 1. What is a StatefulSet?

A **StatefulSet** is a Kubernetes controller used to manage applications that require:

- Stable Pod names
- Stable network identities
- Persistent storage
- Ordered deployment and scaling

Unlike Deployments, StatefulSets ensure each Pod keeps its identity even after being restarted.

> 💡 **Think of a StatefulSet as a Deployment with memory and identity.**

---

# 2. Why Kubernetes Uses StatefulSets

Many applications cannot use randomly created Pods.

Examples include:

- MySQL
- PostgreSQL
- MongoDB
- Cassandra
- Kafka
- ZooKeeper
- etcd

These applications require:

- Persistent data
- Predictable Pod names
- Stable network identities
- Ordered startup and shutdown

---

# 3. When to Use StatefulSets

Use a StatefulSet when your application requires:

- Stable Pod identities
- Persistent storage
- Ordered deployment and scaling
- Direct communication between specific Pods

Typical use cases include:

- Database clusters
- Distributed systems
- Message brokers
- Stateful applications

For stateless applications such as web servers, APIs, and frontend applications, use a **Deployment** instead.

---

# 4. How StatefulSets Work

Each Pod receives its own unique identity.

```text
StatefulSet
      │
      ▼
database-0
database-1
database-2
```

Each Pod:

- Has a unique hostname
- Has its own Persistent Volume
- Keeps the same identity after restart

> 💡 If a StatefulSet Pod is recreated, it keeps the same name and reconnects to its existing Persistent Volume.

---

# 5. StatefulSet vs Deployment

| **StatefulSet** | **Deployment** |
|-----------------|----------------|
| Stable Pod names | Random Pod names |
| Stable network identity | Dynamic identity |
| Dedicated Persistent Volume per Pod | Shared or ephemeral storage |
| Ordered deployment and scaling | Parallel deployment and scaling |
| Designed for stateful applications | Designed for stateless applications |

---

# 6. Stable Pod Identity

Pods receive predictable names.

Example:

```text
database-0
database-1
database-2
```

Even after restarting:

```text
database-0
```

remains

```text
database-0
```

Every Pod also receives a stable hostname and DNS name through the Headless Service.

Example:

```text
database-0
database-1
database-2
```

This predictable naming is essential for clustered databases and distributed systems where nodes communicate directly with one another.

---

# 7. Stable Storage

Each Pod gets its own dedicated Persistent Volume.

```text
database-0
      │
      ▼
PVC-0
      │
      ▼
PV-0

database-1
      │
      ▼
PVC-1
      │
      ▼
PV-1

database-2
      │
      ▼
PVC-2
      │
      ▼
PV-2
```

Unlike Deployments, Pods do **not** share storage.

Each Pod always reconnects to its own Persistent Volume after restarting.

> 💡 StatefulSets preserve application data by reconnecting recreated Pods to their existing Persistent Volumes.

---

## Persistent Volume Claims

StatefulSets automatically create one PersistentVolumeClaim (PVC) for every Pod using **volumeClaimTemplates**.

Example:

```text
database-storage-database-0

database-storage-database-1

database-storage-database-2
```

Even if the StatefulSet is scaled down, these PVCs are **not deleted automatically**.

This prevents accidental data loss.

> 💡 Each Pod receives its own dedicated PVC. Multiple Pods do not share the same Persistent Volume by default.

---

# 8. Ordered Deployment and Scaling

Unlike Deployments, StatefulSets create Pods one at a time.

Creation order:

```text
database-0
      │
      ▼
database-1
      │
      ▼
database-2
```

A new Pod is created only after the previous Pod is **Running** and **Ready**.

---

Scaling down happens in reverse order.

```text
database-2
      │
      ▼
database-1
      │
      ▼
database-0
```

This behavior protects clustered applications by allowing graceful startup and shutdown.

> 💡 Ordered deployment is one of the biggest differences between Deployments and StatefulSets.

---

# 9. Anatomy of a StatefulSet Manifest

Every StatefulSet is defined using a YAML manifest.

## Example

```yaml
apiVersion: apps/v1
kind: StatefulSet

metadata:
  name: database

spec:
  serviceName: database

  replicas: 3

  selector:
    matchLabels:
      app: database

  template:
    metadata:
      labels:
        app: database

    spec:
      containers:
        - name: mysql
          image: mysql:8
          ports:
            - containerPort: 3306

  volumeClaimTemplates:
    - metadata:
        name: database-storage

      spec:
        accessModes:
          - ReadWriteOnce

        resources:
          requests:
            storage: 10Gi
```

---

## Manifest Fields

| **Field** | **Purpose** |
|-----------|-------------|
| `serviceName` | Headless Service used for stable network identity |
| `replicas` | Number of Pods |
| `selector` | Identifies managed Pods |
| `template` | Pod template |
| `volumeClaimTemplates` | Automatically creates one PVC per Pod |

---

# 10. Headless Service

A StatefulSet requires a **Headless Service**.

Unlike a normal Service, a Headless Service does **not** perform load balancing.

> 💡 Unlike a regular Service, a Headless Service does not assign a ClusterIP. Instead, DNS resolves directly to the individual Pod IP addresses, allowing applications to communicate with specific Pods.

Example:

```yaml
apiVersion: v1
kind: Service

metadata:
  name: database

spec:
  clusterIP: None

  selector:
    app: database

  ports:
    - port: 3306
```

---

## Stable DNS

Each Pod receives its own DNS name.

Example:

```text
database-0.database.default.svc.cluster.local

database-1.database.default.svc.cluster.local

database-2.database.default.svc.cluster.local
```

Applications can communicate directly with specific Pods using these DNS names.

> 💡 Headless Services provide stable network identities instead of load balancing.

---

# 11. Creating Your First StatefulSet

## Step 1

Create the Headless Service.

```bash
kubectl apply -f headless-service.yaml
```

---

## Step 2

Create the StatefulSet.

```bash
kubectl apply -f statefulset.yaml
```

---

## Step 3

Verify the StatefulSet.

```bash
kubectl get statefulsets
```

---

## Step 4

Verify the Pods.

```bash
kubectl get pods
```

Expected output:

```text
database-0

database-1

database-2
```

---

## Step 5

Verify the PersistentVolumeClaims.

```bash
kubectl get pvc
```

Expected output:

```text
database-storage-database-0

database-storage-database-1

database-storage-database-2
```

---

# 12. Useful kubectl Commands

```bash
# Create StatefulSet
kubectl apply -f statefulset.yaml

# List StatefulSets
kubectl get statefulsets

# Short form
kubectl get sts

# Describe StatefulSet
kubectl describe statefulset database

# Scale StatefulSet
kubectl scale statefulset database --replicas=5

# List Pods
kubectl get pods

# List PersistentVolumeClaims
kubectl get pvc

# Delete StatefulSet
kubectl delete statefulset database

# Explain StatefulSet
kubectl explain statefulset
```

---

# 13. Best Practices

- Use StatefulSets only for stateful applications.
- Always create a Headless Service.
- Use a dedicated Persistent Volume for each Pod.
- Use StatefulSets for databases and distributed systems.
- Avoid StatefulSets for stateless applications.
- Monitor Persistent Volume usage regularly.
- Back up persistent data before deleting StatefulSets.

---

# 14. Key Takeaways

- ✅ StatefulSets manage stateful applications.
- ✅ Every Pod has a stable identity and hostname.
- ✅ Every Pod gets its own Persistent Volume.
- ✅ Pods are created sequentially and deleted in reverse order.
- ✅ Headless Services provide stable DNS names.
- ✅ StatefulSets are ideal for databases and distributed systems.
- ✅ Persistent data survives Pod recreation.

---

# 📚 What's Next?

➡️ **14. Jobs & CronJobs**


