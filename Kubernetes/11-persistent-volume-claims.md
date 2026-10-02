# Persistent Volume Claims

> A Persistent Volume Claim (PVC) is a request for persistent storage made by a Pod. It allows applications to use storage without knowing the underlying storage implementation.

---

# 1. What is a Persistent Volume Claim?

A **Persistent Volume Claim (PVC)** is a Kubernetes resource that requests storage from a **Persistent Volume (PV)**.

Instead of connecting directly to a Persistent Volume, Pods use a PVC.

> 💡 **Think of a PVC as a storage request made by an application, while the Persistent Volume is the actual storage provided by the cluster.**

---

# 2. Why Kubernetes Uses PVCs

Applications should not depend on specific storage devices.

PVCs provide:

- Storage abstraction
- Decoupling applications from storage
- Automatic volume binding
- Easier storage management
- Portable application manifests

Without PVCs, every Pod would need to know where storage is physically located.

---

# 3. How PVCs Work

A Pod never connects directly to a Persistent Volume.

Instead, Kubernetes binds a matching Persistent Volume to the PVC.

```text
Pod
 │
 ▼
Persistent Volume Claim (PVC)
 │
 ▼
Persistent Volume (PV)
 │
 ▼
Storage
```

> 💡 The Pod only knows about the PVC. Kubernetes handles the connection to the Persistent Volume.

---

# 4. PVC Binding Process

When a PVC is created, Kubernetes searches for a matching Persistent Volume.

Matching is based on:

- Storage capacity
- Access mode
- StorageClass (if specified)

If a matching PV is found:

```text
PVC
   │
   ▼
Bound
   │
   ▼
Pod can use storage
```

If no matching PV exists:

```text
PVC
   │
   ▼
Pending
```

The PVC remains in the **Pending** state until storage becomes available.

---

# 5. Anatomy of a PVC Manifest

## Example

```yaml
apiVersion: v1
kind: PersistentVolumeClaim

metadata:
  name: app-pvc

spec:
  accessModes:
    - ReadWriteOnce

  resources:
    requests:
      storage: 5Gi
```

---

## Manifest Fields

| **Field** | **Purpose** |
|-----------|-------------|
| `apiVersion` | Kubernetes API version |
| `kind` | Resource type |
| `metadata` | PVC information |
| `accessModes` | Requested access mode |
| `resources.requests.storage` | Requested storage size |

---

# 6. Using a PVC in a Pod

A Pod references the PVC instead of the Persistent Volume.

Example:

```yaml
volumes:
  - name: app-storage
    persistentVolumeClaim:
      claimName: app-pvc
```

The Volume is then mounted inside the container.

```yaml
volumeMounts:
  - name: app-storage
    mountPath: /data
```

---

# 7. PVC Status

A PVC moves through different states.

| **Status** | **Description** |
|------------|-----------------|
| **Pending** | Waiting for a matching Persistent Volume. |
| **Bound** | Successfully connected to a Persistent Volume. |
| **Lost** | The associated Persistent Volume is unavailable. |

Verify:

```bash
kubectl get pvc
```

---

# 8. Creating Your First PVC

## Step 1

Create the PVC.

```bash
kubectl apply -f pvc.yaml
```

---

## Step 2

Verify.

```bash
kubectl get pvc
```

---

## Step 3

Describe the PVC.

```bash
kubectl describe pvc app-pvc
```

---

## Step 4

Verify the Persistent Volume.

```bash
kubectl get pv
```

You should see the PV status change from **Available** to **Bound**.

---

# 9. Static vs Dynamic Provisioning

## Static Provisioning

- Administrator creates the Persistent Volume.
- PVC binds to an existing PV.

---

## Dynamic Provisioning

- PVC requests storage.
- Kubernetes automatically creates a Persistent Volume.
- Requires a StorageClass.

> 💡 Dynamic provisioning is the preferred approach in modern Kubernetes clusters.

---

# 10. Useful kubectl Commands

```bash
# Create a PVC
kubectl apply -f pvc.yaml

# List PVCs
kubectl get pvc

# Describe a PVC
kubectl describe pvc app-pvc

# List Persistent Volumes
kubectl get pv

# View PVC YAML
kubectl get pvc app-pvc -o yaml

# Delete a PVC
kubectl delete pvc app-pvc

# Explain PVC
kubectl explain persistentvolumeclaim
```

---

# 11. Best Practices

- Use PVCs instead of referencing Persistent Volumes directly.
- Request only the storage you need.
- Choose the appropriate access mode.
- Prefer dynamic provisioning when available.
- Monitor PVC status regularly.
- Use meaningful names for Persistent Volume Claims.

---

# 12. Persistent Volumes vs Persistent Volume Claims

| **Persistent Volume (PV)** | **Persistent Volume Claim (PVC)** |
|----------------------------|-----------------------------------|
| Actual storage resource | Request for storage |
| Created by an administrator or dynamically | Created by an application or user |
| Supplies storage | Consumes storage |
| Cluster resource | Namespace resource |

> 💡 A **Persistent Volume provides storage**, while a **Persistent Volume Claim requests and consumes that storage**.

---

# 13. Key Takeaways

- ✅ A PVC requests persistent storage.
- ✅ Pods use PVCs instead of Persistent Volumes directly.
- ✅ Kubernetes automatically binds matching PVs to PVCs.
- ✅ PVCs simplify storage management.
- ✅ Dynamic provisioning automatically creates storage when needed.
- ✅ PVCs make applications portable across different storage providers.

---

# 📚 What's Next?

➡️ **12. DaemonSets**
