# GitHub Actions - Workflow Triggers

## What is a Trigger?

A trigger defines when and why a GitHub Actions workflow runs. It listens for specific events in your repository and starts the workflow automatically or manually.

---

## 1. push

- Runs when code is pushed to a branch or tag.
- Commonly used for Continuous Integration (CI) pipelines.

**Use Case:** Automatically build and test code after every push.

### Example

```yaml
on:
  push:
    branches:
      - main
```

Trigger on tags:

```yaml
on:
  push:
    tags:
      - 'v*.*.*'
```

---

## 2. pull_request

- Runs when a pull request targets specific branches.
- Helps validate changes before merging.

**Use Case:** Run automated tests before merging code into the main branch.

### Example

```yaml
on:
  pull_request:
    branches:
      - main
```

---

## 3. schedule

- Runs workflows automatically on a schedule using cron syntax.

**Use Case:** Nightly builds, periodic tests, and maintenance tasks.

### Example

```yaml
on:
  schedule:
    - cron: "0 0 * * *"
```

> The above cron expression runs the workflow every day at midnight (UTC).

---

## 4. workflow_dispatch

- Allows workflows to be triggered manually from the GitHub Actions UI.
- Supports custom inputs for flexible workflows.

**Use Case:** Manually trigger deployments or maintenance workflows.

### Example

```yaml
on:
  workflow_dispatch:
    inputs:
      env:
        description: "Deployment environment"
        required: true
        default: "staging"
```

---

## Quick Summary

| Trigger | Purpose |
|--------|--------|
| `push` | Runs workflows when code is pushed. |
| `pull_request` | Validates changes before merging. |
| `schedule` | Runs workflows automatically at scheduled times. |
| `workflow_dispatch` | Triggers workflows manually from the GitHub UI. |
