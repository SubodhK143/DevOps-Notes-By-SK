# GitHub Actions: Jobs, Steps, and Dependencies

A GitHub Actions workflow is composed of **jobs**, and each job contains one or more **steps**. Understanding this hierarchy is essential for building reliable and scalable CI/CD pipelines.

### Workflow Hierarchy

```
Workflow
│
├── Job 1 (Build)
│   ├── Step 1
│   ├── Step 2
│   └── Step 3
│
└── Job 2 (Deploy)
    ├── Step 1
    └── Step 2
```

| Component | Description |
|----------|----------|
| Workflow | Defines when the pipeline runs and what jobs it executes. |
| Job | A unit of work that runs on a runner (virtual machine). |
| Step | An individual task executed inside a job. |

---

## 1. Jobs

A job is a collection of steps that runs on a specific runner. Multiple jobs can run in parallel unless dependencies are explicitly defined.

### Example

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Hello from GitHub Actions"
```

### Explanation

- `jobs` defines all jobs in the workflow.
- `build` is the job name.
- `runs-on` specifies the runner environment.
- `steps` contains the tasks executed by the job.

> **Note:** Each job runs in an isolated environment.

---

## 2. Steps

Steps are executed sequentially within a job. A step can either:

- Run shell commands using `run`
- Execute reusable actions using `uses`

### Example

```yaml
steps:
  - name: Checkout Repository
    uses: actions/checkout@v4

  - name: Install Dependencies
    run: npm install

  - name: Run Tests
    run: npm test
```

### Explanation

| Keyword | Purpose |
|--------|--------|
| name | Provides a readable name for the step |
| uses | Executes a reusable GitHub Action |
| run | Runs shell commands directly |

### Common Step Types

```yaml
# Run shell commands
- run: echo "Hello World"

# Execute an action
- uses: actions/checkout@v4
```

> **Tip:** Use meaningful step names to improve workflow readability and debugging.

---

## 3. Job Dependencies

By default, GitHub Actions runs jobs in parallel.

Use the `needs` keyword when one job must wait for another job to complete successfully.

### Example

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Running tests"

  deploy:
    needs: test
    runs-on: ubuntu-latest

    steps:
      - run: echo "Deploying application"
```

### Execution Flow

```
test
  │
  ▼
deploy
```

### Explanation

- The `test` job executes first.
- The `deploy` job waits for the `test` job to complete successfully.
- If the `test` job fails, the `deploy` job is skipped.

> **Important:** Use `needs` to control the execution order of jobs in your CI/CD pipelines.

---

## Key Takeaways

- A workflow consists of one or more jobs.
- Each job contains one or more steps.
- Steps run sequentially within a job.
- Jobs run in parallel by default.
- Use `needs` to create job dependencies.
- Well-defined job dependencies improve pipeline reliability and maintainability.

---

## Quick Summary

| Concept | Purpose |
|--------|--------|
| Workflow | Defines triggers and jobs |
| Job | Executes a set of steps on a runner |
| Step | Performs individual tasks |
| run | Executes shell commands |
| uses | Runs reusable GitHub Actions |
| needs | Creates dependencies between jobs |
