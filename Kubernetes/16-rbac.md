# RBAC (Role-Based Access Control)

> RBAC (Role-Based Access Control) is Kubernetes' authorization mechanism used to control who can perform specific actions on cluster resources.

---

# 1. What is RBAC?

**RBAC (Role-Based Access Control)** controls permissions within a Kubernetes cluster.

It determines:

- **Who** can perform an action
- **What** action they can perform
- **Which** resources they can access
- **Where** those permissions apply

> 💡 **Think of RBAC as the permission system that controls access to Kubernetes resources.**

---

# 2. Why Kubernetes Uses RBAC

In a shared Kubernetes cluster, different users and applications require different levels of access.

RBAC provides:

- Secure access control
- Least privilege principle
- Team-based permissions
- Namespace isolation
- Centralized authorization

Without RBAC, every user could potentially modify or delete all cluster resources.

---

# 3. How RBAC Works

RBAC uses four main resources.

```text
User / ServiceAccount
          │
          ▼
Role / ClusterRole
          │
          ▼
RoleBinding / ClusterRoleBinding
          │
          ▼
Permissions Granted
```

> 💡 Permissions are granted by binding a Role or ClusterRole to a user, group, or ServiceAccount.

---

# 4. RBAC Components

| **Component** | **Purpose** |
|---------------|-------------|
| **Role** | Defines permissions within a Namespace |
| **ClusterRole** | Defines permissions across the entire cluster |
| **RoleBinding** | Grants a Role to a user, group, or ServiceAccount |
| **ClusterRoleBinding** | Grants a ClusterRole across the cluster |

---

# 5. Role vs ClusterRole

| **Role** | **ClusterRole** |
|-----------|-----------------|
| Namespace-scoped | Cluster-scoped |
| Access within one Namespace | Access across all Namespaces |
| Used for namespace resources | Used for cluster-wide resources |

---

# 6. RoleBinding vs ClusterRoleBinding

| **RoleBinding** | **ClusterRoleBinding** |
|-----------------|-------------------------|
| Grants access within one Namespace | Grants access across the cluster |
| References a Role or ClusterRole | References a ClusterRole |
| Namespace-scoped | Cluster-scoped |

---

# 7. Anatomy of a Role

## Example

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role

metadata:
  name: pod-reader
  namespace: development

rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list", "watch"]
```

---

## Manifest Fields

| **Field** | **Purpose** |
|-----------|-------------|
| `apiGroups` | Kubernetes API group |
| `resources` | Resources to access |
| `verbs` | Allowed actions |

---

# 8. Anatomy of a RoleBinding

## Example

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding

metadata:
  name: read-pods
  namespace: development

subjects:
  - kind: ServiceAccount
    name: app-sa

roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

---

## Binding Flow

```text
ServiceAccount
        │
        ▼
RoleBinding
        │
        ▼
Role
        │
        ▼
Permissions
```

---

# 9. Common Verbs

RBAC permissions are defined using **verbs**.

| **Verb** | **Description** |
|-----------|-----------------|
| `get` | Read a resource |
| `list` | List multiple resources |
| `watch` | Watch for changes |
| `create` | Create resources |
| `update` | Modify resources |
| `patch` | Partially update resources |
| `delete` | Delete resources |

---

# 10. Common Resources

| **Resource** | **Description** |
|--------------|-----------------|
| `pods` | Pod resources |
| `deployments` | Deployments |
| `services` | Services |
| `configmaps` | ConfigMaps |
| `secrets` | Secrets |
| `namespaces` | Namespaces |

---

# 11. Creating Your First Role

## Step 1

Create the Role.

```bash
kubectl apply -f role.yaml
```

---

## Step 2

Create the RoleBinding.

```bash
kubectl apply -f rolebinding.yaml
```

---

## Step 3

Verify.

```bash
kubectl get roles

kubectl get rolebindings
```

---

# 12. Useful kubectl Commands

```bash
# List Roles
kubectl get roles

# List ClusterRoles
kubectl get clusterroles

# List RoleBindings
kubectl get rolebindings

# List ClusterRoleBindings
kubectl get clusterrolebindings

# Describe a Role
kubectl describe role pod-reader

# Describe a RoleBinding
kubectl describe rolebinding read-pods

# View permissions
kubectl auth can-i get pods

# Explain Role
kubectl explain role

# Explain RoleBinding
kubectl explain rolebinding
```

---

# 13. Best Practices

- Follow the Principle of Least Privilege.
- Grant only the permissions users need.
- Prefer Roles over ClusterRoles whenever possible.
- Use ServiceAccounts for applications.
- Regularly review RBAC permissions.
- Avoid granting cluster-admin unless absolutely necessary.

---

# 14. Key Takeaways

- ✅ RBAC controls authorization in Kubernetes.
- ✅ Roles define permissions within a Namespace.
- ✅ ClusterRoles define cluster-wide permissions.
- ✅ RoleBindings and ClusterRoleBindings assign permissions.
- ✅ ServiceAccounts commonly use RBAC for application access.
- ✅ Follow the Principle of Least Privilege.

---

# 📚 What's Next?

➡️ **17. Network Policies**
