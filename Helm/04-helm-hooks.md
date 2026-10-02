# Helm Hooks — Lifecycle Events & Custom Actions

Helm Hooks allow you to run **Kubernetes resources at specific points during a Helm release lifecycle**.

Hooks are useful when you need to perform an action **before, during, or after** operations such as:

- Install
- Upgrade
- Rollback
- Uninstall
- Test

Examples:

```text
Helm Install
     ↓
pre-install Hook
     ↓
Resources Created
     ↓
post-install Hook
```

A hook is simply a Kubernetes resource with a special Helm annotation.

---

# Why Helm Hooks?

Normal Helm templates are rendered and managed as part of the release.

Hooks allow certain resources to run at specific lifecycle events.

Common use cases:

- Database migrations
- Pre-deployment validation
- Backup before upgrade
- Cleanup tasks
- Initialization jobs
- Post-deployment verification
- Helm tests

Example:

```text
helm upgrade
      ↓
Run database migration
      ↓
Deploy application
      ↓
Run verification
```

---

# How Helm Hooks Work

A hook is created by adding:

```yaml
metadata:
  annotations:
    "helm.sh/hook": <hook-type>
```

Example:

```yaml
apiVersion: batch/v1
kind: Job

metadata:
  name: database-migration
  annotations:
    "helm.sh/hook": pre-install
```

Helm detects the annotation and executes the resource at the specified lifecycle point.

---

# Hook Types

Common Helm hook types:

| Hook | When it runs |
|---|---|
| `pre-install` | Before resources are installed |
| `post-install` | After resources are installed |
| `pre-upgrade` | Before an upgrade |
| `post-upgrade` | After an upgrade |
| `pre-rollback` | Before a rollback |
| `post-rollback` | After a rollback |
| `pre-delete` | Before a release is deleted |
| `post-delete` | After a release is deleted |
| `test` | When `helm test` is executed |

---

# Hook Lifecycle

## Install

```text
helm install
     ↓
pre-install
     ↓
Install Resources
     ↓
post-install
```

---

## Upgrade

```text
helm upgrade
     ↓
pre-upgrade
     ↓
Upgrade Resources
     ↓
post-upgrade
```

---

## Rollback

```text
helm rollback
     ↓
pre-rollback
     ↓
Rollback Resources
     ↓
post-rollback
```

---

## Uninstall

```text
helm uninstall
     ↓
pre-delete
     ↓
Delete Resources
     ↓
post-delete
```

---

# Basic Hook Example

Create:

```text
templates/migration-job.yaml
```

```yaml
apiVersion: batch/v1
kind: Job

metadata:
  name: {{ .Release.Name }}-migration

  annotations:
    "helm.sh/hook": pre-install

spec:
  template:
    spec:
      restartPolicy: Never

      containers:
        - name: migration
          image: myapp:1.0
          command:
            - /bin/sh
            - -c
            - echo "Running database migration..."
```

The Job runs as a `pre-install` hook.

---

# Multiple Hook Events

A resource can be associated with multiple hook events.

```yaml
annotations:
  "helm.sh/hook": pre-install,pre-upgrade
```

This means the hook runs:

```text
helm install → pre-install
helm upgrade → pre-upgrade
```

---

# Post-Install Hook

Example:

```yaml
apiVersion: batch/v1
kind: Job

metadata:
  name: {{ .Release.Name }}-verification

  annotations:
    "helm.sh/hook": post-install

spec:
  template:
    spec:
      restartPolicy: Never

      containers:
        - name: verification
          image: busybox:1.36
          command:
            - sh
            - -c
            - echo "Application deployed successfully"
```

---

# Hook Deletion Policies

Hook resources can remain in the cluster after execution unless you configure a deletion policy.

Use:

```yaml
"helm.sh/hook-delete-policy": before-hook-creation
```

Common policies:

| Policy | Purpose |
|---|---|
| `before-hook-creation` | Delete the previous hook resource before creating a new one |
| `hook-succeeded` | Delete the hook after successful execution |
| `hook-failed` | Delete the hook if execution fails |

Multiple policies can be combined:

```yaml
"helm.sh/hook-delete-policy": before-hook-creation,hook-succeeded
```

---

# Example with Cleanup

