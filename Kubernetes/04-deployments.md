# Deployments

> A Deployment is a Kubernetes resource that manages ReplicaSets and Pods, providing declarative updates, scaling, self-healing, and rollback capabilities.

---

# 1. What is a Deployment?

A **Deployment** is a higher-level Kubernetes controller that manages **ReplicaSets**, which in turn manage **Pods**.

Instead of creating Pods or ReplicaSets directly, production applications are typically deployed using Deployments.

A Deployment automatically:

- Creates ReplicaSets
- Creates Pods
- Replaces failed Pods
- Scales applications
- Performs rolling updates
- Supports rollbacks

> 💡 **Think of a Deployment as the manager of ReplicaSets, which are the managers of Pods.**

---

# 2. Why Kubernetes Uses Deployments

Managing ReplicaSets manually becomes difficult as applications evolve.

Deployments simplify application management by providing:

- Declarative application management
- Self-healing
- Easy scaling
- Rolling updates
- Rollbacks
- Zero or minimal downtime deployments

> 💡 Deployments are the recommended way to run stateless applications in Kubernetes.

---

# 3. Deployment Hierarchy

A Deployment doesn't create Pods directly.

It follows this hierarchy:

```text
Deployment
      │
      ▼
ReplicaSet
      │
      ▼
Pods
      │
      ▼
Containers
```

> 💡 The Deployment manages the ReplicaSet, and the ReplicaSet manages the Pods.

---

# 4. How Deployments Work

When you apply a Deployment manifest:

1. Kubernetes creates a Deployment.
2. The Deployment creates a ReplicaSet.
3. The ReplicaSet creates the required Pods.
4. Kubernetes continuously monitors the desired state.
5. Failed Pods are recreated automatically.

Whenever the Deployment is updated, Kubernetes creates a **new ReplicaSet** and gradually replaces the old Pods.

---

# 5. Anatomy of a Deployment Manifest

Every Deployment is defined using a YAML manifest.

## Example

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: nginx-deployment

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

---

## Manifest Fields

| **Field** | **Purpose** |
|-----------|-------------|
| `apiVersion` | Kubernetes API version |
| `kind` | Resource type |
| `metadata` | Deployment information |
| `replicas` | Number of Pod replicas |
| `selector` | Identifies managed Pods |
| `template` | Blueprint used to create Pods |

---

# 6. Scaling a Deployment

Increase replicas:

```bash
kubectl scale deployment nginx-deployment --replicas=5
```

Decrease replicas:

```bash
kubectl scale deployment nginx-deployment --replicas=2
```

Verify:

```bash
kubectl get deployments

kubectl get pods
```

---

# 7. Rolling Updates

One of the biggest advantages of Deployments is **Rolling Updates**.

When the application image changes:

```yaml
image: nginx:1.28
```

Kubernetes:

- Creates new Pods
- Gradually removes old Pods
- Keeps the application available
- Minimizes downtime

Update the image:

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.28
```

> 💡 Rolling Updates allow applications to be updated with little or no downtime.

---

# 8. Rollback

If a new version fails, Kubernetes can roll back to the previous version.

View rollout history:

```bash
kubectl rollout history deployment nginx-deployment
```

Rollback:

```bash
kubectl rollout undo deployment nginx-deployment
```

Check rollout status:

```bash
kubectl rollout status deployment nginx-deployment
```

---

# 9. Creating Your First Deployment

## Step 1: Save the Manifest

```text
deployment.yaml
```

---

## Step 2: Create the Deployment

```bash
kubectl apply -f deployment.yaml
```

---

## Step 3: Verify

```bash
kubectl get deployments

kubectl get rs

kubectl get pods
```

---

## Step 4: Update the Deployment

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.28
```

Verify:

```bash
kubectl rollout status deployment nginx-deployment
```

---

# 10. Useful kubectl Commands

```bash
# Create a Deployment
kubectl apply -f deployment.yaml

# List Deployments
kubectl get deployments

# List ReplicaSets
kubectl get rs

# List Pods
kubectl get pods

# Describe a Deployment
kubectl describe deployment nginx-deployment

# Scale a Deployment
kubectl scale deployment nginx-deployment --replicas=5

# Update image
kubectl set image deployment/nginx-deployment nginx=nginx:1.28

# Rollout status
kubectl rollout status deployment nginx-deployment

# Rollout history
kubectl rollout history deployment nginx-deployment

# Rollback
kubectl rollout undo deployment nginx-deployment

# Delete Deployment
kubectl delete deployment nginx-deployment
```

---

# 11. Best Practices

- Use Deployments for stateless applications.
- Use meaningful labels and selectors.
- Prefer declarative YAML over imperative commands.
- Perform rolling updates instead of deleting Pods manually.
- Monitor rollout status after every update.
- Keep replica counts greater than one for high availability.

---

# 12. Key Takeaways

- ✅ Deployments manage ReplicaSets.
- ✅ ReplicaSets manage Pods.
- ✅ Deployments provide self-healing and scaling.
- ✅ Rolling Updates enable near zero-downtime deployments.
- ✅ Rollbacks quickly restore the previous working version.
- ✅ Deployments are the recommended way to run stateless applications in Kubernetes.

---

# 📚 What's Next?

➡️ **05. Services**
