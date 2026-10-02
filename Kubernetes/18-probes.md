# Probes

> Probes are health checks that Kubernetes uses to determine whether a container is running correctly, ready to receive traffic, or has finished starting.

---

# 1. What are Probes?

A **Probe** is a health check performed by the **kubelet** to monitor the health of containers running inside a Pod.

Based on the probe result, Kubernetes can:

- Restart unhealthy containers
- Stop sending traffic to unready Pods
- Allow slow-starting applications enough time to initialize

Kubernetes supports three types of probes:

- **Liveness Probe**
- **Readiness Probe**
- **Startup Probe**

> 💡 **Think of probes as regular health checks that help Kubernetes decide whether to restart a container or send traffic to it.**

---

# 2. Why Kubernetes Uses Probes

Applications can experience different problems during their lifecycle.

For example:

- The application crashes or hangs.
- The application is still starting.
- The application is running but not ready to serve requests.

Probes help Kubernetes:

- Detect unhealthy containers
- Restart failed applications
- Prevent traffic from reaching unready Pods
- Improve application availability
- Support zero-downtime deployments

Without probes, Kubernetes assumes that a running container is healthy, even if the application inside it has stopped responding.

---

# 3. Types of Probes

| **Probe** | **Purpose** |
|------------|-------------|
| **Liveness Probe** | Checks whether the container is still running correctly. |
| **Readiness Probe** | Checks whether the container is ready to receive traffic. |
| **Startup Probe** | Checks whether the application has finished starting. |

---

# 4. How Probes Work

```text
                Container
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
   Liveness     Readiness    Startup
      │             │            │
Restart if     Receive       Wait until
 unhealthy      traffic       startup completes
```

Each probe periodically performs a health check and reports the result to the **kubelet**, which decides what action Kubernetes should take.

---

## Probe Lifecycle

The three probes do **not** run at the same time.

```text
Container Starts
        │
        ▼
 Startup Probe
        │
   Success?
        │
        ▼
Liveness Probe ───────► Restart if unhealthy
        │
        ▼
Readiness Probe ──────► Receive traffic if ready
```

> 💡 The **Startup Probe** runs first. After it succeeds, Kubernetes begins running the **Liveness** and **Readiness** probes.

---

# 5. Liveness Probe

A **Liveness Probe** determines whether a container is still healthy.

If the probe fails repeatedly:

```text
Liveness Probe
       │
       ▼
Failure
       │
       ▼
Container Restart
```

### Common Use Cases

- Deadlocks
- Infinite loops
- Hung applications
- Applications that stop responding

Example:

```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 80

  initialDelaySeconds: 10
  periodSeconds: 10
```

> 💡 A failing Liveness Probe causes Kubernetes to restart the container automatically.

---

# 6. Readiness Probe

A **Readiness Probe** determines whether a container is ready to accept client requests.

If the probe fails:

```text
Readiness Probe
       │
       ▼
Failure
       │
       ▼
Removed from
Service Endpoints
```

The container **continues running**, but Kubernetes temporarily stops routing traffic to it.

Example:

```yaml
readinessProbe:
  httpGet:
    path: /ready
    port: 80

  initialDelaySeconds: 5
  periodSeconds: 5
```

> 💡 Readiness Probes prevent users from reaching applications that are still starting or temporarily unavailable.

---

# 7. Startup Probe

Some applications require a long startup time.

A **Startup Probe** gives the application enough time to initialize before Kubernetes starts running Liveness and Readiness checks.

```text
Application Starting
        │
        ▼
 Startup Probe
        │
   Success
        │
        ▼
Liveness + Readiness Start
```

If the Startup Probe fails repeatedly, Kubernetes restarts the container.

Example:

```yaml
startupProbe:
  httpGet:
    path: /startup
    port: 80

  failureThreshold: 30
  periodSeconds: 10
```

> 💡 Startup Probes prevent slow-starting applications from being restarted before they finish initialization.

---

# 8. Probe Types

Kubernetes supports three methods for performing health checks.

| **Probe Type** | **Description** |
|---------------|-----------------|
| **HTTP** | Sends an HTTP request to an application endpoint. |
| **TCP** | Checks whether a TCP port is open. |
| **Exec** | Executes a command inside the container. |

---

## HTTP Probe