```yaml
metadata:
  name: {{ .Release.Name }}-migration

  annotations:
    "helm.sh/hook": pre-upgrade
    "helm.sh/hook-delete-policy": before-hook-creation,hook-succeeded
```

This helps prevent old Job resources from accumulating.

---

# Hook Weights

Helm supports hook weights to control the execution order of hooks.

Use:

```yaml
"helm.sh/hook-weight": "0"
```

Lower values run before higher values.

Example:

```yaml
# Hook 1
"helm.sh/hook-weight": "-5"
```

```yaml
# Hook 2
"helm.sh/hook-weight": "0"
```

```yaml
# Hook 3
"helm.sh/hook-weight": "5"
```

Execution order:

```text
-5
 ↓
 0
 ↓
 5
```

This is useful when multiple hooks must execute in a specific order.

---

# Hook Example with Weights

```yaml
annotations:
  "helm.sh/hook": pre-install
  "helm.sh/hook-weight": "-5"
```

Another:

```yaml
annotations:
  "helm.sh/hook": pre-install
  "helm.sh/hook-weight": "5"
```

The `-5` hook runs first.

---

# Hook Delete Policy + Weight

A production-style hook may look like:

```yaml
annotations:
  "helm.sh/hook": pre-upgrade
  "helm.sh/hook-weight": "-5"
  "helm.sh/hook-delete-policy": before-hook-creation,hook-succeeded
```

This means:

```text
Before Upgrade
      ↓
Delete previous hook if necessary
      ↓
Run hook
      ↓
Successful completion
      ↓
Delete hook Job
      ↓
Continue upgrade
```

---

# Helm Test Hooks

Helm supports test hooks using:

```yaml
"helm.sh/hook": test
```

Example:

```yaml
apiVersion: batch/v1
kind: Job

metadata:
  name: {{ .Release.Name }}-test

  annotations:
    "helm.sh/hook": test

spec:
  template:
    spec:
      restartPolicy: Never

      containers:
        - name: test
          image: busybox:1.36
          command:
            - sh
            - -c
            - echo "Test passed"
```

Run the test:

```bash
helm test my-release
```

---

# Practical Database Migration Example

A common Helm hook use case is database migration.

```text
helm upgrade
      ↓
pre-upgrade
      ↓
Migration Job
      ↓
Migration succeeds
      ↓
Application upgrade
```

Example:

```yaml
apiVersion: batch/v1
kind: Job

metadata:
  name: {{ .Release.Name }}-db-migration

  annotations:
    "helm.sh/hook": pre-upgrade
    "helm.sh/hook-weight": "-5"
    "helm.sh/hook-delete-policy": before-hook-creation,hook-succeeded

spec:
  backoffLimit: 2

  template:
    spec:
      restartPolicy: Never

      containers:
        - name: migration
          image: myapp:1.2.0
          command:
            - /bin/sh
            - -c
            - ./migrate.sh
```

---

# Important: Hook Resources and Releases

Hook resources are treated differently from normal chart-managed resources.

A hook resource is **not managed like a normal release resource** after Helm executes the hook.

Therefore, cleanup policies are important.

For example:

```yaml
"helm.sh/hook-delete-policy": hook-succeeded
```

prevents successful hook Jobs from accumulating.

---

# Hook Execution and Failures

If a hook fails, the Helm operation can fail.

Example:

```text
helm upgrade
     ↓
pre-upgrade hook
     ↓
Migration Job
     ↓
❌ Failed
     ↓
Upgrade fails
```

This is why hook Jobs should:

- Have clear commands
- Return meaningful exit codes
- Have appropriate retry behavior
- Be tested before production use

---

# Debugging Hooks

Check the release:

```bash
helm status my-app
```

View history:

```bash
helm history my-app
```

List Jobs:

```bash
kubectl get jobs
```

Check Pods:

```bash
kubectl get pods
```

Inspect the Job:

```bash
kubectl describe job <job-name>
```

View logs:

```bash
kubectl logs job/<job-name>
```

Check events:

```bash
kubectl get events --sort-by=.lastTimestamp
```

---

# Preview Hook Templates

Render the chart before deployment:

```bash
helm template my-app ./my-chart
```

For debugging:

```bash
helm template my-app ./my-chart --debug
```

Dry-run an installation:

```bash
helm install my-app ./my-chart \
  --dry-run \
  --debug
```

---

# Hook vs Normal Resource

