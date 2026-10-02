# Helm Dependencies — Charts, Subcharts & Dependency Management

Helm dependencies allow a chart to use other Helm charts as **dependencies**.

Instead of creating every Kubernetes component yourself, you can reuse existing charts and combine them into a single application deployment.

For example, an application may need:

```text
My Application
     │
     ├── PostgreSQL
     ├── Redis
     └── Nginx
```

Instead of creating separate charts manually, the main chart can declare these as dependencies.

```text
Parent Chart
     │
     ├── PostgreSQL Chart
     ├── Redis Chart
     └── Nginx Chart
```

---

# Why Use Helm Dependencies?

Dependencies provide:

- Reusable Kubernetes components
- Easier application packaging
- Version management
- Separation of responsibilities
- Consistent deployments
- Easier upgrades and rollbacks
- Ability to use well-maintained community charts

Example:

```text
Without Dependencies

Application Chart
 ├── deployment.yaml
 ├── service.yaml
 ├── postgres deployment
 ├── postgres service
 ├── redis deployment
 └── redis service
```

With dependencies:

```text
Application Chart
 │
 ├── Application Templates
 │
 └── Dependencies
      ├── PostgreSQL
      └── Redis
```

---

# Parent Chart and Child Chart

A chart that declares another chart as a dependency is commonly called the:

**Parent Chart**

The dependency is commonly called a:

**Subchart**

Example:

```text
my-app/
├── Chart.yaml
├── values.yaml
├── charts/
└── templates/
```

If PostgreSQL is added as a dependency:

```text
my-app/
├── Chart.yaml
├── values.yaml
├── charts/
│   └── postgresql-*.tgz
└── templates/
```

The PostgreSQL chart becomes a dependency of `my-app`.

---

# Declaring Dependencies

Helm dependencies are declared in:

```text
Chart.yaml
```

Example:

```yaml
apiVersion: v2

name: my-app

version: 1.0.0

dependencies:
  - name: postgresql
    version: 16.7.27
    repository: "https://charts.bitnami.com/bitnami"
```

The dependency contains three important fields:

```yaml
dependencies:
  - name: postgresql
    version: 16.7.27
    repository: "https://charts.bitnami.com/bitnami"
```

| Field | Purpose |
|---|---|
| `name` | Name of the dependency chart |
| `version` | Version or version constraint |
| `repository` | Helm repository containing the chart |

---

# Example: Adding Redis

```yaml
dependencies:
  - name: redis
    version: 20.6.2
    repository: "https://charts.bitnami.com/bitnami"
```

The complete `Chart.yaml` may look like:

```yaml
apiVersion: v2

name: my-app

description: Application with Redis dependency

type: application

version: 1.0.0

appVersion: "1.0.0"

dependencies:
  - name: redis
    version: 20.6.2
    repository: "https://charts.bitnami.com/bitnami"
```

---

# Adding Multiple Dependencies

A chart can have multiple dependencies.

```yaml
dependencies:

  - name: postgresql
    version: 16.7.27
    repository: "https://charts.bitnami.com/bitnami"

  - name: redis
    version: 20.6.2
    repository: "https://charts.bitnami.com/bitnami"
```

The dependency structure becomes:

```text
my-app
 │
 ├── Application
 │
 ├── PostgreSQL
 │
 └── Redis
```

---

# Helm Dependency Commands

The main commands are:

```bash
helm dependency update
helm dependency build
helm dependency list
```

---

# `helm dependency update`

Downloads the dependencies declared in `Chart.yaml`.

```bash
helm dependency update ./my-app
```

Helm:

```text
Read Chart.yaml
      ↓
Read dependencies
      ↓
Find repositories
      ↓
Download required charts
      ↓
Store them in charts/
      ↓
Create/Update Chart.lock
```

Example:

```bash
helm dependency update
```

---

# `charts/` Directory

After downloading dependencies:

