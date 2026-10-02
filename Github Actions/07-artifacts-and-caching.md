# GitHub Actions: Artifacts & Caching

Artifacts and caching are essential features in GitHub Actions that improve workflow efficiency and reliability.

- **Artifacts** help preserve files generated during a workflow run.
- **Caching** reduces workflow execution time by reusing dependencies between runs.

Both are widely used in modern CI/CD pipelines to optimize performance and simplify build and deployment processes.

---

## Why Use Artifacts and Caching?

- Reduce workflow execution time.
- Share files between jobs.
- Preserve build outputs and reports.
- Avoid repeatedly downloading dependencies.
- Improve CI/CD pipeline performance and reliability.

> **Note:** Artifacts and caching solve different problems and are often used together in production workflows.

---

## 1. Artifacts

Artifacts are files or directories produced during a workflow run that you want to preserve or share.

### 1.1 Common Use Cases

Artifacts are commonly used for:

- Build outputs
- Compiled binaries
- Test reports
- Application logs
- Deployment packages
- Code coverage reports

GitHub provides the following actions for artifact management:

```yaml
actions/upload-artifact
actions/download-artifact
```

> **Note:** Artifacts are stored for 90 days by default. You can configure their retention period.

---

### 1.2 Uploading an Artifact

Use the `actions/upload-artifact` action to save files generated during a workflow.

#### Example

```yaml
steps:
  - name: Build Project
    run: npm run build

  - name: Upload Build Output
    uses: actions/upload-artifact@v4
    with:
      name: build-output
      path: dist/
      retention-days: 7
```

#### Parameters

| Parameter | Description |
|----------|----------|
| name | Name of the artifact |
| path | File or directory to upload |
| retention-days | Number of days to retain the artifact |

#### Explanation

- `name` uniquely identifies the artifact.
- `path` specifies the files or folders to upload.
- `retention-days` controls how long the artifact is stored.

---

### 1.3 Downloading an Artifact

Artifacts can be downloaded by other jobs within the same workflow.

#### Example

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - run: npm run build

      - uses: actions/upload-artifact@v4
        with:
          name: build-output
          path: dist/

  deploy:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: Download Build Output
        uses: actions/download-artifact@v4
        with:
          name: build-output
          path: dist/

      - name: Deploy Application
        run: ./deploy.sh
```

#### Workflow Execution Flow

```text
Build Job
   |
   v
Generate Files
   |
   v
Upload Artifact
   |
   v
Deploy Job
   |
   v
Download Artifact
   |
   v
Deploy Application
```

> **Important:** The artifact name used during download must exactly match the upload name.

---

## 2. Caching

Caching stores dependencies between workflow runs to improve performance.

Instead of downloading dependencies every time a workflow executes, GitHub restores them from a previously saved cache whenever possible.

### 2.1 Common Use Cases

Caching is commonly used for:

- Node.js packages
- Python packages
- Maven dependencies
- Gradle dependencies
- Terraform plugin cache
- Package manager cache directories

### Benefits of Caching

- Faster builds
- Reduced network usage
- Improved CI/CD performance
- Reduced installation time

---

### 2.2 Cache Key Strategy

A cache key uniquely identifies cached data.

A good cache key should change whenever your dependencies change.

#### Example

```yaml
key: node-${{ runner.os }}-${{ hashFiles('package-lock.json') }}
```

#### Explanation

- `runner.os` creates OS-specific caches.
- `hashFiles()` automatically changes the cache key whenever dependency files are modified.
- GitHub creates a new cache when the key changes.

---

### 2.3 Caching Node.js Dependencies

#### Example

```yaml
steps:
  - uses: actions/checkout@v4

  - name: Cache Node Modules
    uses: actions/cache@v4
    with:
      path: node_modules
      key: node-${{ runner.os }}-${{ hashFiles('package-lock.json') }}
      restore-keys: |
        node-${{ runner.os }}-

  - name: Install Dependencies
    run: npm install

  - name: Run Tests
    run: npm test
```

#### Parameters

| Parameter | Purpose |
|----------|----------|
| path | Directory to cache |
| key | Unique cache identifier |
| restore-keys | Fallback cache keys |

---

### 2.4 Caching Python Dependencies

#### Example

```yaml
steps:
  - uses: actions/checkout@v4

  - name: Cache Pip Packages
    uses: actions/cache@v4
    with:
      path: ~/.cache/pip
      key: pip-${{ runner.os }}-${{ hashFiles('requirements.txt') }}
      restore-keys: |
        pip-${{ runner.os }}-

  - name: Install Dependencies
    run: pip install -r requirements.txt
```

---

### 2.5 Cache Workflow

```text
Workflow Starts
      |
      v
Check Cache Key
      |
      v
Cache Found?
   /       \
 Yes        No
 |           |
Restore     Install Dependencies
 Cache         |
 |             |
 |         Create Cache
  \           /
   \         /
     Continue Workflow
```

---

## 3. Artifacts vs Cache

| Feature | Artifacts | Cache |
|--------|--------|--------|
| Purpose | Store workflow-generated files | Speed up workflow execution |
| Shared Between | Jobs and workflow runs | Workflow runs |
| Default Lifetime | 90 days | 7 days since last access |
| Best For | Build outputs and reports | Dependencies |
| Examples | dist/, logs, reports | node_modules/, pip cache |

---

## 4. When to Use Artifacts

Use artifacts when you want to:

- Preserve build outputs.
- Store test reports.
- Save application logs.
- Share files between jobs.
- Create deployment packages.

### Examples

```text
dist/
build/
coverage/
reports/
logs/
```

---

## 5. When to Use Cache

Use cache when you want to:

- Reduce build time.
- Reuse dependencies.
- Improve workflow performance.
- Avoid repeated package downloads.

### Examples

```text
node_modules/
~/.cache/pip
.gradle/
.maven/
terraform plugin cache
```

---

## 6. Best Practices

### Artifacts

- Use descriptive artifact names.
- Configure appropriate retention periods.
- Upload only necessary files.
- Remove unnecessary large artifacts.

### Cache

- Use `hashFiles()` for cache invalidation.
- Include the operating system in cache keys.
- Use `restore-keys` for fallback matching.
- Cache only reusable dependencies.

---

## 7. Key Rules

- Artifact names should be unique within a workflow run.
- Cache keys are immutable.
- Use `hashFiles()` for automatic cache invalidation.
- Cache is not automatically shared across branches.
- Artifact names must match during upload and download.
- Use caching for dependencies, not build outputs.

---

## 8. Key Takeaways

- Artifacts preserve workflow-generated files.
- Caching significantly improves workflow performance.
- Artifacts are ideal for reports and build outputs.
- Cache is ideal for reusable dependencies.
- Proper cache key design is essential for efficient CI/CD pipelines.
- Using both features together improves workflow reliability and speed.

---

## 9. Quick Summary

| Concept | Purpose |
|--------|--------|
| Artifacts | Store workflow-generated files |
| Upload Artifact | Save files after workflow execution |
| Download Artifact | Access files in other jobs |
| Cache | Reuse dependencies between runs |
| Cache Key | Identify cached data |
| restore-keys | Fallback cache matching |
| hashFiles() | Automatically invalidate cache |
| Best Practice | Use artifacts for outputs and cache for dependencies |
