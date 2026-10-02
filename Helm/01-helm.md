# Helm — Kubernetes Package Manager

Helm is the **package manager for Kubernetes**.

It packages Kubernetes resources such as **Deployments, Services, ConfigMaps, Secrets, Ingress, and more** into reusable, versioned packages called **Charts**.

Instead of manually managing multiple Kubernetes YAML files, Helm lets you install and manage an application with a single command.

```bash
helm install my-app ./my-chart
```

Helm manages the complete application lifecycle:

**Install → Upgrade → Rollback → Uninstall**

---

## Why Helm?

Without Helm, an application may require multiple Kubernetes manifests:

```text
Deployment
Service
ConfigMap
Secret
Ingress
HPA
```

Managing these files separately across multiple environments can become difficult.

Helm provides:

```text
Chart
 ├── Templates
 ├── Default Values
 └── Metadata
        ↓
   Helm Rendering
        ↓
Kubernetes Manifests
        ↓
    Kubernetes Cluster
```

The same chart can be reused across environments by changing configuration values.

```text
Development → values-dev.yaml
Staging     → values-staging.yaml
Production  → values-prod.yaml
```

---

# Three Core Concepts

| Concept | Description |
|---|---|
| **Chart** | A package containing Kubernetes manifest templates, default values, and metadata |
| **Release** | A specific installed instance of a chart inside a Kubernetes cluster |
| **Repository** | A remote collection of Helm charts |

### Simple Relationship

```text
Helm Repository
       ↓
     Chart
       ↓
 helm install
       ↓
   Release
       ↓
Kubernetes Cluster
```

For example:

```bash
helm install my-nginx bitnami/nginx
```

Here:

- `bitnami` → Repository
- `nginx` → Chart
- `my-nginx` → Release

---

# Installation

Install Helm by following the official documentation:

