# Network Policies

> A NetworkPolicy is a Kubernetes resource that controls how Pods communicate with each other and with external networks using ingress and egress rules.

---

# 1. What is a NetworkPolicy?

A **NetworkPolicy** defines rules that allow or deny network traffic between Pods and external endpoints.

By default, Kubernetes allows all Pods to communicate with each other. NetworkPolicies let you restrict that communication.

> 💡 **Think of a NetworkPolicy as a firewall for Pods.**

---

# 2. Why Kubernetes Uses Network Policies

In many applications, not every Pod should communicate with every other Pod.

For example:

- A frontend Pod should communicate with the backend.
- A backend Pod should communicate with the database.
- External users should never access the database directly.

NetworkPolicies provide:

- Pod-level firewall rules
- Secure communication
- Traffic isolation
- Least privilege networking
- Compliance with security requirements

---

# 3. How Network Policies Work

A NetworkPolicy selects Pods using labels and defines what traffic is allowed.

```text
Client
   │
   ▼
Frontend Pod
   │
   ▼
Backend Pod
   │
   ▼
Database Pod
```

Only traffic explicitly allowed by the NetworkPolicy is permitted.

> 💡 A NetworkPolicy affects only the Pods selected by its `podSelector`.

---

# 4. Ingress vs Egress

NetworkPolicies control two types of traffic.

| **Traffic Type** | **Description** |
|------------------|-----------------|
| **Ingress** | Incoming traffic to a Pod |
| **Egress** | Outgoing traffic from a Pod |

---

# 5. Default Behavior

Without a NetworkPolicy:

```text
All Pods
      │
      ▼
Can communicate freely
```

After applying a restrictive NetworkPolicy:

```text
Only explicitly allowed traffic
```

> 💡 Once a Pod is selected by a NetworkPolicy, traffic not explicitly allowed is denied.

---

# 6. Anatomy of a NetworkPolicy

## Example

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy

metadata:
  name: backend-policy

spec:
  podSelector:
    matchLabels:
      app: backend

  policyTypes:
    - Ingress

  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: frontend
```

---

## Manifest Fields

| **Field** | **Purpose** |
|-----------|-------------|
| `podSelector` | Selects the Pods the policy applies to |
| `policyTypes` | Specifies Ingress, Egress, or both |
| `ingress` | Defines allowed incoming traffic |
| `egress` | Defines allowed outgoing traffic |

---

# 7. Pod Selectors

NetworkPolicies use labels to identify Pods.

Example:

```yaml
podSelector:
  matchLabels:
    app: backend
```

This policy applies only to Pods with the label:

```yaml
labels:
  app: backend
```

---

# 8. Ingress Policy Example

Allow only frontend Pods to access backend Pods.

```text
Frontend
     │
     ▼
Backend
```

All other Pods are denied.

---

# 9. Egress Policy Example

Allow backend Pods to communicate only with the database.

```text
Backend
     │
     ▼
Database
```

All other outbound connections are denied.

---

# 10. Creating Your First NetworkPolicy

## Step 1

Create the NetworkPolicy.

```bash
kubectl apply -f networkpolicy.yaml
```

---

## Step 2

Verify.

```bash
kubectl get networkpolicies
```

---

## Step 3

Describe the policy.

```bash
kubectl describe networkpolicy backend-policy
```

---

# 11. Important Notes

- NetworkPolicies work only if your Container Network Interface (CNI) plugin supports them.
- Popular CNI plugins that support NetworkPolicies include:
  - Calico
  - Cilium
  - Weave Net (with policy support)
- Pods not selected by a NetworkPolicy continue using the default network behavior.

> 💡 If your CNI plugin does not support NetworkPolicies, the rules will not be enforced.

---

# 12. Useful kubectl Commands

```bash
# Create a NetworkPolicy
kubectl apply -f networkpolicy.yaml

# List NetworkPolicies
kubectl get networkpolicies

# Short form
kubectl get netpol

# Describe a NetworkPolicy
kubectl describe networkpolicy backend-policy

# View YAML
kubectl get networkpolicy backend-policy -o yaml

# Delete a NetworkPolicy
kubectl delete networkpolicy backend-policy

# Explain NetworkPolicy
kubectl explain networkpolicy
```

---

# 13. Best Practices

- Apply the Principle of Least Privilege.
- Allow only required Pod-to-Pod communication.
- Use labels consistently.
- Test policies before deploying to production.
- Combine NetworkPolicies with RBAC for stronger security.
- Document network rules clearly.

---

# 14. Key Takeaways

- ✅ NetworkPolicies control Pod network traffic.
- ✅ They act like firewalls for Pods.
- ✅ Policies use labels to select Pods.
- ✅ Ingress controls incoming traffic.
- ✅ Egress controls outgoing traffic.
- ✅ Traffic not explicitly allowed is denied for selected Pods.
- ✅ NetworkPolicies require a compatible CNI plugin.

---

# 📚 What's Next?

➡️ **18. Helm**
