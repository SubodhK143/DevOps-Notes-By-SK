# Services

> A Service provides a stable network endpoint for accessing a group of Pods, even if the Pods are recreated or their IP addresses change.

---

# 1. What is a Service?

A **Service** is a Kubernetes resource that exposes one or more Pods using a stable IP address and DNS name.

Unlike Pods, which are **ephemeral** and receive new IP addresses when recreated, a Service provides a permanent way to access applications.

> 💡 **Think of a Service as a permanent front door for your Pods.**

---

# 2. Why Kubernetes Uses Services

Pods are temporary.

Whenever a Pod is recreated:

- Pod IP changes
- Existing clients lose connection
- Applications become difficult to access

Services solve this by providing:

- Stable IP address
- Stable DNS name
- Automatic load balancing
- Service discovery
- Decoupling clients from Pods

---

# 3. How Services Work

A Service does not communicate with Pods directly. Instead, it uses **label selectors** to identify matching Pods and forwards traffic to them through Kubernetes networking.

```text
Client
   │
   ▼
Service
   │
   ▼
Label Selector
   │
   ▼
Pods
```

> 💡 Whenever Pods are created, deleted, or recreated, the Service automatically updates the available endpoints.

---

# 4. DNS-Based Service Discovery

Every Service automatically receives a DNS name, allowing applications to communicate without using Pod IP addresses.

**Full DNS format**

```text
<service-name>.<namespace>.svc.cluster.local
```

Pods in the **same namespace** can usually communicate using only the Service name.

Example:

```text
http://nginx-service
```

Across **different namespaces**, use the fully qualified DNS name.

```text
http://nginx-service.default.svc.cluster.local
```

Kubernetes uses **CoreDNS** to automatically resolve the Service name to the Service's ClusterIP.

> 💡 Applications should communicate using **Service names**, not Pod IP addresses.

---

# 5. Types of Kubernetes Services

Kubernetes provides four Service types.

| **Type** | **Purpose** |
|----------|-------------|
| **ClusterIP** | Exposes the application only inside the cluster. |
| **NodePort** | Exposes the application on a port of every Worker Node. |
| **LoadBalancer** | Exposes the application using an external cloud load balancer. |
| **ExternalName** | Maps the Service to an external DNS name. |

---

# 6. Service Type Hierarchy

The Service types are layered. Higher-level Service types include the functionality of the lower-level types.

```text
LoadBalancer
      │
      ▼
NodePort
      │
      ▼
ClusterIP
```

This means:

- Every **NodePort** Service also has a **ClusterIP**.
- Every **LoadBalancer** Service builds on **NodePort** and **ClusterIP** (on platforms that support external load balancers).

> 💡 **ExternalName** is different from the other Service types. It maps a Service to an external DNS name and does not route traffic to Pods.

---

## 6.1 ClusterIP

**Default Service type**

- Accessible only inside the cluster
- Used for communication between applications
- No external access

Example:

```text
Pod A
   │
   ▼
ClusterIP Service
   │
   ▼
Pod B
```

---

## 6.2 NodePort

Exposes an application outside the cluster through a port on every Worker Node.

Default NodePort range:

```text
30000 - 32767
```

Example:

```text
Browser
    │
http://NodeIP:30080
    │
    ▼
NodePort Service
    │
    ▼
Pods
```

---

## 6.3 LoadBalancer

Creates an external Load Balancer provided by the cloud platform.

Commonly used in:

- AWS
- Azure
- Google Cloud

Example:

```text
Internet
    │
    ▼
Load Balancer
    │
    ▼
Service
    │
    ▼
Pods
```

> 💡 **Note:** On local clusters such as **Minikube**, **Kind**, or **Docker Desktop**, the `EXTERNAL-IP` may remain `<pending>` because no cloud load balancer is available.

---

## 6.4 ExternalName

Instead of forwarding traffic to Pods, this Service redirects requests to an external DNS name.

Example:

```yaml
type: ExternalName
externalName: api.example.com
```

---

# 7. Anatomy of a Service Manifest

Every Service is defined using a YAML manifest.

## Example

