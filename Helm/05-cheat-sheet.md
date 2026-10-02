# Helm Cheat Sheet — Kubernetes Package Manager

Helm is the **package manager for Kubernetes**.

```text
Chart → Package
Release → Installed Chart
Repository → Chart Storage
Values → Configuration
Templates → Kubernetes Structure
```

---

# Installation

```bash
# Check Helm version
helm version

# Help
helm help
```

Official installation guide:

https://helm.sh/docs/intro/install/

---

# Repository Commands

```bash
# Add repository
helm repo add bitnami https://charts.bitnami.com/bitnami

# Update repositories
helm repo update

# List repositories
helm repo list

# Search charts
helm search repo nginx

# Search all available versions
helm search repo nginx --versions

# Remove repository
helm repo remove bitnami
```

---

# Chart Commands

```bash
# Create a new chart
helm create my-app

# Show chart metadata
helm show chart bitnami/nginx

# Show default values
helm show values bitnami/nginx

# Show complete chart information
helm show all bitnami/nginx

# Package chart
helm package ./my-app

# Lint chart
helm lint ./my-app
```

---

# Install

```bash
# Install chart from repository
helm install my-nginx bitnami/nginx

# Install local chart
helm install my-app ./my-app

# Install with values file
helm install my-app ./my-app \
  -f values-prod.yaml

# Install with namespace
helm install my-app ./my-app \
  --namespace production \
  --create-namespace

# Install with CLI values
helm install my-app ./my-app \
  --set replicaCount=3
```

---

# Upgrade

```bash
# Upgrade release
helm upgrade my-app ./my-app

# Upgrade with values file
helm upgrade my-app ./my-app \
  -f values-prod.yaml

# Upgrade with CLI value
helm upgrade my-app ./my-app \
  --set replicaCount=5

# Install if release does not exist, otherwise upgrade
helm upgrade --install my-app ./my-app
```

---

# Release Management

```bash
# List releases
helm list

# List releases in all namespaces
helm list --all-namespaces

# Check release status
helm status my-app

# Show release history
helm history my-app

# Show release values
helm get values my-app

# Show all computed values
helm get values my-app --all

# Show rendered manifests
helm get manifest my-app

# Show release notes
helm get notes my-app
```

---

# Rollback

```bash
# View revisions
helm history my-app

# Rollback to revision 1
helm rollback my-app 1

# Check status
helm status my-app
```

Important:

```text
Install  → Revision 1
Upgrade  → Revision 2
Upgrade  → Revision 3
Rollback → Revision 4
```

A rollback creates a **new revision**. It does not overwrite history.

---

# Uninstall

```bash
# Remove release
helm uninstall my-app

# Remove release but keep history
helm uninstall my-app --keep-history
```

---

# Values

### `values.yaml`

```yaml
replicaCount: 2

image:
  repository: nginx
  tag: "1.27"

service:
  type: ClusterIP
  port: 80
```

### Values File

```bash
helm install my-app ./my-app \
  -f values-prod.yaml
```

### CLI Override

```bash
helm install my-app ./my-app \
  --set replicaCount=5 \
  --set image.tag=latest
```

### Precedence

```text
Chart Defaults
      ↓
-f values.yaml
      ↓
--set
```

**Last value wins.**

---

# Helm Templates

Template syntax:

```gotemplate
{{ .Values.replicaCount }}
```

Common objects:

```gotemplate
{{ .Values }}
{{ .Chart.Name }}
{{ .Release.Name }}
{{ .Release.Namespace }}
{{ .Release.Revision }}
{{ .Capabilities.KubeVersion.Version }}
```

---

# Template Functions

```gotemplate
{{ .Values.name | quote }}

{{ .Values.name | upper }}

{{ .Values.name | lower }}

{{ .Values.value | default "default-value" }}

{{ required "value is required" .Values.value }}

{{ toYaml .Values.resources | nindent 12 }}
```

---

# Conditions

```gotemplate
{{ if .Values.ingress.enabled }}
...
{{ end }}
```

```gotemplate
{{ if .Values.enabled }}
...
{{ else }}
...
{{ end }}
```

Multiple conditions:

```gotemplate
{{ if and .Values.a .Values.b }}
...
{{ end }}
```

```gotemplate
{{ if or .Values.a .Values.b }}
...
{{ end }}
```

---

# Loops

List:

```gotemplate
{{ range .Values.ports }}
- {{ . }}
{{ end }}
```

Map:

```gotemplate
{{ range $key, $value := .Values.labels }}
{{ $key }}: {{ $value | quote }}
{{ end }}
```

---

# `with`

```gotemplate
{{ with .Values.image }}
repository: {{ .repository }}
tag: {{ .tag }}
{{ end }}
```

Inside `with`, `.` refers to the current context.

---

# Named Templates

Define in:

```text
templates/_helpers.tpl
```

```gotemplate
{{- define "my-app.name" -}}
{{ .Chart.Name }}
{{- end }}
```

Use:

```gotemplate
{{ include "my-app.name" . }}
```

Common pattern:

