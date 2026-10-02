# Kubernetes (K8s) Cheat Sheet

> Quick Reference for `kubectl`, Kubernetes Resources, Configs, Networking, Storage & Troubleshooting

---

## 1. Cluster & Context

| Command                                                 | Description                                                           |
| ------------------------------------------------------- | --------------------------------------------------------------------- |
| `kubectl version`                                       | Show client and server versions                                       |
| `kubectl cluster-info`                                  | Show cluster endpoint information                                     |
| `kubectl config get-contexts`                           | List all contexts                                                     |
| `kubectl config current-context`                        | Show active context                                                   |
| `kubectl config use-context <context>`                  | Switch context                                                        |
| `kubectl config set-context --current --namespace=<ns>` | Set default namespace                                                 |
| `kubectl get nodes`                                     | List all nodes                                                        |
| `kubectl get nodes -o wide`                             | Nodes with IP, OS and roles                                           |
| `kubectl describe node <node>`                          | Show node details and capacity                                        |
| `kubectl top nodes`                                     | Show node CPU/memory usage                                            |
| `kubectl get componentstatuses`                         | Check control-plane component status *(deprecated in newer clusters)* |

---

## 2. Namespaces

| Command                         | Description                             |
| ------------------------------- | --------------------------------------- |
| `kubectl get namespaces`        | List namespaces                         |
| `kubectl get ns`                | List namespaces using short name        |
| `kubectl create namespace <ns>` | Create namespace                        |
| `kubectl delete namespace <ns>` | Delete namespace                        |
| `kubectl get all -n <ns>`       | List common resources in namespace      |
| `kubectl get all -A`            | List common resources across namespaces |
| `-n <ns>`                       | Target a specific namespace             |
| `--all-namespaces` / `-A`       | Work across all namespaces              |

---

## 3. Pods

| Command                                             | Description                          |
| --------------------------------------------------- | ------------------------------------ |
| `kubectl get pods`                                  | List pods in current namespace       |
| `kubectl get pods -A`                               | List pods in all namespaces          |
| `kubectl get pods -o wide`                          | Pods with node and IP information    |
| `kubectl get pods -w`                               | Watch pod changes live               |
| `kubectl get pod <pod> -o yaml`                     | Show pod manifest                    |
| `kubectl describe pod <pod>`                        | Show pod details and events          |
| `kubectl logs <pod>`                                | Show container logs                  |
| `kubectl logs <pod> -c <container>`                 | Logs from specific container         |
| `kubectl logs <pod> --previous`                     | Logs from previous crashed container |
| `kubectl logs -f <pod>`                             | Follow/stream logs                   |
| `kubectl logs <pod> --all-containers`               | Logs from all containers             |
| `kubectl exec -it <pod> -- /bin/sh`                 | Open shell inside container          |
| `kubectl exec -it <pod> -c <container> -- /bin/sh`  | Shell into specific container        |
| `kubectl delete pod <pod>`                          | Delete pod                           |
| `kubectl delete pod <pod> --force --grace-period=0` | Force-delete stuck pod               |
| `kubectl run <pod> --image=<image>`                 | Create a quick pod                   |
| `kubectl run -it --rm debug --image=busybox -- sh`  | Temporary debug pod                  |
| `kubectl top pods`                                  | Show pod CPU/memory usage            |
| `kubectl top pod <pod> --containers`                | Show container-level usage           |

> **Note:** Pods managed by a Deployment, ReplicaSet, StatefulSet, etc. are usually recreated automatically after deletion.

---

## 4. Deployments

