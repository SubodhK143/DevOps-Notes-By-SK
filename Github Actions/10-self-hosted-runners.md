# GitHub Actions: Self-Hosted Runners

Self-hosted runners are machines that you configure and manage yourself to execute GitHub Actions workflows.

Unlike GitHub-hosted runners, self-hosted runners allow you to use your own infrastructure and customize the execution environment according to your requirements.

Common use cases include:

- Running workflows on on-premises infrastructure.
- Using custom hardware or software configurations.
- Deploying applications within private networks.
- Running resource-intensive workloads.
- Maintaining complete control over the execution environment.

---

## Why Use Self-Hosted Runners?

Self-hosted runners help you:

- Gain full control over the runner environment.
- Use custom tools and configurations.
- Reduce limitations of GitHub-hosted runners.
- Access private or internal resources securely.
- Run workflows on dedicated infrastructure.

> **Note:** Self-hosted runners are ideal for enterprise environments and advanced CI/CD workflows.

---

## 1. What are Self-Hosted Runners?

Self-hosted runners are physical or virtual machines that execute GitHub Actions workflows.

These runners are:

- Managed by you.
- Registered with GitHub.
- Connected securely to your repository or organization.
- Responsible for executing assigned workflow jobs.

### 1.1 Supported Platforms

GitHub supports self-hosted runners on:

- Linux
- Windows
- macOS

Self-hosted runners can run on:

- Local machines
- Virtual machines
- Cloud instances
- On-premises servers
- Kubernetes clusters

---

## 2. How Self-Hosted Runners Work

GitHub communicates with the registered runner whenever a workflow job is triggered.

### Workflow Execution Flow

```text
GitHub Repository
        |
        v
Workflow Triggered
        |
        v
GitHub Actions Service
        |
        v
Self-Hosted Runner Receives Job
        |
        v
Executes Workflow
        |
        v
Sends Results Back to GitHub
```

### Benefits

- Greater flexibility.
- Custom environments.
- Improved infrastructure control.
- Support for internal deployments.

---

## 3. GitHub-Hosted vs Self-Hosted Runners

| Feature | GitHub-Hosted Runner | Self-Hosted Runner |
|--------|--------|--------|
| Infrastructure | Managed by GitHub | Managed by You |
| Custom Software | Limited | Fully Customizable |
| Cost | Included with GitHub plans | Infrastructure costs apply |
| Maintenance | GitHub | User |
| Network Access | Public | Public or Private |
| Hardware Customization | Limited | Fully Supported |
| Use Cases | General CI/CD | Advanced CI/CD Pipelines |

> **Tip:** Use GitHub-hosted runners for simple projects and self-hosted runners for specialized requirements.

---

## 4. Creating a Self-Hosted Runner

Self-hosted runners can be configured at:

- Repository level
- Organization level
- Enterprise level

### 4.1 Setup Process

The basic setup process is:

```text
GitHub Repository
       |
       v
Settings
       |
       v
Actions
       |
       v
Runners
       |
       v
New Self-Hosted Runner
       |
       v
Download Runner Package
       |
       v
Configure Runner
       |
       v
Start Runner Service
```

---

## 5. Runner Labels

Runner labels help GitHub identify which runner should execute a workflow.

### Common Labels

- self-hosted
- linux
- windows
- macOS
- x64
- ARM64

### Example

```yaml
runs-on:
  - self-hosted
  - linux
  - x64
```

### Explanation

- GitHub searches for a runner matching all specified labels.
- Only matching runners will execute the workflow.

> **Note:** Labels improve workflow targeting and scalability.

---

## 6. Using Self-Hosted Runners in Workflows

Specify the self-hosted runner inside the `runs-on` field.

### Example

```yaml
jobs:
  build:
    runs-on: self-hosted

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Build Application
        run: echo "Running on a self-hosted runner."
```

### Using Multiple Labels

#### Example

```yaml
jobs:
  deploy:
    runs-on:
      - self-hosted
      - linux
      - x64

    steps:
      - run: echo "Deploying application."
```

### Explanation

- Multiple labels help target specific runner configurations.
- Useful for large-scale CI/CD pipelines.

---

## 7. Common Use Cases

Self-hosted runners are commonly used for:

- Internal deployments.
- Kubernetes workloads.
- Docker builds.
- Infrastructure automation.
- Enterprise CI/CD pipelines.
- GPU workloads.
- Machine learning projects.
- Private network deployments.

### Examples

```text
Terraform Deployment

Docker Image Builds

Kubernetes Deployments

AWS Infrastructure Automation

Private Database Access

GPU-Based Workloads
```

---

## 8. Security Considerations

Self-hosted runners require proper security practices.

### Best Security Practices

- Restrict repository access.
- Use dedicated runner machines.
- Regularly update the runner software.
- Avoid running untrusted code.
- Limit network permissions.
- Store credentials securely using GitHub Secrets.

> **Important:** Self-hosted runners can access your infrastructure. Ensure they are properly secured before using them in production.

---

## 9. Advantages of Self-Hosted Runners

- Complete infrastructure control.
- Custom software support.
- Private network access.
- Better resource allocation.
- Flexible hardware configurations.
- Improved scalability for enterprise workloads.

---

## 10. Limitations of Self-Hosted Runners

- Infrastructure management is your responsibility.
- Requires regular maintenance.
- Security must be managed properly.
- Hardware and cloud costs may apply.
- Scaling must be configured manually.

---

## 11. Best Practices

- Use labels to organize runners.
- Keep runner software up to date.
- Use dedicated runners for production environments.
- Restrict access to sensitive resources.
- Monitor runner health regularly.
- Use GitHub Secrets for sensitive data.
- Remove unused runners when no longer needed.

---

## 12. Key Rules

- Self-hosted runners are managed by the user.
- They must be registered with GitHub.
- Workflows use the `runs-on` field to target runners.
- Labels help identify appropriate runners.
- Security and maintenance are your responsibility.
- Self-hosted runners support Linux, Windows, and macOS.

---

## 13. Key Takeaways

- Self-hosted runners provide complete control over the execution environment.
- They are ideal for advanced and enterprise CI/CD pipelines.
- Runner labels improve workflow targeting.
- Proper security practices are essential.
- Self-hosted runners offer greater flexibility than GitHub-hosted runners.

---

## 14. Quick Summary

| Concept | Purpose |
|--------|--------|
| Self-Hosted Runner | Execute workflows on your infrastructure |
| Runner Labels | Target specific runners |
| runs-on | Specify the runner configuration |
| Repository Runner | Available to a specific repository |
| Organization Runner | Shared across repositories |
| Security Practices | Protect infrastructure and credentials |
| Best Practice | Use dedicated and secured runners |
