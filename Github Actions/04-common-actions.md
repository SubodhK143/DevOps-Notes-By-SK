# GitHub Actions: Common Actions

GitHub Actions provides reusable components called **Actions** that simplify workflow automation. Instead of writing scripts from scratch, you can use pre-built actions to perform common tasks such as:

- Checking out source code
- Setting up programming environments
- Installing dependencies
- Building and pushing Docker images
- Running tests and deployments

Using reusable actions makes workflows easier to maintain, more reliable, and less error-prone.

---

## Why Use GitHub Actions?

- Reduces repetitive scripting.
- Improves workflow readability.
- Saves development time.
- Supports reusable and community-maintained integrations.
- Simplifies CI/CD pipeline automation.

> **Note:** Most workflows begin by checking out the repository before performing any other tasks.

---

## 1. Checkout Repository

The `actions/checkout` action clones your repository into the GitHub Actions runner.

### Example

```yaml
- name: Checkout Repository
  uses: actions/checkout@v4
```

### Explanation

- Downloads the latest version of your repository.
- Makes your project files available to the workflow.
- Usually the first step in most workflows.

> **Best Practice:** Always use `actions/checkout` before running build, test, or deployment steps.

---

## 2. Setup Node.js

The `actions/setup-node` action installs and configures a specific Node.js version.

### Example

```yaml
- name: Setup Node.js
  uses: actions/setup-node@v4
  with:
    node-version: '18'
```

### Explanation

- Installs Node.js version 18.
- Prepares the runner for Node.js applications.
- Useful for JavaScript and Node.js projects.

### Common Use Cases

- Installing npm packages
- Running tests
- Building applications
- Deploying Node.js projects

---

## 3. Setup Python

The `actions/setup-python` action installs and configures Python on the runner.

### Example

```yaml
- name: Setup Python
  uses: actions/setup-python@v5
  with:
    python-version: '3.11'
```

### Explanation

- Installs Python 3.11.
- Enables Python-based workflows.
- Supports package installation and test execution.

### Common Use Cases

- Installing Python dependencies
- Running unit tests
- Executing Python scripts
- Building Python applications

---

## 4. Docker Build & Push

The `docker/build-push-action` action builds Docker images and optionally pushes them to a container registry.

### Example

```yaml
- name: Build & Push Docker Image
  uses: docker/build-push-action@v6
  with:
    context: .
    push: true
    tags: user/app:latest
```

### Explanation

| Parameter | Description |
|----------|----------|
| context | Specifies the build context (current directory). |
| push | Pushes the image to a container registry. |
| tags | Defines the image name and tag. |

### Common Use Cases

- Building Docker images
- Pushing images to Docker Hub
- Publishing images to container registries
- Automating container deployments

> **Note:** Docker image push operations typically require authentication using GitHub Secrets.

---

## Commonly Used Actions

| Action | Purpose |
|-------|-------|
| actions/checkout | Clone the repository |
| actions/setup-node | Install Node.js |
| actions/setup-python | Install Python |
| docker/build-push-action | Build and push Docker images |

---

## Key Takeaways

- Actions are reusable components used in GitHub Actions workflows.
- They help automate common CI/CD tasks.
- `actions/checkout` is usually the first workflow step.
- Setup actions configure programming environments.
- Docker actions simplify container image management.
- Using official actions improves reliability and maintainability.

---

## Quick Summary

| Action | Usage |
|------|------|
| Checkout Repository | Access project source code |
| Setup Node.js | Configure Node.js environment |
| Setup Python | Configure Python environment |
| Docker Build & Push | Build and publish Docker images |
