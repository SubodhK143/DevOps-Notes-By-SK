# GitHub Actions: Reusable Workflows

Reusable workflows allow you to define a workflow once and reuse it across multiple repositories or workflows.

Instead of duplicating the same CI/CD logic, reusable workflows help you:

- Reduce duplicate workflow code.
- Improve maintainability.
- Standardize CI/CD pipelines.
- Promote workflow reusability.
- Simplify workflow management across projects.

---

## Why Use Reusable Workflows?

Reusable workflows help you:

- Follow the DRY (Don't Repeat Yourself) principle.
- Maintain consistent CI/CD pipelines.
- Centralize workflow management.
- Reduce configuration errors.
- Improve scalability across repositories and teams.

> **Note:** Reusable workflows are ideal for organizations or projects that use similar CI/CD pipelines across multiple repositories.

---

## 1. What are Reusable Workflows?

Reusable workflows are GitHub Actions workflows that can be called by other workflows using the `workflow_call` event.

### 1.1 How It Works

- Create a reusable workflow.
- Define its inputs, secrets, and outputs.
- Call it from another workflow.
- GitHub executes the reusable workflow automatically.

#### Workflow Execution Flow

```text
Caller Workflow
       |
       v
Calls Reusable Workflow
       |
       v
Passes Inputs & Secrets
       |
       v
Reusable Workflow Executes
       |
       v
Returns Outputs (Optional)
```

#### Benefits

- Less duplication.
- Easier maintenance.
- Cleaner repository structure.
- Better workflow organization.

---

## 2. Creating a Reusable Workflow

Reusable workflows use the `workflow_call` trigger.

### 2.1 Basic Syntax

#### Example

```yaml
name: Reusable Workflow

on:
  workflow_call:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Print Message
        run: echo "This is a reusable workflow."
```

#### Explanation

- `workflow_call` allows other workflows to invoke this workflow.
- The workflow behaves like a reusable CI/CD component.
- It can be stored inside the `.github/workflows` directory.

> **Note:** Reusable workflows must reside inside `.github/workflows`.

---

## 3. Passing Inputs

Inputs allow reusable workflows to receive values from the caller workflow.

### 3.1 Defining Inputs

#### Example

```yaml
on:
  workflow_call:
    inputs:
      environment:
        required: true
        type: string
```

#### Explanation

- Inputs are similar to function parameters.
- They make workflows more flexible and reusable.
- Supported types include:
  - string
  - boolean
  - number

---

### 3.2 Using Inputs

#### Example

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Print Environment
        run: echo "Deploying to ${{ inputs.environment }}"
```

#### Explanation

- Inputs are accessed using:

```yaml
${{ inputs.INPUT_NAME }}
```

#### Example

```yaml
${{ inputs.environment }}
```

---

## 4. Passing Secrets

Secrets allow reusable workflows to securely receive sensitive values.

### 4.1 Defining Secrets

#### Example

```yaml
on:
  workflow_call:
    secrets:
      API_TOKEN:
        required: true
```

#### Explanation

- Secrets are securely passed from the caller workflow.
- GitHub automatically masks secret values in logs.

---

### 4.2 Using Secrets

#### Example

```yaml
steps:
  - name: Use API Token
    run: echo "Secret received successfully."
    env:
      API_TOKEN: ${{ secrets.API_TOKEN }}
```

#### Common Use Cases

- Cloud credentials.
- API keys.
- Deployment tokens.
- Docker registry credentials.
- Database passwords.

---

## 5. Calling a Reusable Workflow

Once created, reusable workflows can be invoked from another workflow.

### 5.1 Basic Usage

#### Example

```yaml
jobs:
  deploy:
    uses: username/repository/.github/workflows/deploy.yml@main
```

#### Explanation

- The reusable workflow is called using the `uses` keyword.
- GitHub fetches and executes the workflow.

---

### 5.2 Passing Inputs and Secrets

#### Example

```yaml
jobs:
  deploy:
    uses: username/repository/.github/workflows/deploy.yml@main

    with:
      environment: production

    secrets:
      API_TOKEN: ${{ secrets.API_TOKEN }}
```

#### Explanation

- `with` passes input values.
- `secrets` passes sensitive information.
- Inputs and secrets are optional unless marked as required.

---

## 6. Returning Outputs

Reusable workflows can expose outputs that are consumed by caller workflows.

### 6.1 Defining Outputs

#### Example

```yaml
on:
  workflow_call:
    outputs:
      image-tag:
        value: ${{ jobs.build.outputs.image-tag }}
```

#### Explanation

- Outputs allow workflows to share data.
- Useful for deployment pipelines.

---

### 6.2 Common Use Cases

Outputs are commonly used for:

- Docker image tags.
- Deployment URLs.
- Artifact names.
- Version numbers.
- Build information.

---

## 7. Reusable Workflow Example

### Example

```yaml
name: Reusable Deployment Workflow

on:
  workflow_call:
    inputs:
      environment:
        required: true
        type: string

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Deploy Application
        run: echo "Deploying to ${{ inputs.environment }}"
```

### Caller Workflow

```yaml
jobs:
  deploy:
    uses: username/repository/.github/workflows/deploy.yml@main

    with:
      environment: production
```

#### Workflow Execution Flow

```text
Caller Workflow
       |
       v
Pass Environment Variable
       |
       v
Reusable Workflow
       |
       v
Execute Deployment Job
       |
       v
Deployment Complete
```

---

## 8. Common Use Cases

Reusable workflows are commonly used for:

- CI pipelines.
- CD pipelines.
- Docker builds.
- Terraform deployments.
- Security scans.
- Kubernetes deployments.
- Multi-repository automation.
- Organization-wide standards.

---

## 9. Best Practices

- Use reusable workflows for common CI/CD processes.
- Keep workflows modular and simple.
- Use descriptive input names.
- Store secrets securely.
- Use outputs only when necessary.
- Avoid duplicating workflow logic.
- Maintain version-controlled reusable workflows.

> **Tip:** Treat reusable workflows like reusable functions in programming.

---

## 10. Key Rules

- Reusable workflows use the `workflow_call` event.
- They must be stored inside `.github/workflows`.
- Inputs are passed using `with`.
- Secrets are passed using `secrets`.
- Outputs can be returned to caller workflows.
- Reusable workflows can be shared across repositories.

---

## 11. Key Takeaways

- Reusable workflows improve CI/CD maintainability.
- They reduce duplicate workflow definitions.
- Inputs and secrets make workflows flexible and secure.
- Outputs allow workflows to share information.
- They are widely used in scalable and production-ready CI/CD pipelines.

---

## 12. Quick Summary

| Concept | Purpose |
|--------|--------|
| Reusable Workflow | Reuse CI/CD logic across workflows |
| workflow_call | Enables reusable workflows |
| Inputs | Pass configuration values |
| Secrets | Pass sensitive information securely |
| Outputs | Return values to caller workflows |
| uses | Invoke reusable workflows |
| Best Practice | Keep workflows modular and reusable |