| Command                                                      | Description                       |
| ------------------------------------------------------------ | --------------------------------- |
| `kubectl get deployments`                                    | List deployments                  |
| `kubectl get deploy`                                         | List deployments using short name |
| `kubectl describe deployment <name>`                         | Show deployment details           |
| `kubectl create deployment <name> --image=<image>`           | Create deployment                 |
| `kubectl scale deployment <name> --replicas=3`               | Scale deployment                  |
| `kubectl set image deployment/<name> <container>=<image>:v2` | Update container image            |
| `kubectl rollout status deployment/<name>`                   | Watch rollout progress            |
| `kubectl rollout history deployment/<name>`                  | Show rollout history              |
| `kubectl rollout undo deployment/<name>`                     | Roll back deployment              |
| `kubectl rollout undo deployment/<name> --to-revision=2`     | Roll back to specific revision    |
| `kubectl rollout pause deployment/<name>`                    | Pause rollout                     |
| `kubectl rollout resume deployment/<name>`                   | Resume rollout                    |
| `kubectl rollout restart deployment/<name>`                  | Restart pods through rollout      |
| `kubectl edit deployment <name>`                             | Edit deployment live              |
| `kubectl delete deployment <name>`                           | Delete deployment                 |
| `kubectl get replicasets`                                    | List ReplicaSets                  |

---

## 5. Services & Networking

| Command                                                          | Description                       |
| ---------------------------------------------------------------- | --------------------------------- |
| `kubectl get services`                                           | List services                     |
| `kubectl get svc`                                                | List services using short name    |
| `kubectl get svc -A`                                             | Services across all namespaces    |
| `kubectl describe svc <name>`                                    | Show service details              |
| `kubectl expose deployment <name> --port=80 --type=ClusterIP`    | Expose deployment as ClusterIP    |
| `kubectl expose deployment <name> --port=80 --type=NodePort`     | Expose deployment as NodePort     |
| `kubectl expose deployment <name> --port=80 --type=LoadBalancer` | Expose deployment as LoadBalancer |
| `kubectl get endpoints <service>`                                | Show service endpoints            |
| `kubectl get endpointslices`                                     | Show EndpointSlices               |
| `kubectl port-forward pod/<pod> 8080:80`                         | Forward local port to pod         |
| `kubectl port-forward svc/<service> 8080:80`                     | Forward local port to service     |
| `kubectl get networkpolicies`                                    | List NetworkPolicies              |
| `kubectl describe networkpolicy <name>`                          | Show NetworkPolicy details        |

### Service Types

| Type           | Purpose                          |
| -------------- | -------------------------------- |
| `ClusterIP`    | Internal cluster access          |
| `NodePort`     | Expose service through node port |
| `LoadBalancer` | External load balancer           |
| `ExternalName` | Map service to external DNS name |

---

## 6. ConfigMaps & Secrets

### ConfigMaps

| Command                                                    | Description            |
| ---------------------------------------------------------- | ---------------------- |
| `kubectl get configmaps`                                   | List ConfigMaps        |
| `kubectl describe configmap <name>`                        | Show ConfigMap details |
| `kubectl get configmap <name> -o yaml`                     | Show ConfigMap YAML    |
| `kubectl create configmap <name> --from-literal=key=value` | Create from literal    |
| `kubectl create configmap <name> --from-file=<file>`       | Create from file       |
| `kubectl delete configmap <name>`                          | Delete ConfigMap       |

### Secrets

| Command                                                              | Description                        |
| -------------------------------------------------------------------- | ---------------------------------- |
| `kubectl get secrets`                                                | List Secrets                       |
| `kubectl describe secret <name>`                                     | Show Secret metadata               |
| `kubectl get secret <name> -o yaml`                                  | Show Secret YAML with encoded data |
| `kubectl get secret <name> -o jsonpath='{.data.<key>}'`              | Get base64-encoded value           |
| `kubectl get secret <name> -o jsonpath='{.data.<key>}' \| base64 -d` | Decode secret value                |
| `kubectl create secret generic <name> --from-literal=key=value`      | Create generic Secret              |
| `kubectl create secret generic <name> --from-file=<file>`            | Create Secret from file            |
| `kubectl delete secret <name>`                                       | Delete Secret                      |

> **Important:** Kubernetes Secrets are **base64-encoded, not encrypted by default**. Protect access using RBAC and configure encryption at rest where appropriate.

