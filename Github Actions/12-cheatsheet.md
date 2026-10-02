# GitHub Actions Cheatsheet

A quick reference guide for commonly used GitHub Actions syntax and workflow configurations.

> **Note:** This cheatsheet is intended for quick revision and daily use. Refer to the individual markdown files for detailed explanations and examples.

---

## 1. Basic Workflow Structure

```yaml
name: My Workflow

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
      - run: echo "Hello, GitHub Actions!"
```

---

## 2. Workflow Triggers

```yaml
on:
  push:
    branches:
      - main

  pull_request:
    branches:
      - main

  workflow_dispatch:

  schedule:
    - cron: "0 0 * * *"
```

| Trigger | Description |
|--------|--------|
| push | Trigger on code push |
| pull_request | Trigger on pull requests |
| workflow_dispatch | Manual trigger |
| schedule | Scheduled workflow execution |

---

## 3. Jobs and Dependencies

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - run: npm test

  deploy:
    needs: test

    runs-on: ubuntu-latest

    steps:
      - run: ./deploy.sh
```

---

## 4. Common Actions

```yaml
# Checkout Repository
- uses: actions/checkout@v4

# Setup Node.js
- uses: actions/setup-node@v4
  with:
    node-version: '20'

# Setup Python
- uses: actions/setup-python@v5
  with:
    python-version: '3.13'

# Build Docker Image
- uses: docker/build-push-action@v6

# Cache Dependencies
- uses: actions/cache@v4

# Upload Artifact
- uses: actions/upload-artifact@v4

# Download Artifact
- uses: actions/download-artifact@v4
```

---

## 5. Environment Variables

### Workflow Level

```yaml
env:
  APP_NAME: my-app
```

### Job Level

```yaml
jobs:
  build:
    env:
      BUILD_MODE: release
```

### Step Level

```yaml
steps:
  - env:
      GREETING: Hello

    run: echo "$GREETING"
```

---

## 6. GitHub Secrets

```yaml
env:
  API_TOKEN: ${{ secrets.API_TOKEN }}
```

### Common Examples

```yaml
${{ secrets.API_TOKEN }}

${{ secrets.AWS_ACCESS_KEY_ID }}

${{ secrets.DB_PASSWORD }}
```

---

## 7. Expressions and Contexts

### GitHub Context

```yaml
${{ github.ref_name }}
${{ github.sha }}
${{ github.actor }}
${{ github.repository }}
${{ github.event_name }}
```

### Runner Context

```yaml
${{ runner.os }}
${{ runner.arch }}
```

### Environment Variables

```yaml
${{ env.APP_NAME }}
```

### Secrets

```yaml
${{ secrets.API_TOKEN }}
```

### Step Outputs

```yaml
${{ steps.build.outputs.image_tag }}
```

---

## 8. Conditional Expressions

```yaml
if: github.ref_name == 'main'

if: github.event_name == 'push'

if: success()

if: failure()

if: always()
```

---

## 9. Artifacts

### Upload Artifact

```yaml
- uses: actions/upload-artifact@v4
  with:
    name: build-output
    path: dist/
```

### Download Artifact

```yaml
- uses: actions/download-artifact@v4
  with:
    name: build-output
    path: dist/
```

---

## 10. Caching

```yaml
- uses: actions/cache@v4
  with:
    path: node_modules

    key: node-${{ runner.os }}-${{ hashFiles('package-lock.json') }}

    restore-keys: |
      node-${{ runner.os }}-
```

---

## 11. Matrix Strategy

### Basic Matrix

```yaml
strategy:
  matrix:
    node-version:
      - 18
      - 20
```

### Multi-Dimensional Matrix

```yaml
strategy:
  matrix:
    os:
      - ubuntu-latest
      - windows-latest

    node-version:
      - 18
      - 20
```

### Access Matrix Values

```yaml
${{ matrix.os }}

${{ matrix.node-version }}
```

---

## 12. Reusable Workflows

### Create Reusable Workflow

```yaml
on:
  workflow_call:
```

### Pass Inputs

```yaml
with:
  environment: production
```

### Pass Secrets

```yaml
secrets:
  API_TOKEN: ${{ secrets.API_TOKEN }}
```

### Call Reusable Workflow

```yaml
jobs:
  deploy:
    uses: username/repository/.github/workflows/deploy.yml@main
```

---

## 13. Self-Hosted Runners

### Basic Usage

```yaml
runs-on: self-hosted
```

### Using Labels

```yaml
runs-on:
  - self-hosted
  - linux
  - x64
```

---

## 14. Step Outputs

### Set Output

```yaml
- name: Set Output
  id: build

  run: echo "image_tag=v1.0.0" >> $GITHUB_OUTPUT
```

### Use Output

```yaml
- name: Use Output

  run: echo "${{ steps.build.outputs.image_tag }}"
```

---

## 15. Built-in Functions

| Function | Purpose |
|--------|--------|
| success() | Previous steps succeeded |
| failure() | Previous steps failed |
| always() | Always execute |
| contains() | Check if a string contains a value |
| startsWith() | Check string prefix |
| hashFiles() | Generate cache keys |
| toJSON() | Debug contexts |

---

## 16. Common CI/CD Pipeline Flow

```text
Push Code
    |
    v
Checkout Repository
    |
    v
Setup Environment
    |
    v
Install Dependencies
    |
    v
Run Tests
    |
    v
Build Application
    |
    v
Cache Dependencies
    |
    v
Upload Artifacts
    |
    v
Build Docker Image
    |
    v
Deploy Application
    |
    v
Pipeline Complete
```

---

## 17. Quick Tips

- Use GitHub Secrets for sensitive information.
- Use caching to improve workflow performance.
- Use matrix strategies for multi-version testing.
- Use reusable workflows to avoid duplication.
- Use artifacts to preserve build outputs.
- Use self-hosted runners for custom environments.
- Use `needs` for job dependencies.
- Use `workflow_dispatch` for manual execution.
- Use `fail-fast: false` for complete test coverage.

---

## 18. Quick Summary

| Concept | Purpose |
|--------|--------|
| Triggers | Start workflow execution |
| Jobs | Execute workflow tasks |
| Steps | Perform individual actions |
| Secrets | Store sensitive information |
| Cache | Improve workflow speed |
| Artifacts | Store workflow outputs |
| Matrix Strategy | Run multiple job configurations |
| Reusable Workflows | Reuse CI/CD logic |
| Self-Hosted Runners | Execute workflows on custom infrastructure |
| CI/CD Pipeline | Automate software delivery |