```text
my-app/
├── Chart.yaml
├── Chart.lock
├── values.yaml
├── charts/
│   ├── postgresql-16.7.27.tgz
│   └── redis-20.6.2.tgz
└── templates/
```

The `.tgz` files are packaged Helm charts.

---

# `Chart.lock`

`Chart.lock` records the exact dependency versions that Helm resolved.

Example:

```yaml
dependencies:
  - name: postgresql
    repository: https://charts.bitnami.com/bitnami
    version: 16.7.27

digest: sha256:xxxxxxxx

generated: "2026-08-09T10:00:00Z"
```

The lock file helps make dependency resolution more predictable and reproducible.

---

# `Chart.yaml` vs `Chart.lock`

These files have different purposes.

| File | Purpose |
|---|---|
| `Chart.yaml` | Declares dependency requirements |
| `Chart.lock` | Locks the resolved dependency versions |
| `charts/` | Contains downloaded dependency charts |

Think of it as:

```text
Chart.yaml
    ↓
"What dependency do I want?"

Chart.lock
    ↓
"Which exact dependency was resolved?"

charts/
    ↓
"Downloaded dependency package"
```

---

# `helm dependency build`

Builds dependencies from the existing `Chart.lock`.

```bash
helm dependency build ./my-app
```

This is particularly useful when you want to reproduce the dependency versions already recorded in the lock file.

Typical workflow:

```text
Chart.yaml
    ↓
helm dependency update
    ↓
Chart.lock
    ↓
charts/
```

Later:

```text
Chart.lock
    ↓
helm dependency build
    ↓
charts/
```

---

# `helm dependency list`

View chart dependencies:

```bash
helm dependency list ./my-app
```

Example:

```text
NAME         VERSION    REPOSITORY
postgresql   16.7.27    https://charts.bitnami.com/bitnami
redis        20.6.2     https://charts.bitnami.com/bitnami
```

This is useful for quickly checking which dependencies a chart uses.

---

# Dependency Workflow

A typical dependency workflow is:

```text
Create Parent Chart
        ↓
Add dependencies to Chart.yaml
        ↓
helm dependency update
        ↓
Chart.lock generated
        ↓
Dependencies downloaded
        ↓
charts/ populated
        ↓
helm lint
        ↓
helm template
        ↓
helm install
```

---

# Version Constraints

Dependencies can use version constraints instead of requiring one exact version.

Example:

```yaml
dependencies:
  - name: redis
    version: "20.x"
    repository: "https://charts.bitnami.com/bitnami"
```

Another example:

```yaml
version: ">=20.0.0 <21.0.0"
```

This allows Helm to resolve a compatible chart version.

---

# Exact Version

Example:

```yaml
version: "20.6.2"
```

This requests a specific chart version.

Advantages:

- Predictable
- Reproducible
- Easier to control upgrades

---

# Version Range

Example:

```yaml
version: "~20.6.0"
```

This allows compatible patch-level updates according to Helm's semantic version constraint rules.

Another example:

```yaml
version: "^20.6.0"
```

Version constraints are useful when you want controlled flexibility rather than completely unrestricted upgrades.

---

# Semantic Versioning

Helm dependencies commonly use **Semantic Versioning (SemVer)**.

A version generally follows:

```text
MAJOR.MINOR.PATCH
```

Example:

```text
20.6.2
```

Where:

```text
20 → Major
 6 → Minor
 2 → Patch
```

Major versions may contain breaking changes, so dependency upgrades should be tested carefully.

---

# Repository URLs

A dependency can reference a Helm repository:

```yaml
repository: "https://charts.bitnami.com/bitnami"
```

Helm uses this repository to locate the dependency chart.

Before working with a dependency, you can add the repository:

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

Then:

```bash
helm repo update
```

---

# OCI Registries

Helm also supports OCI-based chart registries.

An OCI dependency can use a repository such as:

```yaml
dependencies:
  - name: my-chart
    version: "1.2.3"
    repository: "oci://registry.example.com/helm"
```

