# Ingress

> An Ingress is a Kubernetes resource that manages external HTTP and HTTPS traffic to Services inside a cluster using hostnames and URL paths.

---

# 1. What is an Ingress?

An **Ingress** is a Kubernetes networking resource that routes incoming HTTP and HTTPS requests to the appropriate Services.

Unlike a Service, which exposes a single application, an Ingress can route traffic to multiple Services through a single entry point.

> 💡 **Think of an Ingress as the front door or reverse proxy for applications running inside your Kubernetes cluster.**

---

# 2. Why Kubernetes Uses Ingress

Without an Ingress, every application exposed outside the cluster typically requires its own Service of type **LoadBalancer** or **NodePort**.

Ingress provides:

- Single entry point for multiple applications
- Host-based routing
- Path-based routing
- SSL/TLS termination
- Reduced cloud load balancer costs
- Centralized traffic management

---

# 3. How Ingress Works

An Ingress works together with an **Ingress Controller**.

```text
Internet
    │
    ▼
Ingress Controller
    │
    ▼
Ingress
    │
    ▼
Service
    │
    ▼
Pods
```

> 💡 An Ingress resource only defines routing rules. An **Ingress Controller** implements those rules and handles the traffic.

---

# 4. Ingress Controller

An **Ingress Controller** watches Ingress resources and configures a reverse proxy to route traffic.

Common Ingress Controllers:

| **Controller** | **Description** |
|----------------|-----------------|
| NGINX Ingress Controller | Most widely used |
| Traefik | Lightweight reverse proxy |
| HAProxy Ingress | HAProxy-based controller |
| AWS Load Balancer Controller | AWS-native Ingress |
| Kong Ingress Controller | API gateway and Ingress |

> 💡 Without an Ingress Controller, an Ingress resource has no effect.

---

# 5. Routing Types

Ingress supports multiple routing methods.

## Host-Based Routing

Routes traffic based on the requested hostname.

Example:

```text
app.example.com
        │
        ▼
Frontend Service

api.example.com
        │
        ▼
API Service
```

---

## Path-Based Routing

Routes traffic based on the URL path.

Example:

```text
example.com/
        │
        ▼
Frontend Service

example.com/api
        │
        ▼
API Service
```

---

# 6. Anatomy of an Ingress Manifest

## Example

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress

metadata:
  name: app-ingress

spec:
  ingressClassName: nginx

  rules:
    - host: app.example.com

      http:
        paths:
          - path: /
            pathType: Prefix

            backend:
              service:
                name: frontend-service

                port:
                  number: 80
```

---

## Manifest Fields

| **Field** | **Purpose** |
|-----------|-------------|
| `ingressClassName` | Specifies the Ingress Controller |
| `host` | Domain name used for routing |
| `path` | URL path used for routing |
| `backend.service.name` | Destination Service |
| `backend.service.port.number` | Service port |

---

# 7. TLS (HTTPS)

Ingress can terminate HTTPS connections using TLS certificates.

Example:

```yaml
tls:
  - hosts:
      - app.example.com

    secretName: app-tls
```

The TLS certificate is stored as a Kubernetes Secret.

> 💡 TLS termination allows encrypted client connections while simplifying certificate management.

---

# 8. Creating Your First Ingress

## Step 1

Deploy your application and Service.

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

---

## Step 2

Create the Ingress.

```bash
kubectl apply -f ingress.yaml
```

---

## Step 3

Verify.

```bash
kubectl get ingress
```

---

## Step 4

Describe the Ingress.

```bash
kubectl describe ingress app-ingress
```

---

# 9. Service vs Ingress

| **Service** | **Ingress** |
|-------------|-------------|
| Exposes a single application | Routes traffic to multiple Services |
| Provides stable networking for Pods | Manages external HTTP/HTTPS traffic |
| Uses ClusterIP, NodePort, or LoadBalancer | Uses routing rules |
| No hostname or path routing | Supports host and path routing |

---

# 10. Useful kubectl Commands

```bash
# Create an Ingress
kubectl apply -f ingress.yaml

# List Ingress resources
kubectl get ingress

# Short form
kubectl get ing

# Describe an Ingress
kubectl describe ingress app-ingress

# View YAML
kubectl get ingress app-ingress -o yaml

# Delete an Ingress
kubectl delete ingress app-ingress

# Explain Ingress
kubectl explain ingress
```

---

# 11. Best Practices

- Install an Ingress Controller before creating Ingress resources.
- Use host-based routing for multiple applications.
- Use path-based routing to organize application endpoints.
- Enable HTTPS using TLS certificates.
- Keep routing rules simple and maintainable.
- Use meaningful hostnames and paths.

---

# 12. Key Takeaways

- ✅ Ingress manages external HTTP and HTTPS traffic.
- ✅ An Ingress Controller is required for Ingress to function.
- ✅ Ingress supports host-based and path-based routing.
- ✅ A single Ingress can route traffic to multiple Services.
- ✅ TLS enables secure HTTPS connections.
- ✅ Ingress reduces the need for multiple LoadBalancer Services.

---

# 📚 What's Next?

➡️ **16. RBAC**