```yaml
apiVersion: v1
kind: Service

metadata:
  name: nginx-service

spec:
  selector:
    app: nginx

  ports:
    - protocol: TCP
      port: 80
      targetPort: 80

  type: ClusterIP
```

---

## Manifest Fields

| **Field** | **Purpose** |
|-----------|-------------|
| `apiVersion` | Kubernetes API version |
| `kind` | Resource type |
| `metadata` | Service information |
| `selector` | Selects the Pods managed by the Service |
| `ports` | Defines network ports |
| `type` | Specifies the Service type |

---

# 8. Service Selectors

A Service identifies Pods using **Labels**.

## Service Selector

```yaml
selector:
  app: nginx
```

Matching Pod:

```yaml
metadata:
  labels:
    app: nginx
```

If the labels do not match, the Service will not send traffic to those Pods.

---

# 9. Endpoints

A Service does not send traffic directly to Pods.

Instead, Kubernetes automatically creates an **Endpoints** object that stores the IP addresses of all Pods matching the Service selector.

Whenever Pods are:

- Created
- Deleted
- Recreated
- Scaled

Kubernetes automatically updates the Endpoints list.

Example:

```text
Service
   │
   ▼
Endpoints
   │
   ├── 10.244.0.5:80
   ├── 10.244.0.6:80
   └── 10.244.0.7:80
```

View Endpoints:

```bash
kubectl get endpoints
```

Describe Endpoints:

```bash
kubectl describe endpoints nginx-service
```

> 💡 Modern Kubernetes uses **EndpointSlices** internally for better scalability, but `kubectl get endpoints` remains useful for learning and troubleshooting.

---

# 10. Port vs TargetPort vs NodePort

Understanding these ports is essential.

| **Field** | **Description** |
|-----------|-----------------|
| **port** | The port exposed by the Service inside the cluster. |
| **targetPort** | The port on the Pod where the application is listening. |
| **nodePort** | The external port opened on each Worker Node (NodePort Services only). |

Example:

```yaml
ports:
  - port: 80
    targetPort: 8080
    nodePort: 30080
```

Traffic Flow:

```text
Browser
      │
30080
      ▼
NodePort
      │
80
      ▼
Service
      │
8080
      ▼
Pod
```

---

# 11. Creating Your First Service

## Step 1: Create the Service

```bash
kubectl apply -f service.yaml
```

---

## Step 2: Verify

```bash
kubectl get svc
```

---

## Step 3: Describe the Service

```bash
kubectl describe svc nginx-service
```

---

## Step 4: Verify Endpoints

```bash
kubectl get endpoints
```

---

# 12. Useful kubectl Commands

```bash
# Create a Service
kubectl apply -f service.yaml

# List Services
kubectl get svc

# Describe a Service
kubectl describe svc nginx-service

# List Endpoints
kubectl get endpoints

# Export YAML
kubectl get svc nginx-service -o yaml

# Explain Service
kubectl explain service

# Delete Service
kubectl delete svc nginx-service

# List EndpointSlices
kubectl get endpointslices
```

---

# 13. Best Practices

- Use meaningful labels and selectors.
- Use **ClusterIP** for internal communication.
- Use **LoadBalancer** for production workloads in cloud environments.
- Use **NodePort** mainly for learning, testing, or small environments.
- Verify selectors before creating a Service.
- Keep Service names meaningful and consistent.
- Use DNS names instead of Pod IP addresses.

---

# 14. Key Takeaways

- ✅ Services provide a stable network endpoint for Pods.
- ✅ Pods can restart without affecting clients.
- ✅ Services use labels and selectors to route traffic.
- ✅ CoreDNS provides automatic Service discovery.
- ✅ ClusterIP is the default Service type.
- ✅ NodePort exposes applications outside the cluster.
- ✅ LoadBalancer provides cloud-based external access.
- ✅ ExternalName maps a Service to an external DNS name.
- ✅ Endpoints store the current Pod IPs behind a Service.
- ✅ Services continue working even when Pods are recreated.

---

# 📚 What's Next?

➡️ **06. Namespaces**