OCI support allows Helm charts to be stored in OCI-compatible registries.

This is increasingly common in modern CI/CD environments.

---

# Local Chart Dependencies

A dependency can also be stored locally.

Example:

```yaml
dependencies:
  - name: database
    version: "1.0.0"
    repository: "file://../database"
```

Directory structure:

```text
project/
├── database/
│   ├── Chart.yaml
│   ├── values.yaml
│   └── templates/
│
└── my-app/
    ├── Chart.yaml
    ├── values.yaml
    └── templates/
```

The parent chart references the local chart using:

```yaml
repository: "file://../database"
```

---

# Subchart Values

A parent chart can provide configuration to a dependency through `values.yaml`.

Suppose the dependency is:

```yaml
dependencies:
  - name: redis
    version: "20.6.2"
    repository: "https://charts.bitnami.com/bitnami"
```

The parent chart can configure Redis:

```yaml
redis:
  architecture: standalone

  auth:
    enabled: false

  master:
    persistence:
      enabled: false
```

The dependency name:

```text
redis
```

becomes the top-level key:

```yaml
redis:
```

---

# Example: Parent Chart Values

```yaml
# values.yaml

replicaCount: 2

image:
  repository: nginx
  tag: "1.27"

redis:
  architecture: standalone

  auth:
    enabled: false
```

The structure is:

```text
Parent Chart
│
├── replicaCount
├── image
│
└── redis
      ├── architecture
      └── auth
```

---

# Global Values

Helm supports a special:

```yaml
global:
```

section for values that can be shared across charts.

Example:

```yaml
global:
  environment: production
  imageRegistry: registry.example.com
```

A dependency can access global values using:

```gotemplate
{{ .Values.global.environment }}
```

This is useful when multiple charts need common configuration.

---

# Global vs Dependency-Specific Values

### Global

```yaml
global:
  environment: production
```

Used for values shared across multiple charts.

### Dependency-specific

```yaml
redis:
  architecture: standalone
```

Used specifically for the Redis dependency.

Example:

```yaml
global:
  environment: production

redis:
  architecture: standalone

postgresql:
  auth:
    database: appdb
```

---

# Dependency Conditions

Dependencies can be conditionally enabled.

Example:

```yaml
dependencies:
  - name: redis
    version: "20.6.2"
    repository: "https://charts.bitnami.com/bitnami"
    condition: redis.enabled
```

Then in `values.yaml`:

```yaml
redis:
  enabled: true
```

If:

```yaml
redis:
  enabled: false
```

the Redis dependency is disabled.

---

# Conditional Dependency Example

`Chart.yaml`:

```yaml
dependencies:
  - name: redis
    version: "20.6.2"
    repository: "https://charts.bitnami.com/bitnami"
    condition: redis.enabled
```

`values.yaml`:

```yaml
redis:
  enabled: false
```

Enable Redis:

```bash
helm install my-app ./my-app \
  --set redis.enabled=true
```

Disable Redis:

```bash
helm install my-app ./my-app \
  --set redis.enabled=false
```

---

# Dependency Tags

Tags can also be used to enable or disable groups of dependencies.

Example:

```yaml
dependencies:
  - name: redis
    version: "20.6.2"
    repository: "https://charts.bitnami.com/bitnami"
    tags:
      - caching
```

Another dependency:

```yaml
dependencies:
  - name: memcached
    version: "7.5.2"
    repository: "https://charts.bitnami.com/bitnami"
    tags:
      - caching
```

Values:

```yaml
tags:
  caching: true
```

This allows multiple dependencies to be controlled together.

---

# Dependency Aliases

An alias allows the same chart to be included more than once under different names.

Example:

```yaml
dependencies:

  - name: redis
    alias: redis-cache
    version: "20.6.2"
    repository: "https://charts.bitnami.com/bitnami"

  - name: redis
    alias: redis-session
    version: "20.6.2"
    repository: "https://charts.bitnami.com/bitnami"
```

