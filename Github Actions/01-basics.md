# GitHub Actions - Basics

## What is GitHub Actions?

GitHub Actions is GitHub's built-in CI/CD platform that automates workflows for building, testing, and deploying applications.

---

## Key Concepts

| Concept | Description |
|--------|--------|
| Workflow | YAML file inside `.github/workflows/` that defines automation. |
| Job | A unit of work that runs on a runner. |
| Step | A single command or action inside a job. |
| Action | Reusable functionality shared between workflows. |
| Runner | The machine or environment that executes jobs. |

---

## Workflow Anatomy

- **on**
  - Defines when the workflow is triggered.
  - Examples: `push`, `pull_request`, and `workflow_dispatch`.

- **jobs**
  - Defines one or more jobs that the workflow will execute.

- **runs-on**
  - Specifies the runner environment.
  - Examples: `ubuntu-latest`, `windows-latest`, and `macos-latest`.

- **steps**
  - Defines the sequence of actions executed within a job.

- **uses**
  - Uses a pre-built GitHub Action.
  - Example: `actions/checkout@v4`.

- **run**
  - Executes shell commands directly on the runner.

- **name**
  - Provides a human-readable name for workflows, jobs, or steps.

---

## Basic Workflow Example

```yaml
name: Hello Workflow

on:
  push:
    branches:
      - main

jobs:
  greet:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Print Greeting
        run: echo "Hello from GitHub Actions!"

      - name: Print Current Date and Time
        run: date

      - name: Print Branch Name
        run: |
          echo "Triggered by branch: ${{ github.ref_name }}"

      - name: List Repository Files
        run: ls -la

      - name: Print Runner OS
        run: |
          echo "Runner OS: ${{ runner.os }}"
```

---

## Workflow Execution Flow

```text
Push Code
    ↓
Trigger Workflow
    ↓
Create Runner
    ↓
Execute Job(s)
    ↓
Run Step(s)
    ↓
Workflow Completed
```

---

## Real-World Example

**Repository:** Kubernetes Minikube

**Workflow File:**

- https://github.com/kubernetes/minikube/blob/master/.github/workflows/build.yml

### Workflow Overview

- Triggered manually or on push events for specific files and directories.
- Contains 3 jobs.

### Jobs

#### First Job: build_minikube

- Uses an Ubuntu 22.04 runner.
- Checks out the source code.
- Installs dependencies.
- Builds Minikube binaries.
- Uploads build artifacts.

#### Second Job: lint

- Performs code linting and formatting checks.
- Detects common issues before execution.

#### Third Job: unit_test

- Runs automated unit tests.
- Verifies that individual components behave as expected.

---

## Quick Summary

- GitHub Actions automates CI/CD workflows.
- Workflows consist of jobs, steps, actions, and runners.
- Workflows are triggered by GitHub events.
- GitHub Actions supports both simple and production-grade automation pipelines.
- Exploring open-source workflows is a great way to understand real-world CI/CD practices.
