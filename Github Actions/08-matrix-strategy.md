# GitHub Actions: Matrix Strategy

A Matrix Strategy allows GitHub Actions to run the same job multiple times using different configurations in parallel.

Common use cases include:

- Testing applications across multiple operating systems.
- Testing multiple programming language versions.
- Running workflows across different environments.
- Performing compatibility and regression testing.
- Reducing duplicate workflow definitions.

---

## Why Use Matrix Strategy?

Matrix strategies help you:

- Reduce repetitive workflow code.
- Execute jobs in parallel.
- Improve CI/CD pipeline scalability.
- Validate applications across multiple environments.
- Simplify multi-version and cross-platform testing.

> **Note:** Matrix strategies are widely used in modern CI/CD pipelines for automated compatibility testing.

---

## 1. Basic Matrix Strategy

A basic matrix uses a single variable to generate multiple jobs automatically.

### 1.1 How It Works

- Define one or more values for a variable.
- GitHub automatically creates a separate job for each value.
- All generated jobs run independently.

#### Example

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [16, 18, 20]

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}

      - name: Run Tests
        run: npm test
```

#### Generated Jobs

```text
Test Job
│
├── Node.js 16
├── Node.js 18
└── Node.js 20
```

#### Explanation

- Three jobs are created automatically.
- Each job uses a different Node.js version.
- Jobs can run in parallel.

---

### 1.2 Accessing Matrix Variables

Matrix values are accessed using:

```yaml
${{ matrix.variable_name }}
```

#### Example

```yaml
${{ matrix.node-version }}
```

#### Explanation

- GitHub injects the appropriate value during workflow execution.
- Matrix variables can be used throughout the workflow.

---

## 2. Multi-Dimensional Matrix

Multi-dimensional matrices allow you to combine multiple variables.

### 2.1 How It Works

- Define multiple variables.
- GitHub creates every possible combination automatically.

#### Example

```yaml
jobs:
  test:
    strategy:
      matrix:
        os:
          - ubuntu-latest
          - windows-latest

        node-version:
          - 16
          - 18
          - 20

    runs-on: ${{ matrix.os }}

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}

      - name: Run Tests
        run: npm test
```

#### Generated Jobs

```text
OS                  Node.js Version
-----------------------------------
ubuntu-latest       16
ubuntu-latest       18
ubuntu-latest       20
windows-latest      16
windows-latest      18
windows-latest      20
```

#### Result

```text
2 Operating Systems × 3 Versions = 6 Jobs
```

---

## 3. Excluding Specific Combinations

Sometimes, certain matrix combinations are unnecessary or unsupported.

### 3.1 Using exclude

Use the `exclude` keyword to skip specific combinations.

#### Example

```yaml
strategy:
  matrix:
    os:
      - ubuntu-latest
      - windows-latest
      - macos-latest

    node-version:
      - 16
      - 18
      - 20

    exclude:
      - os: windows-latest
        node-version: 16
```

#### Result

```text
Skipped Combination:

windows-latest + Node.js 16
```

#### Common Use Cases

- Unsupported software versions.
- Platform-specific limitations.
- Reducing unnecessary workflow runs.

---

## 4. Including Extra Combinations

The `include` keyword allows you to add custom configurations to your matrix.

### 4.1 Using include

#### Example

```yaml
strategy:
  matrix:
    os:
      - ubuntu-latest
      - windows-latest

    node-version:
      - 18
      - 20

    include:
      - os: ubuntu-latest
        node-version: 20
        experimental: true
```

#### Result

```text
Ubuntu + Node.js 20

Additional Variable:
experimental = true
```

#### Common Use Cases

- Experimental builds.
- Beta releases.
- Feature testing.
- Environment-specific configurations.

---

## 5. Fail Fast Strategy

By default, GitHub Actions uses:

```yaml
fail-fast: true
```

### 5.1 How It Works

```text
One Job Fails
      |
      v
Cancel Remaining Jobs
```

### 5.2 Disable Fail Fast

#### Example

```yaml
strategy:
  fail-fast: false
```

#### Benefits

- Allows all matrix jobs to complete.
- Provides complete test results.
- Improves compatibility testing.

> **Tip:** Use `fail-fast: false` when testing across multiple environments or versions.

---

## 6. Limiting Parallel Jobs

GitHub Actions can run multiple matrix jobs simultaneously.

### 6.1 Using max-parallel

#### Example

```yaml
strategy:
  max-parallel: 2

  matrix:
    node-version:
      - 16
      - 18
      - 20
```

#### Workflow Execution Flow

```text
Node.js 16 ---> Running
Node.js 18 ---> Running

Wait...

Node.js 20 ---> Starts after one job completes
```

#### Benefits

- Controls resource consumption.
- Helps manage concurrent job limits.
- Optimizes large workflows.

---

## 7. Common Use Cases

Matrix strategies are commonly used for:

- Cross-platform testing.
- Multi-version testing.
- Multi-environment deployments.
- Browser compatibility testing.
- Framework version testing.
- CI/CD automation pipelines.

#### Example

```text
Linux + Python 3.11
Linux + Python 3.12

Windows + Python 3.11
Windows + Python 3.12

macOS + Python 3.11
macOS + Python 3.12
```

---

## 8. Best Practices

- Use matrix strategies for repetitive jobs.
- Keep matrix definitions simple and readable.
- Use `exclude` for unsupported combinations.
- Use `include` for custom configurations.
- Use `fail-fast: false` when full test coverage is required.
- Use `max-parallel` to control concurrent job execution.
- Avoid unnecessarily large matrices.

---

## 9. Key Rules

- Matrix values are accessed using `${{ matrix.VARIABLE_NAME }}`.
- Every matrix combination runs as an independent job.
- Each job receives its own runner.
- Matrix jobs count toward GitHub Actions concurrent job limits.
- Use `max-parallel` to control resource usage.
- Use `exclude` and `include` to customize matrix behavior.

---

## 10. Key Takeaways

- Matrix strategies simplify CI/CD workflows.
- They eliminate duplicate job definitions.
- GitHub automatically generates matrix combinations.
- Multi-dimensional matrices support cross-platform testing.
- Proper use of matrix strategies improves scalability and maintainability.

---

## 11. Quick Summary

| Concept | Purpose |
|--------|--------|
| Matrix Strategy | Run multiple job configurations automatically |
| Basic Matrix | Test a single variable |
| Multi-Dimensional Matrix | Test multiple variables together |
| exclude | Skip specific combinations |
| include | Add custom configurations |
| fail-fast | Cancel remaining jobs on failure |
| max-parallel | Limit concurrent job execution |
| matrix.variable | Access matrix values |
| Best Practice | Use matrices for scalable CI/CD pipelines |