```gotemplate
labels:
  {{- include "my-app.labels" . | nindent 4 }}
```

---

# Template Rendering

```bash
# Render templates
helm template my-app ./my-app

# Render with values
helm template my-app ./my-app \
  -f values-prod.yaml

# Override value
helm template my-app ./my-app \
  --set replicaCount=5

# Save rendered YAML
helm template my-app ./my-app > rendered.yaml

# Debug
helm template my-app ./my-app --debug
```

---

# Dry Run

```bash
helm install my-app ./my-app \
  --dry-run \
  --debug
```

Difference:

```text
helm template
    ↓
Render YAML locally

helm install --dry-run
    ↓
Simulate installation
```

---

# Kubernetes Validation

```bash
# Client-side validation
kubectl apply \
  --dry-run=client \
  -f rendered.yaml

# Server-side validation
kubectl apply \
  --dry-run=server \
  -f rendered.yaml
```

---

# Dependencies

Declare dependencies in:

```text
Chart.yaml
```

Example:

```yaml
dependencies:
  - name: redis
    version: "20.6.2"
    repository: "https://charts.bitnami.com/bitnami"
```

Commands:

```bash
# Update / resolve dependencies
helm dependency update ./my-app

# Build from Chart.lock
helm dependency build ./my-app

# List dependencies
helm dependency list ./my-app
```

Important files:

```text
Chart.yaml
    ↓
Dependency declarations

Chart.lock
    ↓
Resolved dependency versions

charts/
    ↓
Downloaded dependencies
```

---

# Dependency Conditions

```yaml
dependencies:
  - name: redis
    version: "20.6.2"
    repository: "https://charts.bitnami.com/bitnami"
    condition: redis.enabled
```

Values:

```yaml
redis:
  enabled: true
```

Disable:

```bash
helm install my-app ./my-app \
  --set redis.enabled=false
```

---

# Helm Hooks

Hook annotation:

```yaml
annotations:
  "helm.sh/hook": pre-install
```

Common hooks:

```text
pre-install
post-install

pre-upgrade
post-upgrade

pre-rollback
post-rollback

pre-delete
post-delete

test
```

---

# Hook Weight

Controls execution order.

```yaml
annotations:
  "helm.sh/hook": pre-install
  "helm.sh/hook-weight": "-5"
```

Lower weight runs first.

```text
-5 → 0 → 5
```

---

# Hook Cleanup

```yaml
annotations:
  "helm.sh/hook-delete-policy": hook-succeeded
```

Other policies:

```text
before-hook-creation
hook-succeeded
hook-failed
```

Multiple policies:

```yaml
"helm.sh/hook-delete-policy": before-hook-creation,hook-succeeded
```

---

# Helm Tests

Test hook:

```yaml
annotations:
  "helm.sh/hook": test
```

Run:

```bash
helm test my-app
```

Inspect test Jobs:

```bash
kubectl get jobs
```

View logs:

```bash
kubectl logs job/<job-name>
```

Describe:

```bash
kubectl describe job <job-name>
```

---

# Testing Workflow

```bash
# 1. Lint
helm lint ./my-app

# 2. Render
helm template my-app ./my-app

# 3. Dry run
helm install my-app ./my-app \
  --dry-run \
  --debug

# 4. Validate manifests
helm template my-app ./my-app > rendered.yaml

kubectl apply \
  --dry-run=server \
  -f rendered.yaml

# 5. Deploy
helm upgrade --install my-app ./my-app

# 6. Test
helm test my-app
```

---

# Debugging

```bash
# Release status
helm status my-app

# Release history
helm history my-app

# Render templates
helm template my-app ./my-app --debug

# List Pods
kubectl get pods

# List Jobs
kubectl get jobs

# Describe resource
kubectl describe pod <pod-name>

# Job details
kubectl describe job <job-name>

# Logs
kubectl logs <pod-name>

# Job logs
kubectl logs job/<job-name>

# Cluster events
kubectl get events --sort-by=.lastTimestamp
```

---

# Chart Structure

```text
my-app/
├── Chart.yaml
├── Chart.lock
├── values.yaml
├── charts/
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── _helpers.tpl
│   └── tests/
└── .helmignore
```

---

# Important `Chart.yaml` Fields

```yaml
apiVersion: v2

name: my-app

description: My Helm application

type: application

version: 1.0.0

appVersion: "1.0.0"

dependencies:
  - name: redis
    version: "20.6.2"
    repository: "https://charts.bitnami.com/bitnami"
```

Remember:

```text
version
    ↓
Chart version

appVersion
    ↓
Application version
```

---

# Helm Lifecycle

```text
                 Helm
                  │
        ┌─────────┴─────────┐
        ↓                   ↓
      Chart             Repository
        │
        ↓
      Install
        │
        ↓
     Release
        │
   ┌────┼────┐
   ↓    ↓    ↓
Upgrade Rollback Uninstall
```

---

# Helm + Kubernetes Flow

```text
Chart
  │
  ├── values.yaml
  └── templates/
          ↓
      Helm Engine
          ↓
     Rendered YAML
          ↓
   Kubernetes API Server
          ↓
    Kubernetes Resources
```