| Normal Resource | Helm Hook |
|---|---|
| Managed as part of the release | Runs at a specific lifecycle event |
| Created during normal release processing | Created according to hook timing |
| Continues to exist according to manifest | Often temporary |
| Normal Kubernetes resource lifecycle | Controlled by hook behavior |
| No hook annotation required | Requires `helm.sh/hook` annotation |

---

# Common Hook Use Cases

### Database Migration

```text
pre-upgrade
     ↓
Migration Job
```

### Backup

```text
pre-upgrade
     ↓
Backup Job
```

### Validation

```text
pre-install
     ↓
Validation Job
```

### Post-Deployment Verification

```text
post-install
     ↓
Smoke Test
```

### Cleanup

```text
pre-delete
     ↓
Cleanup Job
```

### Testing

```text
helm test
     ↓
test Hook
```

---

# Best Practices

### 1. Keep hooks lightweight

Hooks should perform focused tasks.

Avoid putting the entire application deployment logic inside hooks.

---

### 2. Use Jobs for one-time operations

Jobs are commonly used for:

- Migrations
- Validation
- Backups
- Tests

---

### 3. Always consider cleanup

Use:

```yaml
"helm.sh/hook-delete-policy": hook-succeeded
```

or:

```yaml
"helm.sh/hook-delete-policy": before-hook-creation
```

when appropriate.

---

### 4. Use hook weights when order matters

```yaml
"helm.sh/hook-weight": "-5"
```

---

### 5. Make hooks idempotent

A hook should ideally be safe to execute more than once.

For example, a migration should detect whether a migration has already been applied instead of blindly repeating destructive operations.

---

### 6. Test hooks before production

Use:

```bash
helm template
helm lint
helm install --dry-run --debug
```

and test in a non-production environment.

---

### 7. Monitor hook failures

A failed migration or validation hook can block a Helm operation.

Always inspect:

```bash
kubectl get jobs
kubectl describe job <job-name>
kubectl logs job/<job-name>
```

---

# Common Mistakes

## Forgetting Cleanup

Bad:

```yaml
annotations:
  "helm.sh/hook": pre-install
```

The Job may remain after execution.

Better:

```yaml
annotations:
  "helm.sh/hook": pre-install
  "helm.sh/hook-delete-policy": hook-succeeded
```

---

## Using Hooks for Everything

Do not use hooks when a normal Kubernetes resource is sufficient.

Hooks are intended for **lifecycle-specific actions**.

---

## Non-Idempotent Operations

A hook may run again during another lifecycle event.

Avoid operations that can corrupt or duplicate data when repeated.

---

## No Failure Handling

For Jobs, configure appropriate behavior:

```yaml
backoffLimit: 3
```

and make sure the command returns a non-zero exit code when the operation fails.

---

# Quick Revision

```text
Helm Hooks
    │
    ├── pre-install
    ├── post-install
    ├── pre-upgrade
    ├── post-upgrade
    ├── pre-rollback
    ├── post-rollback
    ├── pre-delete
    ├── post-delete
    └── test
```

### Important Annotations

```yaml
"helm.sh/hook": pre-install
```

```yaml
"helm.sh/hook-weight": "-5"
```

```yaml
"helm.sh/hook-delete-policy": hook-succeeded
```

### Important Commands

```bash
helm install
helm upgrade
helm rollback
helm uninstall
helm test
```

---

# Key Takeaways

- **Helm Hooks run Kubernetes resources at specific points in the Helm lifecycle.**
- Hooks are defined using the `helm.sh/hook` annotation.
- Common hooks include `pre-install`, `post-install`, `pre-upgrade`, `post-upgrade`, `pre-delete`, and `test`.
- `hook-weight` controls the execution order of hooks.
- `hook-delete-policy` controls when hook resources are removed.
- Jobs are commonly used for migrations, validation, backups, and tests.
- A failed hook can cause the Helm operation to fail.
- Hooks should be lightweight, idempotent, and easy to debug.
- Use hooks for **lifecycle-specific actions**, not as a replacement for normal Kubernetes resources.

> **Remember:**
>
> **Hook = Lifecycle Action**  
> **Weight = Execution Order**  
> **Delete Policy = Cleanup**  
> **Job = Common Hook Resource**  
> **Idempotent = Safe to Run Again**