---

## 7. Storage

| Command                                | Description                          |
| -------------------------------------- | ------------------------------------ |
| `kubectl get pv`                       | List PersistentVolumes               |
| `kubectl get pvc`                      | List PersistentVolumeClaims          |
| `kubectl get pvc -A`                   | List PVCs across namespaces          |
| `kubectl describe pv <name>`           | Show PV details                      |
| `kubectl describe pvc <name>`          | Show PVC details and binding         |
| `kubectl get storageclass`             | List StorageClasses                  |
| `kubectl get sc`                       | List StorageClasses using short name |
| `kubectl describe storageclass <name>` | Show StorageClass details            |
| `kubectl delete pvc <name>`            | Delete PVC                           |
| `kubectl delete pv <name>`             | Delete PV                            |

### Storage Flow

```text
Pod
 │
 ▼
PVC
 │
 ▼
PV
 │
 ▼
StorageClass / Provisioner
 │
 ▼
Actual Storage
```

---

## 8. Apply & Manage Manifests

| Command                               | Description                          |
| ------------------------------------- | ------------------------------------ |
| `kubectl apply -f <file.yaml>`        | Create/update resources              |
| `kubectl apply -f <directory>/`       | Apply YAML files in directory        |
| `kubectl delete -f <file.yaml>`       | Delete resources defined in manifest |
| `kubectl create -f <file.yaml>`       | Create resources only                |
| `kubectl replace -f <file.yaml>`      | Replace existing resource            |
| `kubectl diff -f <file.yaml>`         | Preview changes                      |
| `kubectl get -f <file.yaml>`          | Get resource defined in manifest     |
| `kubectl explain <resource>`          | Explain Kubernetes resource          |
| `kubectl explain pod.spec.containers` | Explore specific API fields          |

---

## 9. Labels & Selectors

| Command                                       | Description                  |
| --------------------------------------------- | ---------------------------- |
| `kubectl get pods -l app=myapp`               | Filter pods by label         |
| `kubectl get all -l app=myapp`                | Get resources matching label |
| `kubectl label pod <pod> env=prod`            | Add label                    |
| `kubectl label pod <pod> env=dev --overwrite` | Update existing label        |
| `kubectl get pods --show-labels`              | Show pod labels              |
| `kubectl get pods -l 'env in (prod,staging)'` | Match multiple values        |
| `kubectl get pods -l 'env notin (dev,test)'`  | Exclude values               |

---

## 10. Output & Formatting

| Command / Flag                          | Description             |
| --------------------------------------- | ----------------------- |
| `-o wide`                               | Show additional columns |
| `-o yaml`                               | Output complete YAML    |
| `-o json`                               | Output JSON             |
| `-o name`                               | Output resource names   |
| `-o jsonpath='{...}'`                   | Extract specific field  |
| `--show-labels`                         | Display labels          |
| `--sort-by=.metadata.name`              | Sort results            |
| `--field-selector=status.phase=Running` | Filter by field         |
| `--watch` / `-w`                        | Watch resource changes  |

### Useful Examples

```bash
kubectl get pods -o wide
kubectl get pods -o yaml
kubectl get pods -o name
kubectl get pods --show-labels
kubectl get pods --sort-by=.metadata.name
```

---

## 11. Troubleshooting

### Pod Issues

| Command                                       | Purpose                      |
| --------------------------------------------- | ---------------------------- |
| `kubectl get pods`                            | Check pod status             |
| `kubectl describe pod <pod>`                  | Check events and conditions  |
| `kubectl logs <pod>`                          | Check application logs       |
| `kubectl logs <pod> --previous`               | Check crashed container logs |
| `kubectl get events --sort-by=.lastTimestamp` | View recent events           |
| `kubectl get events -n <ns>`                  | Events in namespace          |
| `kubectl exec -it <pod> -- /bin/sh`           | Enter container              |
| `kubectl top pod <pod>`                       | Check resource usage         |

