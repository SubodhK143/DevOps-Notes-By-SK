# Resource Requests & Limits

> Resource Requests and Limits define how much CPU and memory a container needs and the maximum amount it is allowed to consume.

---

# 1. What are Resource Requests & Limits?

Kubernetes allows you to specify how much **CPU** and **memory** a container requires.

There are two important settings:

- **Request** – The minimum amount of resources Kubernetes guarantees to a container.
- **Limit** – The maximum amount of resources a container is allowed to use.

> 💡 **Think of a Request as a reservation and a Limit as a spending cap.**

---

# 2. Why Kubernetes Uses Requests & Limits

Without resource management:

- One container could consume all CPU.
- One application could run out of memory.
- Other Pods might become slow or crash.
- Cluster resources would be used inefficiently.

Requests and Limits provide:

- Fair resource allocation
- Better scheduling decisions
- Protection against resource starvation
- Improved cluster stability
- Predictable application performance

---

# 3. How Requests & Limits Work

When a Pod is created:

1. The **Scheduler** uses **Requests** to decide which node can run the Pod.
2. Once running, the container can use resources up to its **Limit**.

```text
Container

CPU Request: 250m
CPU Limit:   500m

Memory Request: 256Mi
Memory Limit:   512Mi
```

---

# 4. Requests vs Limits

| **Request** | **Limit** |
|-------------|-----------|
| Minimum guaranteed resources | Maximum allowed resources |
| Used during scheduling | Enforced while the container is running |
| Determines Pod placement | Prevents excessive resource usage |

---

# 5. CPU Units

CPU resources are measured in **cores** or **millicores (m)**.

| **Value** | **Meaning** |
|-----------|-------------|
| `1000m` | 1 CPU core |
| `500m` | 0.5 CPU |
| `250m` | 0.25 CPU |
| `100m` | 0.1 CPU |

Example:

```yaml
resources:
  requests:
    cpu: "250m"

  limits:
    cpu: "500m"
```

---

# 6. Memory Units

Memory resources are measured in bytes.

Common units:

| **Value** | **Meaning** |
|-----------|-------------|
| `128Mi` | 128 Mebibytes |
| `512Mi` | 512 Mebibytes |
| `1Gi` | 1024 MiB |
| `2Gi` | 2048 MiB |

Example:

```yaml
resources:
  requests:
    memory: "256Mi"

  limits:
    memory: "512Mi"
```

---

# 7. Anatomy of Resource Configuration

Resources are configured inside the container specification.

Example:

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: nginx

spec:
  containers:
    - name: nginx
      image: nginx

      resources:
        requests:
          cpu: "250m"
          memory: "256Mi"

        limits:
          cpu: "500m"
          memory: "512Mi"
```

---

## Manifest Fields

| **Field** | **Purpose** |
|-----------|-------------|
| `requests.cpu` | Guaranteed CPU |
| `requests.memory` | Guaranteed memory |
| `limits.cpu` | Maximum CPU |
| `limits.memory` | Maximum memory |

---

# 8. What Happens When Limits Are Exceeded?

## CPU Limit

If a container uses more CPU than its limit:

- Kubernetes throttles the CPU usage.
- The container continues running.

```text
CPU Usage
      │
      ▼
Exceeds Limit
      │
      ▼
CPU Throttling
```

---

## Memory Limit

If a container exceeds its memory limit:

- Kubernetes terminates the container.
- The Pod may restart depending on its restart policy.

```text
Memory Usage
      │
      ▼
Exceeds Limit
      │
      ▼
OOMKilled
```

> 💡 **OOMKilled** stands for **Out Of Memory Killed**.

---

# 9. Quality of Service (QoS) Classes

Kubernetes assigns every Pod a **Quality of Service (QoS)** class.

| **QoS Class** | **Condition** |
|---------------|---------------|
| **Guaranteed** | Requests = Limits for CPU and memory |
| **Burstable** | Requests are set, but Limits are higher |
| **BestEffort** | No Requests or Limits specified |

Priority during resource pressure:

```text
Guaranteed
      │
      ▼
Burstable
      │
      ▼
BestEffort
```

> 💡 BestEffort Pods are the first to be evicted when a node runs out of resources.

---

# 10. Creating a Pod with Resource Limits

## Step 1

Create the Pod.

```bash
kubectl apply -f pod.yaml
```

---

## Step 2

Verify.

```bash
kubectl get pods
```

---

## Step 3

Describe the Pod.

```bash
kubectl describe pod nginx
```

Look for the **Requests** and **Limits** section.

---

# 11. Useful kubectl Commands

```bash
# Create a Pod
kubectl apply -f pod.yaml

# List Pods
kubectl get pods

# Describe Pod resources
kubectl describe pod nginx

# View Pod YAML
kubectl get pod nginx -o yaml

# View resource usage (Metrics Server required)
kubectl top pods

# View node resource usage
kubectl top nodes

# Explain resources field
kubectl explain pod.spec.containers.resources
```

> 💡 The `kubectl top` command requires the **Metrics Server** to be installed in the cluster.

---

# 12. Best Practices

- Always define CPU and memory Requests.
- Set Limits to prevent excessive resource usage.
- Avoid using BestEffort Pods in production.
- Monitor resource usage regularly.
- Adjust Requests and Limits based on application behavior.
- Use realistic values instead of very high limits.
- Test resource settings before deploying to production.

---

# 13. Key Takeaways

- ✅ Requests reserve CPU and memory for a container.
- ✅ Limits define the maximum resources a container can use.
- ✅ The Scheduler uses Requests to place Pods.
- ✅ CPU overuse results in throttling.
- ✅ Memory overuse can cause `OOMKilled`.
- ✅ QoS classes determine Pod priority during resource pressure.
- ✅ Proper Requests and Limits improve cluster stability and efficiency.

---

# 📚 What's Next?

➡️ **18. Probes**
