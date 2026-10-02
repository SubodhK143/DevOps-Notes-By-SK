# Namespaces

> A Namespace is a logical partition within a Kubernetes cluster that helps organize, isolate, and manage resources.

---

# 1. What is a Namespace?

A **Namespace** is a Kubernetes resource used to divide a cluster into multiple virtual environments.

Resources such as Pods, Services, Deployments, ConfigMaps, and Secrets can be created inside different Namespaces.

> 💡 **Think of a Namespace as a folder that organizes Kubernetes resources.**

---

# 2. Why Kubernetes Uses Namespaces

As Kubernetes clusters grow, managing hundreds or thousands of resources becomes difficult.

Namespaces help by providing:

- Logical organization
- Resource isolation
- Easier administration
- Team separation
- Environment separation (Development, Testing, Production)

Without Namespaces, all resources would exist in a single **default** Namespace, making management much more difficult.

---

# 3. How Namespaces Work

Every Kubernetes resource belongs to a Namespace.

If no Namespace is specified, Kubernetes places the resource in the **default** Namespace.

```text
Kubernetes Cluster
│
├── default
│   ├── Pods
│   ├── Services
│   └── Deployments
│
├── development
│   ├── Pods
│   ├── Services
│   └── Deployments
│
├── production
│   ├── Pods
│   ├── Services
│   └── Deployments
│
└── testing
    ├── Pods
    ├── Services
    └── Deployments
```

> 💡 Resources in different Namespaces can have the same name because they are isolated.

---

# 4. Built-in Namespaces

Kubernetes automatically creates several Namespaces.

| **Namespace** | **Purpose** |
|---------------|-------------|
| `default` | Default Namespace for user-created resources. |
| `kube-system` | Contains Kubernetes system components. |
| `kube-public` | Stores publicly accessible cluster resources. |
| `kube-node-lease` | Stores node heartbeat (lease) information. |

---

# 5. Creating a Namespace

## Using YAML

```yaml
apiVersion: v1
kind: Namespace

metadata:
  name: development
```

Create it:

```bash
kubectl apply -f namespace.yaml
```

---

## Using kubectl

```bash
kubectl create namespace development
```

---

# 6. Working with Namespaces

Create resources inside a Namespace.

```bash
kubectl apply -f deployment.yaml -n development
```

View Pods inside a Namespace.

```bash
kubectl get pods -n development
```

View Services.

```bash
kubectl get svc -n development
```

View all resources.

```bash
kubectl get all -n development
```

View all Namespaces.

```bash
kubectl get namespaces
```

---

# 7. Viewing Resources Across Namespaces

By default, `kubectl` displays resources from the current Namespace.

View Pods in a specific Namespace:

```bash
kubectl get pods -n development
```

View Pods across all Namespaces:

```bash
kubectl get pods -A
```

or

```bash
kubectl get pods --all-namespaces
```

> 💡 `-A` is the short form of `--all-namespaces`.

---

# 8. Switching the Default Namespace

View the current context.

```bash
kubectl config current-context
```

Set the default Namespace for the current context.

```bash
kubectl config set-context --current --namespace=development
```

Verify the current Namespace.

```bash
kubectl config view --minify | grep namespace
```

---

# 9. Namespace Isolation

Namespaces provide **logical isolation** between resources.

Example:

```text
development
│
└── nginx

production
│
└── nginx
```

Both Deployments can have the same name because they exist in different Namespaces.

> 💡 Namespaces organize resources, but they do **not** provide complete security isolation.

---

# 10. Resource Quotas and RBAC

Namespaces become more powerful when combined with additional Kubernetes features.

### Resource Quotas

Limit how much CPU, memory, storage, or the number of resources a Namespace can use.

Example:

- Development team → 2 CPUs
- Production team → 8 CPUs

---

### RBAC (Role-Based Access Control)

RBAC controls **who can access or modify resources** inside a Namespace.

Example:

- Developers → Development Namespace
- QA Team → Testing Namespace
- Operations Team → Production Namespace

This allows multiple teams to safely share the same Kubernetes cluster.

---

# 11. Important Notes

- Namespaces provide **logical isolation**, not complete security isolation.
- Most Kubernetes resources are **Namespace-scoped**.
- Some resources are **cluster-scoped** and do not belong to any Namespace.

Examples of cluster-scoped resources:

- Nodes
- Namespaces
- PersistentVolumes
- StorageClasses

Deleting a Namespace removes almost all resources contained within it.

---

# 12. Creating Your First Namespace

## Step 1

Create a Namespace.

```bash
kubectl create namespace development
```

---

## Step 2

Verify.

```bash
kubectl get namespaces
```

---

## Step 3

Deploy an application inside the Namespace.

```bash
kubectl apply -f deployment.yaml -n development
```

---

## Step 4

Verify all resources.

```bash
kubectl get all -n development
```

---

# 13. Useful kubectl Commands

```bash
# List Namespaces
kubectl get namespaces

# Short form
kubectl get ns

# Create Namespace
kubectl create namespace development

# Describe Namespace
kubectl describe namespace development

# Delete Namespace
kubectl delete namespace development

# List Pods in a Namespace
kubectl get pods -n development

# List all resources in a Namespace
kubectl get all -n development

# List resources across all Namespaces
kubectl get pods -A

# Set default Namespace
kubectl config set-context --current --namespace=development

# View current Namespace
kubectl config view --minify | grep namespace
```

---

# 14. Best Practices

- Create separate Namespaces for Development, Testing, and Production.
- Use meaningful Namespace names.
- Avoid deploying applications inside `kube-system`.
- Keep Kubernetes system components separate from user workloads.
- Use Resource Quotas to prevent one team from consuming all cluster resources.
- Use RBAC to control access between teams.
- Use labels consistently inside each Namespace.

---

# 15. Key Takeaways

- ✅ Namespaces logically organize Kubernetes resources.
- ✅ Every resource belongs to a Namespace unless it is cluster-scoped.
- ✅ The `default` Namespace is used when none is specified.
- ✅ Multiple resources can have the same name in different Namespaces.
- ✅ Namespaces simplify administration and environment separation.
- ✅ Resource Quotas and RBAC work together with Namespaces.
- ✅ Namespaces provide logical organization but not complete security isolation.

---

# 📚 What's Next?

➡️ **07. Deployments**