### Node Issues

```bash
kubectl get nodes
kubectl get nodes -o wide
kubectl describe node <node>
kubectl top nodes
kubectl get events --sort-by=.lastTimestamp
```

### Service/DNS Testing

```bash
kubectl get svc
kubectl describe svc <service>
kubectl get endpoints <service>
kubectl get endpointslices
```

Inside a debugging pod:

```bash
kubectl run -it --rm debug --image=busybox -- sh
```

Then:

```bash
nslookup <service>
wget -qO- http://<service>:<port>
```

If `curl` is available:

```bash
curl http://<service>:<port>
```

---

## 12. Resource Requests & Limits

```bash
kubectl describe pod <pod>
kubectl top pod <pod>
kubectl top nodes
```

### Check Resource Configuration

```bash
kubectl get pod <pod> -o jsonpath='{.spec.containers[*].resources}'
```

### Common Concepts

| Term              | Meaning                                 |
| ----------------- | --------------------------------------- |
| `requests.cpu`    | CPU guaranteed/requested for scheduling |
| `requests.memory` | Memory requested for scheduling         |
| `limits.cpu`      | Maximum CPU allowed                     |
| `limits.memory`   | Maximum memory allowed                  |

---

## 13. ReplicaSets

| Command                      | Description              |
| ---------------------------- | ------------------------ |
| `kubectl get replicasets`    | List ReplicaSets         |
| `kubectl get rs`             | Short name               |
| `kubectl describe rs <name>` | Show ReplicaSet details  |
| `kubectl get rs -o wide`     | Detailed ReplicaSet list |
| `kubectl delete rs <name>`   | Delete ReplicaSet        |

---

## 14. StatefulSets

| Command                                         | Description              |
| ----------------------------------------------- | ------------------------ |
| `kubectl get statefulsets`                      | List StatefulSets        |
| `kubectl get sts`                               | Short name               |
| `kubectl describe statefulset <name>`           | Show StatefulSet details |
| `kubectl scale statefulset <name> --replicas=3` | Scale StatefulSet        |
| `kubectl rollout status statefulset/<name>`     | Check rollout            |
| `kubectl delete statefulset <name>`             | Delete StatefulSet       |

---

## 15. DaemonSets

| Command                                    | Description            |
| ------------------------------------------ | ---------------------- |
| `kubectl get daemonsets`                   | List DaemonSets        |
| `kubectl get ds`                           | Short name             |
| `kubectl describe daemonset <name>`        | Show DaemonSet details |
| `kubectl rollout status daemonset/<name>`  | Check rollout          |
| `kubectl rollout restart daemonset/<name>` | Restart DaemonSet      |
| `kubectl delete daemonset <name>`          | Delete DaemonSet       |

---

## 16. Jobs & CronJobs

### Jobs

| Command                       | Description      |
| ----------------------------- | ---------------- |
| `kubectl get jobs`            | List Jobs        |
| `kubectl describe job <name>` | Show Job details |
| `kubectl logs job/<name>`     | View Job logs    |
| `kubectl delete job <name>`   | Delete Job       |

### CronJobs

| Command                                          | Description              |
| ------------------------------------------------ | ------------------------ |
| `kubectl get cronjobs`                           | List CronJobs            |
| `kubectl get cj`                                 | Short name               |
| `kubectl describe cronjob <name>`                | Show CronJob details     |
| `kubectl create job --from=cronjob/<name> <job>` | Manually trigger CronJob |
| `kubectl delete cronjob <name>`                  | Delete CronJob           |

---

## 17. Ingress

| Command                           | Description                      |
| --------------------------------- | -------------------------------- |
| `kubectl get ingress`             | List Ingress resources           |
| `kubectl get ing`                 | Short name                       |
| `kubectl describe ingress <name>` | Show Ingress details             |
| `kubectl get ingress -A`          | List Ingresses across namespaces |

