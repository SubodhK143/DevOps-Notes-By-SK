# Volumes

> A Volume provides storage that can be shared between containers in a Pod and persists for the lifetime of the Pod.

---

# 1. What is a Volume?

A **Volume** is a storage resource attached to a Pod that allows containers to store and share data.

Unlike the container filesystem, which is temporary, a Volume exists independently of the individual containers within the Pod.

> 💡 **Think of a Volume as a shared storage space for containers inside the same Pod.**

---

# 2. Why Kubernetes Uses Volumes

Containers are **ephemeral**.

When a container restarts:

- Files inside the container are lost.
- Application data disappears.
- Logs and temporary files are removed.

Volumes solve this by providing:

- Shared storage between containers
- Data persistence during container restarts
- Temporary working space
- Configuration storage
- Secret and ConfigMap mounting

---

# 3. How Volumes Work

A Volume is created for a Pod and mounted inside one or more containers.

```text
Pod
│
├── Container A
│      │
│      ▼
│   /data
│
├── Container B
│      │
│      ▼
│   /data
│
└── Shared Volume
```

Both containers can read and write to the same mounted Volume.

---

# 4. Volume Lifecycle

A Volume is tied to the **Pod**, not the individual containers.

- Container restarts → ✅ Volume data remains.
- Pod deletion → ❌ Volume is deleted (depending on the volume type).

> 💡 Volumes survive container restarts but may not survive Pod deletion.

---

# 5. Common Volume Types

Kubernetes supports many Volume types.

| **Volume Type** | **Purpose** |
|-----------------|-------------|
| `emptyDir` | Temporary storage shared by containers in a Pod. |
| `hostPath` | Mounts a directory from the Worker Node. |
| `configMap` | Mounts ConfigMap data as files. |
| `secret` | Mounts Secret data securely. |
| `persistentVolumeClaim` | Connects a Pod to persistent storage. |

> 💡 Persistent Volumes and PersistentVolumeClaims are covered in the next notes.

---

# 6. Anatomy of a Volume

Volumes are defined in the Pod specification and mounted inside containers.

## Example

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: volume-demo

spec:
  containers:
    - name: nginx
      image: nginx
      volumeMounts:
        - name: app-storage
          mountPath: /usr/share/nginx/html

  volumes:
    - name: app-storage
      emptyDir: {}
```

---

## Manifest Fields

| **Field** | **Purpose** |
|-----------|-------------|
| `volumes` | Defines available Volumes for the Pod. |
| `volumeMounts` | Mounts a Volume inside a container. |
| `name` | Connects the Volume with its mount. |
| `mountPath` | Directory where the Volume is mounted. |

---

# 7. emptyDir Volume

`emptyDir` is the simplest Volume type.

Features:

- Created when a Pod starts.
- Shared by all containers in the Pod.
- Deleted when the Pod is removed.
- Commonly used for temporary files and caching.

Example:

```yaml
volumes:
  - name: cache
    emptyDir: {}
```

---

# 8. hostPath Volume

A `hostPath` Volume mounts a directory from the Worker Node into the Pod.

Example:

```yaml
volumes:
  - name: logs
    hostPath:
      path: /var/log
```

> 💡 `hostPath` is mainly used for development, testing, or specific system workloads. It is generally not recommended for production applications.

---

# 9. ConfigMap and Secret Volumes

ConfigMaps and Secrets can also be mounted as Volumes.

Example:

```yaml
volumes:
  - name: config
    configMap:
      name: app-config

  - name: credentials
    secret:
      secretName: db-secret
```

Each key becomes a separate file inside the mounted directory.

---

# 10. Creating Your First Volume

## Step 1: Create the Pod

```bash
kubectl apply -f volume-pod.yaml
```

---

## Step 2: Verify

```bash
kubectl get pods
```

---

## Step 3: Inspect the Pod

```bash
kubectl describe pod volume-demo
```

---

## Step 4: Verify the Mounted Volume

```bash
kubectl exec -it volume-demo -- /bin/sh
```

Inside the container:

```bash
ls /usr/share/nginx/html
```

---

# 11. Useful kubectl Commands

```bash
# Create a Pod
kubectl apply -f volume-pod.yaml

# List Pods
kubectl get pods

# Describe Pod
kubectl describe pod volume-demo

# Execute into a Pod
kubectl exec -it volume-demo -- /bin/sh

# View Pod YAML
kubectl get pod volume-demo -o yaml

# Delete Pod
kubectl delete pod volume-demo

# Explain Pod Volumes
kubectl explain pod.spec.volumes
```

---

# 12. Best Practices

- Use Volumes instead of writing data inside containers.
- Use `emptyDir` only for temporary storage.
- Avoid using `hostPath` for production workloads.
- Mount ConfigMaps and Secrets instead of hardcoding configuration.
- Use PersistentVolumeClaims for data that must survive Pod deletion.

---

# 13. Key Takeaways

- ✅ Volumes provide shared storage for containers in a Pod.
- ✅ Volumes survive container restarts.
- ✅ `emptyDir` provides temporary Pod storage.
- ✅ `hostPath` mounts directories from the Worker Node.
- ✅ ConfigMaps and Secrets can be mounted as Volumes.
- ✅ Use PersistentVolumeClaims for persistent application data.

---

# 📚 What's Next?

➡️ **10. Persistent Volumes**