Now the parent chart can configure them separately:

```yaml
redis-cache:
  architecture: standalone

redis-session:
  architecture: standalone
```

This is useful when the same chart needs to serve different purposes.

---

# Dependency Tags vs Conditions

Both can control dependencies, but they are useful in different situations.

### Condition

Good for controlling an individual dependency:

```yaml
condition: redis.enabled
```

### Tags

Good for controlling multiple related dependencies:

```yaml
tags:
  - caching
```

Example:

```text
cache-enabled
      ↓
 ┌────┴────┐
 ↓         ↓
Redis   Memcached
```

---

# Dependency Update vs Dependency Build

This distinction is important.

## `helm dependency update`

Resolves dependencies from `Chart.yaml`.

```bash
helm dependency update
```

It can update the lock information and downloaded dependencies.

Use it when:

- Adding a dependency
- Changing dependency versions
- Intentionally refreshing dependency resolution

---

## `helm dependency build`

Builds dependencies using the existing lock information.

```bash
helm dependency build
```

Use it when:

- You already have `Chart.lock`
- You want to reproduce locked dependencies
- Building in CI/CD
- You want predictable dependency versions

---

# Dependency Update vs Build — Quick Comparison

| Command | Main Purpose |
|---|---|
| `helm dependency update` | Resolve/update dependencies and lock information |
| `helm dependency build` | Rebuild dependencies from `Chart.lock` |
| `helm dependency list` | List declared dependencies |

Remember:

```text
Update → Resolve / Refresh

Build → Reproduce Locked Dependencies
```

---

# Example Project

Suppose we have:

```text
my-app/
├── Chart.yaml
├── Chart.lock
├── values.yaml
├── charts/
└── templates/
    ├── deployment.yaml
    └── service.yaml
```

`Chart.yaml`:

```yaml
apiVersion: v2

name: my-app

version: 1.0.0

appVersion: "1.0.0"

dependencies:

  - name: redis
    version: "20.6.2"
    repository: "https://charts.bitnami.com/bitnami"
    condition: redis.enabled
```

`values.yaml`:

```yaml
replicaCount: 2

redis:
  enabled: true
  architecture: standalone

  auth:
    enabled: false
```

Update dependencies:

```bash
helm dependency update ./my-app
```

Check them:

```bash
helm dependency list ./my-app
```

Render:

```bash
helm template my-app ./my-app
```

Install:

```bash
helm install my-app ./my-app
```

Check:

```bash
helm status my-app
```

---

# Packaging a Chart with Dependencies

Before packaging:

```bash
helm dependency update ./my-app
```

Then:

```bash
helm package ./my-app
```

The resulting package includes the chart and its packaged dependencies.

Example:

```text
my-app-1.0.0.tgz
```

---

# Dependency Management in CI/CD

A typical CI/CD workflow can look like:

```text
Git Push
   ↓
CI Pipeline
   ↓
Checkout Repository
   ↓
helm dependency build
   ↓
helm lint
   ↓
helm template
   ↓
helm package
   ↓
Publish Chart
   ↓
Deploy
```

Example:

```bash
helm dependency build ./my-app

helm lint ./my-app

helm template my-release ./my-app

helm package ./my-app
```

Using `Chart.lock` helps make dependency versions reproducible across environments.

---

# Dependency Security

Helm dependencies introduce third-party code and configuration into your deployment.

Before using a dependency:

- Check the chart source
- Review chart versions
- Pin or constrain versions
- Review `values.yaml`
- Review rendered manifests
- Keep dependencies updated
- Scan container images where applicable
- Avoid blindly trusting third-party charts

Always inspect what a dependency actually creates.

Useful command:

```bash
helm template my-app ./my-app
```

---

# Dependency Troubleshooting

## Repository Not Found

Error may occur if Helm cannot locate the repository.

Check:

```bash
helm repo list
```

Update:

```bash
helm repo update
```

---