```bash
kubectl get ingress
kubectl describe ingress <name>
```

> **Note:** An Ingress resource requires an Ingress controller to actually process traffic.

---

## 18. RBAC

### ServiceAccounts

| Command                                | Description                 |
| -------------------------------------- | --------------------------- |
| `kubectl get serviceaccounts`          | List ServiceAccounts        |
| `kubectl get sa`                       | Short name                  |
| `kubectl describe sa <name>`           | Show ServiceAccount details |
| `kubectl create serviceaccount <name>` | Create ServiceAccount       |

### Permissions

```bash
kubectl auth can-i --list
kubectl auth can-i get pods
kubectl auth can-i create deployments
kubectl auth can-i delete pods -n <namespace>
```

### RBAC Resources

```bash
kubectl get roles
kubectl get rolebindings
kubectl get clusterroles
kubectl get clusterrolebindings
```

---

## 19. Resource Discovery

| Command                               | Description                  |
| ------------------------------------- | ---------------------------- |
| `kubectl api-resources`               | List supported API resources |
| `kubectl api-versions`                | List API versions            |
| `kubectl explain pod`                 | Explain Pod                  |
| `kubectl explain deployment`          | Explain Deployment           |
| `kubectl explain service`             | Explain Service              |
| `kubectl explain pod.spec`            | Explain Pod spec             |
| `kubectl explain pod.spec.containers` | Explain container fields     |

---

## 20. Editing Resources

```bash
kubectl edit deployment <name>
kubectl edit service <name>
kubectl edit configmap <name>
kubectl edit secret <name>
kubectl edit pod <name>
```

> For production workloads, prefer updating the version-controlled manifest and applying it rather than making undocumented live edits.

---

## 21. Delete Resources

```bash
kubectl delete pod <pod>
kubectl delete deployment <deployment>
kubectl delete service <service>
kubectl delete configmap <configmap>
kubectl delete secret <secret>
kubectl delete pvc <pvc>
kubectl delete namespace <namespace>
```

Delete using labels:

```bash
kubectl delete pods -l app=myapp
```

Delete using manifest:

```bash
kubectl delete -f deployment.yaml
```

---

## 22. Rollouts & Rollbacks

```bash
# Check rollout
kubectl rollout status deployment/<name>

# View history
kubectl rollout history deployment/<name>

# Roll back
kubectl rollout undo deployment/<name>

# Roll back to revision
kubectl rollout undo deployment/<name> --to-revision=2

# Pause rollout
kubectl rollout pause deployment/<name>

# Resume rollout
kubectl rollout resume deployment/<name>

# Restart deployment
kubectl rollout restart deployment/<name>
```

### Rolling Update Flow

```text
Old ReplicaSet
      │
      ▼
New ReplicaSet Created
      │
      ▼
New Pods Start
      │
      ▼
Old Pods Scale Down
      │
      ▼
New Version Running
```

---

## 23. Kubernetes Events

```bash
kubectl get events
kubectl get events -A
kubectl get events --sort-by=.lastTimestamp
kubectl get events -n <namespace> --sort-by=.lastTimestamp
```

Events are especially useful for diagnosing:

* `Pending`
* `FailedScheduling`
* `ImagePullBackOff`
* `ErrImagePull`
* `CrashLoopBackOff`
* `FailedMount`
* `Unhealthy`
* Probe failures

---

## 24. Common Pod Statuses

| Status              | Meaning                            |
| ------------------- | ---------------------------------- |
| `Pending`           | Pod has not been scheduled/started |
| `Running`           | Pod is running                     |
| `Succeeded`         | Pod completed successfully         |
| `Failed`            | Pod terminated unsuccessfully      |
| `CrashLoopBackOff`  | Container repeatedly crashes       |
| `ImagePullBackOff`  | Kubernetes cannot pull image       |
| `ErrImagePull`      | Image pull failed                  |
| `ContainerCreating` | Container is being created         |
| `Terminating`       | Pod is being deleted               |

