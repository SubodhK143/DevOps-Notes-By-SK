# GitHub Actions: Expressions & Contexts

GitHub Actions expressions and contexts allow you to build dynamic and intelligent workflows. They enable workflows to access runtime information, evaluate conditions, and pass data between jobs and steps.

Common use cases include:

- Accessing repository and workflow information
- Running conditional steps or jobs
- Reading environment variables and secrets
- Passing outputs between steps
- Creating reusable and dynamic CI/CD pipelines

---

## Why Use Expressions and Contexts?

- Make workflows more flexible and dynamic.
- Enable conditional execution of jobs and steps.
- Access metadata about the repository, runner, and workflow.
- Improve automation and reusability.
- Simplify debugging and workflow customization.

---

## 1. Expression Syntax

Expressions use the following syntax:

```yaml
${{ <expression> }}
```

They can be used in:

- `run`
- `env`
- `if`
- `with`
- `name`
- various workflow configuration fields

### Example

```yaml
- name: Print Branch Name
  run: echo "Branch is ${{ github.ref_name }}"
```

### Explanation

- `${{ }}` evaluates an expression at runtime.
- Expressions can reference contexts, functions, variables, and literals.
- They are evaluated before the workflow step executes.

> **Note:** Expressions are one of the most commonly used features in GitHub Actions workflows.

---

## 2. Common Contexts

Contexts are objects that provide information about the workflow execution environment.

### GitHub Context (`github`)

Provides information about:

- Repository
- Branch
- Workflow events
- Commit details
- Workflow runs

| Expression | Description |
|----------|----------|
| `${{ github.ref_name }}` | Branch or tag name |
| `${{ github.sha }}` | Commit SHA |
| `${{ github.actor }}` | User who triggered the workflow |
| `${{ github.repository }}` | Repository name |
| `${{ github.event_name }}` | Triggering event |
| `${{ github.run_id }}` | Workflow run ID |

### Example

```yaml
- name: Print Repository Information
  run: |
    echo "Repository: ${{ github.repository }}"
    echo "Branch: ${{ github.ref_name }}"
```

---

### Runner Context (`runner`)

Provides information about the GitHub Actions runner.

| Expression | Description |
|----------|----------|
| `${{ runner.os }}` | Operating system |
| `${{ runner.arch }}` | System architecture |
| `${{ runner.temp }}` | Temporary directory path |

### Example

```yaml
- name: Print Runner Information
  run: |
    echo "OS: ${{ runner.os }}"
    echo "Architecture: ${{ runner.arch }}"
```

---

### Environment Context (`env`)

Used to access environment variables.

### Example

```yaml
env:
  MY_VAR: hello

steps:
  - name: Print Environment Variable
    run: echo "${{ env.MY_VAR }}"
```

### Explanation

- Reads values defined using `env`.
- Useful for reusable workflow configurations.

---

### Secrets Context (`secrets`)

Provides access to repository and organization secrets.

### Example

```yaml
- name: Access Secret
  run: echo "${{ secrets.API_TOKEN }}"
```

> **Important:** Never print secret values in production workflows.

### Common Uses

- API tokens
- Cloud credentials
- Database passwords
- Deployment keys

---

### Steps Context (`steps`)

Used to access outputs from previously executed steps.

### Example

```yaml
steps:
  - name: Set Value
    id: my_step
    run: echo "result=42" >> $GITHUB_OUTPUT

  - name: Use Value
    run: echo "Result is ${{ steps.my_step.outputs.result }}"
```

### Explanation

- Allows communication between workflow steps.
- Uses the step `id` and output values.
- Improves workflow modularity.

---

## 3. Conditional Expressions (`if`)

Conditional expressions determine whether a job or step should run.

### Syntax

```yaml
if: condition
```

> **Note:** The `${{ }}` wrapper is optional when using `if`.

---

### Run Only on the Main Branch

```yaml
- name: Deploy Application
  if: github.ref_name == 'main'
  run: ./deploy.sh
```

---

### Run Only on Push Events

```yaml
- name: Build Application
  if: github.event_name == 'push'
  run: ./build.sh
```

---

### Run Only if a Previous Step Fails

```yaml
- name: Notify on Failure
  if: failure()
  run: echo "Something went wrong!"
```

### Common Use Cases

- Branch-specific deployments
- Event-based workflows
- Failure notifications
- Conditional testing
- Cleanup operations

---

## 4. Built-in Functions

GitHub Actions provides several useful functions for workflow logic.

| Function | Description |
|--------|--------|
| `success()` | Returns true if all previous steps passed |
| `failure()` | Returns true if any previous step failed |
| `always()` | Always runs the step |
| `contains()` | Checks whether a string contains a value |
| `startsWith()` | Checks whether a string starts with a prefix |
| `toJSON()` | Converts values to JSON format |

---

### Always Run Cleanup Tasks

```yaml
- name: Cleanup Files
  if: always()
  run: rm -rf temp/
```

---

### Check Release Branches

```yaml
- name: Release Branch Check
  if: startsWith(github.ref_name, 'release')
  run: echo "This is a release branch."
```

---

## Debugging Contexts

The `toJSON()` function is extremely useful for debugging workflows.

### Example

```yaml
- name: Debug GitHub Context
  run: echo '${{ toJSON(github) }}'
```

This prints all available information stored in the GitHub context.

> **Tip:** Use `toJSON()` when learning GitHub Actions or troubleshooting workflow issues.

---

## Key Rules

- Use `${{ }}` syntax for expressions.
- The `${{ }}` wrapper is optional inside `if`.
- Contexts are read-only.
- Use `$GITHUB_OUTPUT` to pass data between steps.
- Avoid using environment variables for step outputs.
- Never hardcode sensitive values.
- Use built-in functions to simplify workflow logic.

---

## Best Practices

- Prefer expressions over hardcoded values.
- Keep conditional logic simple and readable.
- Use descriptive step IDs when working with outputs.
- Store credentials in GitHub Secrets.
- Use `always()` for cleanup operations.
- Use `toJSON()` for debugging contexts.
- Follow the principle of least privilege for secrets.

---

## Key Takeaways

- Expressions make workflows dynamic.
- Contexts provide runtime information.
- Conditional expressions improve workflow flexibility.
- Built-in functions simplify CI/CD logic.
- Step outputs enable communication between workflow steps.
- GitHub Secrets should always be used for sensitive information.

---

## Quick Summary

| Concept | Purpose |
|--------|--------|
| Expressions | Evaluate dynamic values |
| Contexts | Provide workflow metadata |
| github | Repository and workflow information |
| runner | Runner environment information |
| env | Access environment variables |
| secrets | Access sensitive values securely |
| steps | Access previous step outputs |
| if | Run steps conditionally |
| Built-in Functions | Simplify workflow logic |
| toJSON() | Debug workflow contexts |