[Helm Installation Guide](https://helm.sh/docs/intro/install/)

Verify the installation:

```bash
helm version
```

Check available commands:

```bash
helm help
```

---

# Managing Helm Repositories

## Add a Repository

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

## Update Repository Information

```bash
helm repo update
```

This downloads the latest chart information from configured repositories.

## List Configured Repositories

```bash
helm repo list
```

## Search Charts in Configured Repositories

```bash
helm search repo nginx
```

```bash
helm search repo bitnami
```

## Remove a Repository

```bash
helm repo remove bitnami
```

---

# Helm Chart Lifecycle

The basic Helm lifecycle is:

```text
Install
   ↓
Upgrade
   ↓
Revision 2
   ↓
Upgrade
   ↓
Revision 3
   ↓
Rollback
   ↓
New Revision
   ↓
Uninstall
```

---

## Install a Chart

Install a chart from a repository:

```bash
helm install my-nginx bitnami/nginx
```

Install a local chart:

```bash
helm install my-app ./my-chart
```

Install with a custom values file:

```bash
helm install my-app ./my-chart -f custom-values.yaml
```

Specify a namespace:

```bash
helm install my-app ./my-chart \
  --namespace dev \
  --create-namespace
```

---

## Upgrade a Release

Upgrade an existing release:

```bash
helm upgrade my-nginx bitnami/nginx
```

Change a value during upgrade:

```bash
helm upgrade my-nginx bitnami/nginx \
  --set replicaCount=5
```

Upgrade a local chart:

```bash
helm upgrade my-app ./my-chart
```

---

## Rollback a Release

View release history:

```bash
helm history my-nginx
```

Rollback to revision `1`:

```bash
helm rollback my-nginx 1
```

### Important

A rollback **does not delete or overwrite the existing revision history**.

It creates a **new revision** containing the previous configuration.

Example:

```text
Revision 1 → Install
Revision 2 → Upgrade
Revision 3 → Upgrade
Revision 4 → Rollback to revision 1
```

---

## Uninstall a Release

```bash
helm uninstall my-nginx
```

This removes the release and its managed Kubernetes resources.

To uninstall while retaining release history:

```bash
helm uninstall my-nginx --keep-history
```

The release is still uninstalled, but its history is retained.

---

# Inspecting Releases

## List Releases

List releases in the current namespace:

```bash
helm list
```

List releases across all namespaces:

```bash
helm list --all-namespaces
```

---

## Check Release Status

```bash
helm status my-nginx
```

Shows information such as:

- Release status
- Namespace
- Revision
- Chart
- App version
- Resources
- Notes

---

## View Release History

```bash
helm history my-nginx
```

Example:

```text
REVISION    STATUS
1           superseded
2           superseded
3           deployed
```

---

## View Release Values

```bash
helm get values my-nginx
```

Show all values, including chart defaults:

```bash
helm get values my-nginx --all
```

---

## View Rendered Kubernetes Manifests

```bash
helm get manifest my-nginx
```

This shows the Kubernetes YAML generated and stored for the release.

---

## View Release Notes

```bash
helm get notes my-nginx
```

---

# Customizing Charts with Values

One of Helm's biggest advantages is separating:

```text
Configuration → values.yaml
Structure     → templates/
```

The same chart can therefore be deployed differently across environments.

---

## Values Precedence

Helm values generally follow this precedence:

```text
Chart defaults
      ↓
-f values.yaml
      ↓
--set CLI values
```

The value provided later takes precedence.

Example:

```bash
helm install my-app ./my-chart \
  -f custom-values.yaml \
  --set replicaCount=5
```

If `custom-values.yaml` contains:

```yaml
replicaCount: 3
```

and the command contains:

```bash
--set replicaCount=5
```

the final value will be:

```yaml
replicaCount: 5
```

---

# Method 1 — Values File

Example `custom-values.yaml`:

```yaml
replicaCount: 3

service:
  type: NodePort
  port: 80

resources:
  requests:
    cpu: "100m"
    memory: "128Mi"

  limits:
    cpu: "250m"
    memory: "256Mi"
```

Install using the values file:

```bash
helm install my-app ./my-chart \
  -f custom-values.yaml
```

Multiple values files can also be supplied:

```bash
helm install my-app ./my-chart \
  -f values.yaml \
  -f values-prod.yaml
```

The later file takes precedence when the same value is defined more than once.

---

# Method 2 — `--set`

Values can also be supplied directly from the command line:

```bash
helm install my-app ./my-chart \
  --set replicaCount=5 \
  --set image.tag=latest \
  --set service.type=NodePort
```

This is useful for quick changes, CI/CD pipelines, and temporary overrides.

For larger configurations, prefer a values file.

---

# View Chart Default Values

To inspect a chart's default configuration:

```bash
helm show values bitnami/nginx
```

You can also inspect chart metadata:

```bash
helm show chart bitnami/nginx
```

Show the complete chart information:

```bash
helm show all bitnami/nginx
```

---

# Creating a Helm Chart

Create a new chart:

```bash
helm create my-app
```

This generates a standard chart structure.

```text
my-app/
├── Chart.yaml
├── values.yaml
├── charts/
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── serviceaccount.yaml
│   ├── _helpers.tpl
│   └── tests/
└── .helmignore
```

---

# Important Chart Files

## `Chart.yaml`

Contains chart metadata.

Example:

```yaml
apiVersion: v2
name: my-app
description: A Helm chart for Kubernetes
type: application
version: 0.1.0
appVersion: "1.0.0"
```

Important fields:

| Field | Purpose |
|---|---|
| `apiVersion` | Helm chart API version |
| `name` | Chart name |
| `description` | Description of the chart |
| `type` | `application` or `library` |
| `version` | Chart version |
| `appVersion` | Version of the application |

---

## `values.yaml`

Contains the chart's default configuration.

Example:

```yaml
replicaCount: 2

image:
  repository: nginx
  tag: "1.27"
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 80
```

---

## `templates/`

Contains Kubernetes manifest templates.

For example:

```text
templates/
├── deployment.yaml
├── service.yaml
├── ingress.yaml
└── _helpers.tpl
```

Helm processes these templates and generates Kubernetes manifests.

---

## `charts/`

Contains chart dependencies or subcharts.

---

## `.helmignore`

Specifies files that should not be included when packaging the chart.

---

# Go Templating

Helm uses the **Go template language**.

Template expressions use:

```text
{{ ... }}
```

Example:

```yaml
spec:
  replicas: {{ .Values.replicaCount }}

  template:
    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
```

Helm replaces these expressions with actual values during rendering.

---

# Common Helm Template Objects

## `.Values`

Accesses values from `values.yaml` or overrides.

```yaml
replicas: {{ .Values.replicaCount }}
```

---

## `.Chart`

Provides chart metadata.

```yaml
name: {{ .Chart.Name }}
```

---

## `.Release`

Provides release information.

```yaml
name: {{ .Release.Name }}
```

Common examples:

```yaml
{{ .Release.Name }}
{{ .Release.Namespace }}
{{ .Release.Revision }}
```

---

## `.Release.Name` Example

If you run:

```bash
helm install production ./my-app
```

then:

```yaml
{{ .Release.Name }}
```

evaluates to:

```text
production
```

---

# Example Deployment Template

`templates/deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: {{ .Release.Name }}

spec:
  replicas: {{ .Values.replicaCount }}

  selector:
    matchLabels:
      app: {{ .Chart.Name }}

  template:
    metadata:
      labels:
        app: {{ .Chart.Name }}

    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          ports:
            - containerPort: 80
```

With:

```yaml
replicaCount: 3

image:
  repository: nginx
  tag: "1.27"
```

Helm renders the template into normal Kubernetes YAML.

---

# Helm Development Workflow

A good workflow before deploying a chart is:

```text
Create Chart
     ↓
helm lint
     ↓
helm template
     ↓
helm install / upgrade
     ↓
helm status
     ↓
helm test
```

---

## 1. Lint the Chart

Check the chart for common issues:

```bash
helm lint ./my-app
```

---

## 2. Render Templates Locally

Render the Kubernetes YAML without installing anything:

```bash
helm template my-release ./my-app
```

This is extremely useful for debugging templates.

---

## 3. Dry Run an Installation

Simulate an installation:

```bash
helm install my-release ./my-app --dry-run
```

A more detailed debug output:

```bash
helm install my-release ./my-app \
  --dry-run \
  --debug
```

---

## 4. Install the Chart

```bash
helm install my-release ./my-app
```

---

## 5. Check the Release

```bash
helm status my-release
```

---

## 6. Upgrade the Release

```bash
helm upgrade my-release ./my-app \
  --set replicaCount=5
```

---

## 7. Rollback if Required

```bash
helm history my-release
```

```bash
helm rollback my-release 1
```

---

# Helm Testing

If the chart contains Helm tests:

```bash
helm test my-release
```

Helm tests are Kubernetes resources defined under:

```text
templates/tests/
```

They can be used to verify whether the deployed application behaves as expected.

---

# Packaging a Chart

Package a chart into a `.tgz` archive:

```bash
helm package ./my-app
```

Example output:

```text
my-app-0.1.0.tgz
```

This package can be distributed through a Helm repository or artifact registry.

---

# Useful Helm Commands

| Command | Purpose |
|---|---|
| `helm version` | Show Helm version |
| `helm help` | Show Helm help |
| `helm repo add` | Add a chart repository |
| `helm repo update` | Update repository information |
| `helm repo list` | List configured repositories |
| `helm search repo` | Search charts in repositories |
| `helm install` | Install a chart |
| `helm upgrade` | Upgrade a release |
| `helm rollback` | Roll back a release |
| `helm uninstall` | Remove a release |
| `helm list` | List releases |
| `helm status` | Show release status |
| `helm history` | Show release revisions |
| `helm get values` | Show release values |
| `helm get manifest` | Show rendered manifests |
| `helm show values` | Show chart default values |
| `helm create` | Create a new chart |
| `helm lint` | Validate a chart |
| `helm template` | Render templates locally |
| `helm test` | Run chart tests |
| `helm package` | Package a chart |

---

# Helm vs Raw Kubernetes YAML

| Kubernetes YAML | Helm |
|---|---|
| Manifests are managed individually | Resources are packaged as charts |
| More duplication across environments | Reusable templates |
| Configuration changes require editing YAML | Configuration can be changed through values |
| No built-in release revision management | Release history and revisions |
| Rollback must be handled separately | `helm rollback` |
| Difficult to package applications | Charts are versioned and distributable |

---

# Helm in CI/CD

Helm is commonly used in Kubernetes deployment pipelines.

```text
Developer
    ↓
Git Push
    ↓
CI Pipeline
    ↓
Build & Test
    ↓
Docker Image
    ↓
Container Registry
    ↓
Helm Chart
    ↓
helm upgrade
    ↓
Kubernetes Cluster
```

Example:

```bash
helm upgrade --install my-app ./helm/my-app \
  --namespace production \
  --create-namespace \
  --set image.tag=$IMAGE_TAG
```

This allows CI/CD to deploy a new application image without modifying the chart itself.

---

# Helm Best Practices

### 1. Keep configuration in values

Prefer:

```yaml
replicaCount: 3
```

over hardcoding:

```yaml
replicas: 3
```

inside templates.

---

### 2. Use environment-specific values files

```text
values.yaml
values-dev.yaml
values-staging.yaml
values-prod.yaml
```

---

### 3. Validate before deployment

Use:

```bash
helm lint ./my-app
```

and:

```bash
helm template my-release ./my-app
```

---

### 4. Version your charts

Update:

```yaml
version: 0.1.0
```

when releasing chart changes.

Keep `appVersion` aligned with the application version where appropriate.

---

### 5. Avoid excessive `--set`

Use values files for large or complex configuration.

Use `--set` for small overrides and CI/CD variables.

---

### 6. Review rendered manifests

Before deploying:

```bash
helm template my-release ./my-app
```

This helps catch:

- Incorrect indentation
- Wrong values
- Invalid Kubernetes fields
- Unexpected template output

---

# Quick Revision

```text
Helm
 │
 ├── Chart
 │     ├── Chart.yaml
 │     ├── values.yaml
 │     ├── templates/
 │     └── charts/
 │
 ├── Repository
 │     └── Collection of Charts
 │
 └── Release
       └── Installed instance of a Chart
```

### Most Important Commands

```bash
# Repository
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm search repo nginx

# Install
helm install my-app ./my-chart

# Inspect
helm list
helm status my-app
helm history my-app
helm get values my-app
helm get manifest my-app

# Upgrade
helm upgrade my-app ./my-chart

# Rollback
helm rollback my-app 1

# Uninstall
helm uninstall my-app

# Development
helm create my-app
helm lint ./my-app
helm template my-app ./my-app
helm package ./my-app

# Testing
helm test my-app
```

---

# Key Takeaways

- **Helm is the package manager for Kubernetes.**
- A **Chart** is a reusable package of Kubernetes templates and configuration.
- A **Release** is an installed instance of a chart.
- A **Repository** stores and distributes charts.
- `values.yaml` separates **configuration** from **template structure**.
- `helm template` renders Kubernetes manifests without deploying them.
- `helm lint` helps catch chart issues before deployment.
- Every install and upgrade creates a **release revision**.
- A rollback creates a **new revision** rather than rewriting history.
- `--keep-history` retains release history after uninstall.
- Helm is widely used in **Kubernetes CI/CD deployment workflows**.

> **Remember:**  
> **Chart = Package**  
> **Release = Installed Instance**  
> **Repository = Chart Storage**  
> **Values = Configuration**  
> **Templates = Kubernetes Structure**