## Dependency Missing

If the `charts/` directory is missing dependencies:

```bash
helm dependency update
```

or:

```bash
helm dependency build
```

depending on whether you want to resolve or reproduce locked dependencies.

---

## Version Not Found

Check available chart versions:

```bash
helm search repo redis --versions
```

Then select a compatible version in:

```yaml
dependencies:
  - name: redis
    version: "20.6.2"
    repository: "https://charts.bitnami.com/bitnami"
```

---

# Useful Commands

```bash
# Add repository
helm repo add bitnami https://charts.bitnami.com/bitnami

# Update repositories
helm repo update

# Search chart versions
helm search repo redis --versions

# Update dependencies
helm dependency update ./my-app

# Build dependencies
helm dependency build ./my-app

# List dependencies
helm dependency list ./my-app

# Render chart with dependencies
helm template my-app ./my-app

# Lint chart
helm lint ./my-app

# Package chart
helm package ./my-app
```

---

# Best Practices

### 1. Declare dependencies in `Chart.yaml`

Do not manually copy dependency charts into your project.

Use:

```yaml
dependencies:
  - name: redis
    version: "20.6.2"
    repository: "https://charts.bitnami.com/bitnami"
```

---

### 2. Use controlled versions

Avoid unnecessarily broad version ranges.

Prefer predictable versions when reproducibility is important.

---

### 3. Keep `Chart.lock`

For applications where reproducible dependency resolution matters, commit `Chart.lock` to Git.

---

### 4. Review dependency changes

When updating dependencies, inspect:

```text
Chart.yaml
Chart.lock
charts/
```

and review the rendered output:

```bash
helm template my-app ./my-app
```

---

### 5. Use conditions for optional dependencies

Example:

```yaml
condition: redis.enabled
```

This is useful for optional components.

---

### 6. Use aliases when the same chart is required multiple times

Example:

```yaml
alias: redis-cache
```

and:

```yaml
alias: redis-session
```

---

### 7. Test dependency upgrades

Do not upgrade production dependencies blindly.

Test:

```bash
helm dependency update
helm lint ./my-app
helm template my-app ./my-app
```

before deployment.

---

# Quick Revision

```text
Chart.yaml
    │
    └── dependencies:
            │
            ├── Redis
            ├── PostgreSQL
            └── Other Charts
                    ↓
          helm dependency update
                    ↓
               Chart.lock
                    ↓
                 charts/
                    ↓
             Parent Chart
                    ↓
            Helm Rendering
                    ↓
             Kubernetes
```

### Important Files

```text
Chart.yaml
    → Dependency declarations

Chart.lock
    → Resolved dependency versions

charts/
    → Downloaded dependency packages

values.yaml
    → Dependency configuration
```

### Important Commands

```bash
helm dependency update
helm dependency build
helm dependency list
```

---

# Key Takeaways

- Helm charts can depend on other Helm charts.
- The parent chart declares dependencies in `Chart.yaml`.
- Dependencies are commonly called **subcharts**.
- `helm dependency update` resolves and downloads dependencies.
- `helm dependency build` rebuilds dependencies using `Chart.lock`.
- `Chart.lock` helps make dependency resolution reproducible.
- Downloaded dependencies are stored under `charts/`.
- Dependency configuration can be provided through `values.yaml`.
- `condition` can enable or disable individual dependencies.
- `tags` can control groups of dependencies.
- `alias` allows the same chart to be included multiple times.
- Dependencies can come from Helm repositories, OCI registries, or local paths.
- Always inspect dependency output before deploying.
- Dependency management is especially important in **CI/CD and production Kubernetes environments**.

> **Remember:**
>
> **Chart.yaml = What dependencies are required**  
> **Chart.lock = What versions were resolved**  
> **charts/ = Downloaded dependency packages**  
> **values.yaml = How dependencies are configured**  
> **helm dependency update = Resolve / refresh**  
> **helm dependency build = Reproduce locked dependencies**
