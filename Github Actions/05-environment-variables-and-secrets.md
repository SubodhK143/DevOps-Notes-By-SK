# GitHub Actions: Environment Variables & Secrets

Environment variables and secrets allow you to manage configuration values and sensitive information securely in GitHub Actions workflows. They help make workflows reusable, maintainable, and secure by avoiding hardcoded values.

Common use cases include:

- Application configuration
- API keys and access tokens
- Database credentials
- Deployment settings
- Environment-specific values

---

## Why Use Environment Variables and Secrets?

- Avoid hardcoding values in workflows.
- Improve workflow reusability.
- Secure sensitive information.
- Simplify environment-specific configurations.
- Follow CI/CD security best practices.

> **Note:** Use environment variables for non-sensitive data and secrets for sensitive information.

---

## 1. Environment Variables (`env`)

Environment variables are used to store non-sensitive configuration values.

GitHub Actions allows you to define environment variables at three levels:

- Workflow level
- Job level
- Step level

---

### Workflow-Level Environment Variables

Variables defined at the workflow level are available to all jobs and steps.

#### Example

```yaml
env:
  NODE_ENV: production
  APP_NAME: my-app

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Print Environment Variables
        run: echo "App: $APP_NAME, Env: $NODE_ENV"
```

#### Explanation

- Available throughout the entire workflow.
- Ideal for values used across multiple jobs.
- Reduces duplication.

---

### Job-Level Environment Variables

Variables defined at the job level are available only within that job.

#### Example

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    env:
      BUILD_MODE: release

    steps:
      - run: echo "Mode: $BUILD_MODE"
```

#### Explanation

- Accessible by all steps within the job.
- Useful when a variable is specific to one job.

---

### Step-Level Environment Variables

Variables defined at the step level are available only for that particular step.

#### Example

```yaml
steps:
  - name: Run with Custom Variable
    env:
      GREETING: Hello

    run: echo "$GREETING from GitHub Actions"
```

#### Explanation

- Limits the scope of the variable.
- Helps keep workflows clean and secure.
- Recommended when a variable is required only once.

---

## Environment Variable Scope

| Level | Scope |
|------|------|
| Workflow | Entire workflow |
| Job | Specific job |
| Step | Specific step |

---

## 2. Secrets

Secrets are used to store sensitive information securely.

Examples include:

- API keys
- Access tokens
- Database passwords
- Cloud credentials
- Deployment secrets

GitHub encrypts and securely stores secrets.

> **Important:** Never store passwords, API keys, or tokens directly inside workflow files.

---

### Adding Secrets

Navigate to:

```text
Repository
→ Settings
→ Secrets and variables
→ Actions
→ New repository secret
```

Add the following:

```text
Name: API_KEY
Value: your-secret-value
```

---

### Using Secrets in Workflows

#### Example

```yaml
steps:
  - name: Deploy Application
    env:
      API_KEY: ${{ secrets.API_KEY }}
      DB_PASSWORD: ${{ secrets.DB_PASSWORD }}

    run: ./deploy.sh
```

#### Explanation

- Secrets are securely injected into the workflow.
- GitHub automatically masks secret values in logs.
- Secrets should never be echoed or printed.

---

## 3. GitHub Built-in Environment Variables

GitHub automatically provides several useful environment variables in every workflow.

| Variable | Description |
|--------|--------|
| GITHUB_REPOSITORY | Repository name (owner/repository) |
| GITHUB_REF | Branch or tag reference |
| GITHUB_SHA | Commit SHA |
| GITHUB_ACTOR | Username that triggered the workflow |
| GITHUB_WORKSPACE | Path to the checked-out repository |

---

### Example

```yaml
- name: Print Built-in Variables
  run: |
    echo "Repository: $GITHUB_REPOSITORY"
    echo "Commit SHA: $GITHUB_SHA"
    echo "Actor: $GITHUB_ACTOR"
```

---

## Key Differences

| Feature | Environment Variables | Secrets |
|--------|--------|--------|
| Sensitive Data | No | Yes |
| Visible in Workflow | Yes | Masked |
| Best Use Case | Configuration values | Credentials and tokens |
| Storage | Workflow file | GitHub repository settings |

---

## Best Practices

- Never hardcode sensitive values.
- Always use `${{ secrets.NAME }}` for credentials.
- Use workflow-level variables when shared across jobs.
- Use job-level variables when limited to one job.
- Use step-level variables for temporary values.
- Keep secret names descriptive and consistent.
- Rotate sensitive credentials periodically.
- Avoid printing secret values in logs.

---

## Security Notes

- Secrets are automatically masked in workflow logs.
- Secrets are not exposed to workflows triggered by forked pull requests by default.
- Environment variables are case-sensitive on Linux runners.
- Limit variable scope whenever possible.

> **Tip:** Follow the principle of least privilege when managing secrets.

---

## Key Takeaways

- Use `env` for non-sensitive configuration values.
- Use `secrets` for sensitive information.
- Environment variables can be defined at workflow, job, or step levels.
- GitHub provides built-in variables for repository and workflow information.
- Never hardcode credentials in your workflows.
- Proper secret management is essential for secure CI/CD pipelines.

---

## Quick Summary

| Concept | Purpose |
|--------|--------|
| Workflow-Level Variables | Shared across all jobs |
| Job-Level Variables | Shared within one job |
| Step-Level Variables | Used for a single step |
| Secrets | Store sensitive information securely |
| Built-in Variables | Provide workflow metadata |
| Best Practice | Never hardcode credentials |
