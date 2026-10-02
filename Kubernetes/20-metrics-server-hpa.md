# Metrics Server & Horizontal Pod Autoscaler (HPA)

> The Metrics Server collects real-time CPU and memory usage from Kubernetes nodes and Pods, while the Horizontal Pod Autoscaler (HPA) automatically scales applications based on resource utilization.

---

# 1. What is the Metrics Server?

The **Metrics Server** is a lightweight Kubernetes component that collects **CPU** and **memory usage** from the **kubelet** running on every Worker Node.

It provides resource metrics for:

- `kubectl top`
- Horizontal Pod Autoscaler (HPA)

> 💡 **Think of the Metrics Server as Kubernetes' resource monitor.**

---

# 2. Why Kubernetes Uses the Metrics Server

Kubernetes needs real-time resource usage to make scaling decisions.

Without the Metrics Server:

- `kubectl top` does not work.
- HPA cannot scale applications automatically.
- CPU and memory usage cannot be viewed easily.

The Metrics Server provides:

- Live CPU usage
- Live memory usage
- Metrics for autoscaling
- Lightweight resource monitoring

> 💡 The Metrics Server is **not** a full monitoring solution like Prometheus. It only provides resource metrics required by Kubernetes.

---

# 3. How the Metrics Server Works

The Metrics Server periodically collects resource usage from every node through the kubelet.

```text
        Worker Node
             │
             ▼
         Kubelet
             │
             ▼
     Metrics Server
       ┌─────┴─────┐
       ▼           ▼
 kubectl top      HPA
```

The Metrics Server polls kubelets approximately every **15 seconds**.

> 💡 `kubectl top` displays **actual CPU and memory usage**, not configured Requests or Limits.

---

# 4. Viewing Resource Usage

After installing the Metrics Server, Kubernetes can display live resource usage.

## View Node Usage

```bash
kubectl top nodes
```

---

## View Pod Usage

```bash
kubectl top pods
```

---

## View All Pods

```bash
kubectl top pods -A
```

---

## Sort Pods by CPU Usage

```bash
kubectl top pods -A --sort-by=cpu
```

---

## Sort Pods by Memory Usage

```bash
kubectl top pods -A --sort-by=memory
```

Example output:

```text
NAME           CPU(cores)   MEMORY(bytes)

nginx-abc      25m          45Mi

redis-def      80m          120Mi

api-xyz        150m         210Mi
```

---

# 5. Installing the Metrics Server

Install the official Metrics Server.

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

Verify the installation.

```bash
kubectl get pods -n kube-system
```

Look for:

```text
metrics-server
```

---

## Local Cluster Note

On local clusters such as:

- Kind
- Minikube
- Docker Desktop

you may need to allow insecure kubelet TLS.

```bash
kubectl patch deployment metrics-server -n kube-system \
  --type='json' \
  -p='[{"op":"add","path":"/spec/template/spec/containers/0/args/-","value":"--kubelet-insecure-tls"}]'
```

Wait for the Metrics Server to become ready.

```bash
kubectl rollout status deployment metrics-server -n kube-system --timeout=120s
```

> 💡 Use `--kubelet-insecure-tls` only for local development. Never use it in production clusters.

---

# 6. What is Horizontal Pod Autoscaler (HPA)?

The **Horizontal Pod Autoscaler (HPA)** automatically increases or decreases the number of Pod replicas based on observed resource utilization.

HPA can scale applications using:

- CPU utilization
- Memory utilization *(autoscaling/v2)*
- Custom metrics
- External metrics

> 💡 HPA performs **horizontal scaling** by changing the number of Pod replicas.

---

# 7. Why Kubernetes Uses HPA

Application traffic changes continuously.

For example:

```text
Morning
   │
2 Pods

Afternoon
   │
10 Pods

Night
   │
2 Pods
```

Instead of manually changing the replica count, Kubernetes automatically adjusts it based on resource usage.

Benefits:

- Automatic scaling
- Better resource utilization
- Lower infrastructure cost
- Improved application availability
- Handles changing workloads automatically

---

# 8. How HPA Works

The Horizontal Pod Autoscaler continuously monitors resource usage and adjusts the number of Pod replicas.

```text
             Users
                │
                ▼
          Deployment
                │
          Current Pods
                │
        CPU / Memory Usage
                │
                ▼
         Metrics Server
                │
                ▼
Horizontal Pod Autoscaler
                │
     Increase / Decrease
          Pod Replicas
```

HPA continuously compares the current resource usage with the configured target utilization and adjusts the number of replicas automatically.

---

# 9. HPA Scaling Flow

When application load increases:

```text
High CPU Usage
       │
       ▼
Metrics Server
       │
       ▼
Horizontal Pod Autoscaler
       │
       ▼
Deployment
       │
       ▼
More Pods Created
```

When resource usage decreases:

```text
Low CPU Usage
       │
       ▼
Metrics Server
       │
       ▼
Horizontal Pod Autoscaler
       │
       ▼
Deployment
       │
       ▼
Extra Pods Removed
```

> 💡 HPA scales Pods automatically to match application demand while maintaining the desired resource utilization.

---

# 10. HPA Scaling Formula