---

# Helm + CI/CD

```text
Developer
    ↓
Git Push
    ↓
CI Pipeline
    ↓
helm dependency build
    ↓
helm lint
    ↓
helm template
    ↓
Kubernetes Validation
    ↓
helm upgrade --install
    ↓
helm test
    ↓
Production
```

Example:

```bash
helm dependency build ./my-app

helm lint ./my-app

helm template my-app ./my-app > rendered.yaml

kubectl apply \
  --dry-run=server \
  -f rendered.yaml

helm upgrade --install my-app ./my-app

helm test my-app
```

---

# Most Important Commands

| Command | Purpose |
|---|---|
| `helm version` | Show Helm version |
| `helm repo add` | Add chart repository |
| `helm repo update` | Update repository information |
| `helm search repo` | Search charts |
| `helm create` | Create chart |
| `helm install` | Install release |
| `helm upgrade` | Upgrade release |
| `helm upgrade --install` | Install or upgrade |
| `helm rollback` | Roll back release |
| `helm uninstall` | Remove release |
| `helm list` | List releases |
| `helm status` | Show release status |
| `helm history` | Show revision history |
| `helm get values` | Show release values |
| `helm get manifest` | Show rendered manifests |
| `helm show values` | Show default chart values |
| `helm lint` | Validate chart |
| `helm template` | Render templates |
| `helm test` | Run Helm tests |
| `helm package` | Package chart |
| `helm dependency update` | Resolve dependencies |
| `helm dependency build` | Build locked dependencies |
| `helm dependency list` | List dependencies |

---

# Common Helm Flags

```bash
# Values file
-f values.yaml

# Set value
--set key=value

# Namespace
--namespace production

# Create namespace
--create-namespace

# Dry run
--dry-run

# Debug
--debug

# Wait for resources
--wait

# Set timeout
--timeout 5m

# Reuse previous values during upgrade
--reuse-values

# Reset to chart defaults during upgrade
--reset-values
```

---

# Useful Installation Pattern

```bash
helm upgrade --install my-app ./my-app \
  --namespace production \
  --create-namespace \
  -f values-prod.yaml \
  --wait \
  --timeout 5m
```

This is a common CI/CD deployment pattern.

---

# Helm Interview Quick Revision

### What is Helm?

Helm is the **package manager for Kubernetes**.

### What is a Chart?

A reusable package of Kubernetes templates and configuration.

### What is a Release?

An installed instance of a Helm chart.

### What is `values.yaml`?

The default configuration for a chart.

### What is `Chart.yaml`?

Chart metadata and dependency declarations.

### What is `Chart.lock`?

The lock file containing resolved dependency information.

### What is `helm template`?

Renders Kubernetes manifests without deploying them.

### What is `helm lint`?

Checks a chart for common structural and configuration issues.

### What is `helm rollback`?

Restores a previous release revision by creating a new revision.

### What is a Helm Hook?

A Kubernetes resource executed at a specific Helm lifecycle event.

### What is `helm test`?

Runs tests defined using Helm test hooks.

### What is a Subchart?

A chart used as a dependency by another chart.

---

# One-Minute Helm Revision

```text
HELM
 │
 ├── Chart
 │     ├── Chart.yaml
 │     ├── values.yaml
 │     ├── templates/
 │     └── charts/
 │
 ├── Repository
 │
 └── Release
       ├── Install
       ├── Upgrade
       ├── Rollback
       └── Uninstall
```

```text
Templates
   ↓
.Values
.Chart
.Release
   ↓
Functions
   ↓
Rendered YAML
```

```text
Dependencies
   ↓
Chart.yaml
   ↓
Chart.lock
   ↓
charts/
```

```text
Testing
   ↓
helm lint
   ↓
helm template
   ↓
dry-run
   ↓
kubectl validation
   ↓
helm test
```

---

# Golden Helm Workflow

```bash
# Create
helm create my-app

# Develop
helm lint ./my-app

# Render
helm template my-app ./my-app

# Validate
helm install my-app ./my-app \
  --dry-run \
  --debug

# Dependencies
helm dependency update ./my-app

# Deploy
helm upgrade --install my-app ./my-app

# Verify
helm status my-app

# Test
helm test my-app

# Inspect
helm history my-app

# Rollback if required
helm rollback my-app <REVISION>

# Remove
helm uninstall my-app
```

---

# Key Takeaways

```text
Chart      → Package
Release    → Installed Chart
Repository → Chart Storage
Values     → Configuration
Templates  → Kubernetes Structure
Hooks      → Lifecycle Actions
Tests      → Application Verification
Dependencies → Reusable Charts
```

> **Remember:**
>
> `helm lint` → **Check the chart**  
> `helm template` → **See the generated YAML**  
> `helm install` → **Create a release**  
> `helm upgrade` → **Update a release**  
> `helm rollback` → **Restore a previous revision**  
> `helm test` → **Verify application behavior**  
> `helm dependency` → **Manage subcharts**  
> `helm uninstall` → **Remove the release**