### First Troubleshooting Commands

```bash
kubectl get pods
kubectl describe pod <pod>
kubectl logs <pod>
kubectl logs <pod> --previous
kubectl get events --sort-by=.lastTimestamp
```

---

## 25. Resource Short Names

| Short Name | Full Resource          |
| ---------- | ---------------------- |
| `po`       | pods                   |
| `deploy`   | deployments            |
| `svc`      | services               |
| `rs`       | replicasets            |
| `ds`       | daemonsets             |
| `sts`      | statefulsets           |
| `ns`       | namespaces             |
| `no`       | nodes                  |
| `cm`       | configmaps             |
| `sa`       | serviceaccounts        |
| `ing`      | ingresses              |
| `pv`       | persistentvolumes      |
| `pvc`      | persistentvolumeclaims |
| `sc`       | storageclasses         |
| `ep`       | endpoints              |
| `ev`       | events                 |
| `cj`       | cronjobs               |

---

## 26. Useful `kubectl get` Commands

```bash
# Pods
kubectl get pods

# All common resources
kubectl get all

# Everything in all namespaces
kubectl get all -A

# Pods with node/IP information
kubectl get pods -o wide

# YAML
kubectl get pod <pod> -o yaml

# JSON
kubectl get pod <pod> -o json

# Specific resource
kubectl get deployment <name>

# Multiple resources
kubectl get pods,svc,deploy

# Watch resources
kubectl get pods -w
```

---

## 27. JSONPath

Extract useful fields from Kubernetes objects:

```bash
# Pod IP
kubectl get pod <pod> -o jsonpath='{.status.podIP}'

# Pod node
kubectl get pod <pod> -o jsonpath='{.spec.nodeName}'

# Pod phase
kubectl get pod <pod> -o jsonpath='{.status.phase}'

# Container image
kubectl get pod <pod> -o jsonpath='{.spec.containers[*].image}'

# Secret value
kubectl get secret <secret> \
  -o jsonpath='{.data.password}' | base64 -d
```

---

## 28. Debugging Checklist

### Pod is Pending

```bash
kubectl get pod <pod>
kubectl describe pod <pod>
kubectl get events --sort-by=.lastTimestamp
kubectl get nodes
```

Check:

* Insufficient CPU/memory
* Node selectors
* Taints/tolerations
* PVC binding
* Scheduling constraints

### Pod is CrashLoopBackOff

```bash
kubectl logs <pod>
kubectl logs <pod> --previous
kubectl describe pod <pod>
```

Check:

* Application crash
* Wrong command/arguments
* Missing environment variables
* ConfigMap/Secret problems
* Liveness probe failure

### ImagePullBackOff

```bash
kubectl describe pod <pod>
```

Check:

* Image name
* Image tag
* Registry availability
* ImagePullSecrets
* Private registry authentication

### Service Not Working

```bash
kubectl get svc
kubectl describe svc <service>
kubectl get endpoints <service>
kubectl get pods --show-labels
```

Check:

```text
Service selector
      ↓
Pod labels
      ↓
Endpoints
      ↓
Application port
```

---

## 29. Imperative YAML Generation

Generate YAML without creating the resource:

```bash
kubectl create deployment nginx \
  --image=nginx \
  --dry-run=client \
  -o yaml
```

Generate a Pod:

```bash
kubectl run nginx \
  --image=nginx \
  --dry-run=client \
  -o yaml
```

Generate a Service:

```bash
kubectl expose deployment nginx \
  --port=80 \
  --dry-run=client \
  -o yaml
```

---

## 30. Namespace-Specific Commands

```bash
kubectl get pods -n <namespace>

kubectl get svc -n <namespace>

kubectl get deployments -n <namespace>

kubectl describe pod <pod> -n <namespace>

kubectl logs <pod> -n <namespace>

kubectl get all -n <namespace>
```

---

## 31. Context Management