The Horizontal Pod Autoscaler calculates the desired number of replicas using the following formula.

```text
desiredReplicas =
ceil(currentReplicas × (currentUsage / targetUsage))
```

Example:

```text
Current Replicas = 4

Current CPU Usage = 80%

Target CPU Usage = 40%

desiredReplicas

= ceil(4 × (80 / 40))

= ceil(8)

= 8 Pods
```

The HPA continuously recalculates this value and adjusts the number of Pods automatically.

---

# 11. HPA Prerequisites

Before creating an HPA, ensure the following requirements are met.

- Metrics Server must be installed and running.
- Pods must define **CPU Requests**.
- A Deployment, ReplicaSet, or StatefulSet must already exist.

Example:

```yaml
resources:
  requests:
    cpu: "250m"
    memory: "256Mi"
```

> 💡 HPA uses **CPU Requests**, not CPU Limits, to calculate CPU utilization percentages.

Without CPU Requests, HPA cannot calculate utilization and will not scale correctly.

---

# 12. autoscaling/v1 vs autoscaling/v2

Kubernetes provides two API versions for the Horizontal Pod Autoscaler.

| **Feature** | **autoscaling/v1** | **autoscaling/v2** |
|--------------|--------------------|--------------------|
| CPU Scaling | ✅ | ✅ |
| Memory Scaling | ❌ | ✅ |
| Custom Metrics | ❌ | ✅ |
| External Metrics | ❌ | ✅ |
| Behavior Configuration | ❌ | ✅ |

> 💡 Use **autoscaling/v2** for production workloads because it supports multiple metrics and advanced scaling behavior.

---

# 13. HPA Behavior

The **behavior** section controls how aggressively HPA scales applications.

Example:

```yaml
behavior:
  scaleDown:
    stabilizationWindowSeconds: 300
```

This waits **5 minutes** before reducing the number of Pods.

---

## Common Behavior Settings

| **Field** | **Purpose** |
|-----------|-------------|
| `stabilizationWindowSeconds` | Wait before scaling up or down |
| `policies` | Limit how many Pods can be added or removed |
| `type: Percent` | Scale by a percentage |
| `type: Pods` | Scale by a fixed number of Pods |
| `periodSeconds` | Minimum time between scaling actions |

Typical production behavior:

- Scale up quickly during heavy traffic.
- Scale down gradually to avoid frequent fluctuations.

> 💡 Delaying scale-down prevents rapid scaling up and down, a situation known as **flapping**.

---

# 14. Creating Your First HPA

## Step 1

Deploy the application.

```bash
kubectl apply -f deployment.yaml
```

---

## Step 2

Create the Horizontal Pod Autoscaler.

```bash
kubectl autoscale deployment nginx \
  --cpu-percent=50 \
  --min=2 \
  --max=10
```

---

## Step 3

Verify the HPA.

```bash
kubectl get hpa
```

---

## Step 4

View detailed information.

```bash
kubectl describe hpa
```

This displays:

- Current CPU utilization
- Target utilization
- Current replicas
- Desired replicas
- Scaling events

---

# 15. Useful kubectl Commands

```bash
# View node resource usage
kubectl top nodes

# View Pod resource usage
kubectl top pods

# View all Pods
kubectl top pods -A

# Sort Pods by CPU usage
kubectl top pods -A --sort-by=cpu

# Sort Pods by Memory usage
kubectl top pods -A --sort-by=memory

# Create an HPA
kubectl autoscale deployment nginx \
  --cpu-percent=50 \
  --min=2 \
  --max=10

# List HPAs
kubectl get hpa

# Describe an HPA
kubectl describe hpa

# Delete an HPA
kubectl delete hpa nginx

# View HPA YAML
kubectl get hpa nginx -o yaml

# Explain HPA
kubectl explain hpa
```

---

# 16. Metrics Server vs Prometheus

| **Metrics Server** | **Prometheus** |
|--------------------|----------------|
| Lightweight component | Full monitoring platform |
| CPU & memory metrics | Collects many types of metrics |
| Supports HPA | Advanced monitoring and alerting |
| Short-term metrics | Long-term metric storage |
| Kubernetes resource metrics | Infrastructure and application metrics |

> 💡 Metrics Server is designed for autoscaling, while Prometheus is designed for monitoring and alerting.

---

# 17. Best Practices

- Install the Metrics Server before creating an HPA.
- Always define CPU Requests for workloads using HPA.
- Use **autoscaling/v2** for production workloads.
- Configure sensible minimum and maximum replica counts.
- Monitor scaling behavior regularly.
- Avoid overly aggressive scaling policies.
- Use Prometheus for detailed monitoring and alerting.
- Test autoscaling behavior before deploying to production.

---

# 18. Key Takeaways

- ✅ Metrics Server collects real-time CPU and memory usage.
- ✅ `kubectl top` depends on the Metrics Server.
- ✅ HPA automatically scales Pods based on resource utilization.
- ✅ CPU Requests are required for CPU-based autoscaling.
- ✅ `autoscaling/v2` supports memory, custom metrics, external metrics, and behavior tuning.
- ✅ HPA improves scalability, availability, and resource efficiency.
