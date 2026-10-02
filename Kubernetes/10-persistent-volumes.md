# Persistent Volumes

> A Persistent Volume (PV) is a cluster-wide storage resource that provides persistent storage independent of a Pod's lifecycle.

---

# 1. What is a Persistent Volume?

A **Persistent Volume (PV)** is a piece of storage managed by Kubernetes that exists independently of Pods.

Unlike regular Volumes, a Persistent Volume continues to exist even after the Pod using it is deleted.

A Persistent Volume can be backed by storage such as:

- Local disks
- NFS
- AWS EBS
- Azure Disk
- Google Persistent Disk
- Ceph
- Other storage systems

> 💡 **Think of a Persistent Volume as a reusable storage disk managed by the Kubernetes cluster.**

---

# 2. Why Kubernetes Uses Persistent Volumes

Containers and Pods are **ephemeral**.

If a Pod is deleted:

- Container filesystem is lost.
- `emptyDir` is deleted.
- Application data disappears.

Persistent Volumes solve this by providing:

- Persistent storage
- Data durability
- Storage independent of Pods
- Reusable storage
- Centralized storage management

---

# 3. How Persistent Volumes Work

A Persistent Volume is created at the cluster level.

Pods do **not** use a Persistent Volume directly.

Instead:

```text
Administrator
      │
      ▼
Persistent Volume (PV)
      │
      ▼
Persistent Volume Claim (PVC)
      │
      ▼
Pod
```

> 💡 Pods access Persistent Volumes through **Persistent Volume Claims (PVCs)**.

---

# 4. Persistent Volume Lifecycle

A Persistent Volume follows a lifecycle.

```text
Available
     │
     ▼
Bound
     │
     ▼
Released
     │
     ▼
Available / Failed
```

| **State** | **Description** |
|-----------|-----------------|
| **Available** | Ready to be claimed. |
| **Bound** | Connected to a PersistentVolumeClaim. |
| **Released** | Claim deleted but storage not yet reclaimed. |
| **Failed** | Volume could not be reclaimed automatically. |

---

# 5. Anatomy of a Persistent Volume

## Example

```yaml
apiVersion: v1
kind: PersistentVolume

metadata:
  name: app-pv

spec:
  capacity:
    storage: 5Gi

  accessModes:
    - ReadWriteOnce

  persistentVolumeReclaimPolicy: Retain

  hostPath:
    path: /mnt/data
```

---

## Manifest Fields

| **Field** | **Purpose** |
|-----------|-------------|
| `capacity` | Maximum storage capacity. |
| `accessModes` | Defines how the volume can be mounted. |
| `persistentVolumeReclaimPolicy` | Determines what happens after the claim is deleted. |
| `hostPath` | Storage location on the Worker Node (example only). |

---

# 6. Access Modes

Access modes determine how a volume can be mounted.

| **Access Mode** | **Description** |
|-----------------|-----------------|
| `ReadWriteOnce (RWO)` | Mounted as read-write by one node. |
| `ReadOnlyMany (ROX)` | Mounted as read-only by multiple nodes. |
| `ReadWriteMany (RWX)` | Mounted as read-write by multiple nodes. |
| `ReadWriteOncePod (RWOP)` | Mounted by only one Pod at a time. |

> 💡 Supported access modes depend on the underlying storage provider.

---

# 7. Reclaim Policies

A reclaim policy defines what happens to the storage after its claim is deleted.

| **Policy** | **Description** |
|------------|-----------------|
| `Retain` | Keeps the storage for manual cleanup. |
| `Delete` | Deletes the storage automatically. |
| `Recycle` | Legacy policy (deprecated). |

---

# 8. Static vs Dynamic Provisioning

## Static Provisioning

- Administrator creates the PV manually.
- Pods consume it through a PVC.

---

## Dynamic Provisioning

- Kubernetes automatically creates a PV when a PVC requests storage.
- Requires a **StorageClass**.

> 💡 Dynamic provisioning is the preferred approach in modern Kubernetes clusters.

---

# 9. Creating Your First Persistent Volume

## Step 1

Create the PV.

```bash
kubectl apply -f pv.yaml
```

---

## Step 2

Verify.

```bash
kubectl get pv
```

---

## Step 3

Describe the PV.

```bash
kubectl describe pv app-pv
```

Observe that the PV is in the **Available** state until a PVC claims it.

---

# 10. Useful kubectl Commands

```bash
# Create a Persistent Volume
kubectl apply -f pv.yaml

# List Persistent Volumes
kubectl get pv

# Describe a Persistent Volume
kubectl describe pv app-pv

# View YAML
kubectl get pv app-pv -o yaml

# Delete a Persistent Volume
kubectl delete pv app-pv

# Explain PersistentVolume
kubectl explain persistentvolume
```

---

# 11. Best Practices

- Use Persistent Volumes for data that must survive Pod deletion.
- Prefer dynamic provisioning with StorageClasses.
- Choose the appropriate access mode.
- Select the correct reclaim policy based on application requirements.
- Avoid using `hostPath` for production storage.
- Monitor Persistent Volume usage regularly.

---

# 12. Key Takeaways

- ✅ Persistent Volumes provide cluster-wide persistent storage.
- ✅ Persistent Volumes exist independently of Pods.
- ✅ Pods access Persistent Volumes through PVCs.
- ✅ Access modes control how storage is mounted.
- ✅ Reclaim policies determine what happens after storage is released.
- ✅ Dynamic provisioning is the recommended approach.

---

# 📚 What's Next?

➡️ **11. Persistent Volume Claims**