```bash
# List contexts
kubectl config get-contexts

# Current context
kubectl config current-context

# Switch context
kubectl config use-context <context>

# List clusters
kubectl config get-clusters

# List users
kubectl config get-users

# Set default namespace
kubectl config set-context --current \
  --namespace=<namespace>
```

---

## 32. Pro Tips

### Alias `kubectl`

```bash
alias k=kubectl
```

Then:

```bash
k get pods
k get svc
k get nodes
k describe pod <pod>
```

### Generate YAML

```bash
kubectl create deployment nginx \
  --image=nginx \
  --dry-run=client \
  -o yaml
```

### Decode Secret

```bash
kubectl get secret <name> \
  -o jsonpath='{.data.password}' | base64 -d
```

### Explore API Fields

```bash
kubectl explain pod.spec.containers
```

### Check Permissions

```bash
kubectl auth can-i --list
kubectl auth can-i get pods
```

### Watch Rollout

```bash
kubectl rollout status deployment/<name> -w
```

### Watch Pods

```bash
kubectl get pods -w
```

### Get Recent Events

```bash
kubectl get events --sort-by=.lastTimestamp
```

---

## 33. Quick Troubleshooting Flow

```text
                 Pod Problem
                      │
                      ▼
             kubectl get pods
                      │
                      ▼
          kubectl describe pod
                      │
                      ▼
              Check Events
                      │
          ┌───────────┼───────────┐
          ▼           ▼           ▼
       Pending    Image Error   CrashLoop
          │           │           │
          ▼           ▼           ▼
      Scheduling   Image/      Logs &
      Resources    Registry    Previous Logs
          │           │           │
          └───────────┼───────────┘
                      ▼
                Fix Configuration
                      │
                      ▼
                Verify Pod
                      │
                      ▼
             kubectl get pods
```

---

## 34. Essential Commands — Interview Quick Reference

```bash
# Cluster
kubectl cluster-info
kubectl get nodes
kubectl get nodes -o wide

# Namespace
kubectl get ns
kubectl get all -n <ns>

# Pods
kubectl get pods
kubectl get pods -o wide
kubectl describe pod <pod>
kubectl logs <pod>
kubectl exec -it <pod> -- /bin/sh

# Deployment
kubectl get deploy
kubectl scale deploy <name> --replicas=3
kubectl set image deployment/<name> <container>=<image>:<tag>
kubectl rollout status deployment/<name>
kubectl rollout undo deployment/<name>

# Service
kubectl get svc
kubectl describe svc <service>
kubectl get endpoints <service>

# Config
kubectl get cm
kubectl get secrets

# Storage
kubectl get pv
kubectl get pvc
kubectl get sc

# Troubleshooting
kubectl get events --sort-by=.lastTimestamp
kubectl top pods
kubectl top nodes

# RBAC
kubectl auth can-i --list

# API discovery
kubectl api-resources
kubectl explain pod.spec
```

---

## 35. Golden Rule for Kubernetes Troubleshooting

> **Get → Describe → Logs → Events → Exec → Fix → Verify**

```text
1. kubectl get
       ↓
2. kubectl describe
       ↓
3. kubectl logs
       ↓
4. kubectl get events
       ↓
5. kubectl exec
       ↓
6. Fix configuration
       ↓
7. Verify again
```

This sequence covers a large percentage of day-to-day Kubernetes troubleshooting.

---

## Quick Reference

```text
Pods          → po
Deployments   → deploy
Services      → svc
ReplicaSets   → rs
DaemonSets    → ds
StatefulSets  → sts
Namespaces    → ns
Nodes         → no
ConfigMaps    → cm
Secrets       → secret
PV            → pv
PVC           → pvc
StorageClass  → sc
Ingress       → ing
ServiceAccount→ sa
Events        → ev
CronJob       → cj
```

---

> **Kubernetes mindset:** Don't just restart the pod. Find out **why Kubernetes created that state** and fix the underlying configuration, scheduling, networking, storage, or application problem.