Sends an HTTP request to a specific endpoint.

Example:

```yaml
httpGet:
  path: /health
  port: 80
```

Commonly used for:

- REST APIs
- Web applications
- Microservices

---

## TCP Probe

Checks whether a TCP port is accepting connections.

Example:

```yaml
tcpSocket:
  port: 3306
```

Commonly used for:

- MySQL
- PostgreSQL
- Redis
- Kafka

---

## Exec Probe

Runs a command inside the container.

Example:

```yaml
exec:
  command:
    - cat
    - /tmp/healthy
```

If the command exits successfully (exit code `0`), the probe succeeds.

---

# 9. What Happens When a Probe Fails?

Each probe triggers a different Kubernetes action.

| **Probe** | **Failure Result** |
|-----------|--------------------|
| **Startup Probe** | Container is restarted if startup does not complete within the configured threshold. |
| **Liveness Probe** | Container is restarted automatically. |
| **Readiness Probe** | Pod is removed from the Service endpoints but continues running. |

> 💡 Only the **Readiness Probe** does **not** restart the container.

---

# 10. Important Probe Parameters

These parameters control how Kubernetes performs health checks.

| **Field** | **Purpose** |
|-----------|-------------|
| `initialDelaySeconds` | Delay before the first probe runs. |
| `periodSeconds` | Time between probe executions. |
| `timeoutSeconds` | Maximum time to wait for a probe response. |
| `successThreshold` | Consecutive successful probes required before marking healthy. |
| `failureThreshold` | Consecutive failed probes before taking action. |

---

# 11. Anatomy of a Pod with Probes

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

      livenessProbe:
        httpGet:
          path: /health
          port: 80

      readinessProbe:
        httpGet:
          path: /ready
          port: 80

      startupProbe:
        httpGet:
          path: /startup
          port: 80
        failureThreshold: 30
        periodSeconds: 10
```

In production applications, separate endpoints are commonly used:

- `/health` → Liveness
- `/ready` → Readiness
- `/startup` → Startup

---

# 12. Creating Your First Pod with Probes

## Step 1

Create the Pod.

```bash
kubectl apply -f pod.yaml
```

---

## Step 2

Verify the Pod.

```bash
kubectl get pods
```

---

## Step 3

Describe the Pod.

```bash
kubectl describe pod nginx
```

Look under the **Events** section to see probe results.

---

## Step 4

View application logs.

```bash
kubectl logs nginx
```

This helps troubleshoot probe failures.

---

# 13. Useful kubectl Commands

```bash
# Create Pod
kubectl apply -f pod.yaml

# List Pods
kubectl get pods

# Describe Pod
kubectl describe pod nginx

# View Pod logs
kubectl logs nginx

# Execute inside Pod
kubectl exec -it nginx -- /bin/sh

# View Pod YAML
kubectl get pod nginx -o yaml

# Explain Liveness Probe
kubectl explain pod.spec.containers.livenessProbe

# Explain Readiness Probe
kubectl explain pod.spec.containers.readinessProbe

# Explain Startup Probe
kubectl explain pod.spec.containers.startupProbe
```

---

# 14. Best Practices

- Configure Readiness Probes for production applications.
- Use Liveness Probes to recover unhealthy containers.
- Use Startup Probes for slow-starting applications.
- Keep health check endpoints lightweight.
- Avoid aggressive probe intervals.
- Test probe settings before deploying to production.
- Use separate endpoints for health, readiness, and startup checks whenever possible.

---

# 15. Liveness vs Readiness vs Startup

| **Liveness** | **Readiness** | **Startup** |
|--------------|---------------|-------------|
| Detects unhealthy containers | Determines if the application can receive traffic | Checks whether the application has finished starting |
| Restarts the container | Removes the Pod from Service endpoints | Delays Liveness and Readiness until startup completes |
| Improves self-healing | Prevents failed requests | Prevents unnecessary restarts |

---

# 16. Key Takeaways

- ✅ Kubernetes supports Liveness, Readiness, and Startup Probes.
- ✅ Liveness Probes restart unhealthy containers.
- ✅ Readiness Probes control when Pods receive traffic.
- ✅ Startup Probes protect slow-starting applications.
- ✅ HTTP, TCP, and Exec are the three probe types.
- ✅ Proper probe configuration improves application reliability and availability.
