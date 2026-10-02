# CI/CD Security

## What is CI/CD Security?

**CI/CD Security** is the practice of integrating security into every stage of the Continuous Integration and Continuous Deployment (CI/CD) pipeline.

It ensures that code, dependencies, build artifacts, container images, and deployments are continuously validated before reaching production.

The goal is to deliver software **quickly, securely, and reliably**.

---

## Why is CI/CD Security Important?

A CI/CD pipeline automates software delivery. If the pipeline itself is compromised, attackers can inject malicious code into production.

CI/CD Security helps:

- Protect the software delivery process
- Detect vulnerabilities early
- Secure build artifacts
- Prevent unauthorized deployments
- Reduce supply chain risks

---

## Secure CI/CD Pipeline

```
Developer
     │
     ▼
Source Code
     │
     ▼
Build
     │
     ▼
SAST
     │
     ▼
SCA
     │
     ▼
Build Container Image
     │
     ▼
Image Scan
     │
     ▼
Policy Checks
     │
     ▼
Deploy
     │
     ▼
DAST
     │
     ▼
Production
```

Security checks should be integrated throughout the pipeline.

---

## Common CI/CD Security Checks

| Stage | Security Check |
|--------|----------------|
| Source Code | SAST |
| Dependencies | SCA |
| Secrets | Secret Scanning |
| Container Image | Image Scanning |
| Infrastructure | Policy as Code |
| Deployment | DAST |
| Release | SBOM Generation |

---

## Common CI/CD Security Risks

- Hardcoded secrets
- Vulnerable dependencies
- Insecure build environments
- Untrusted container images
- Weak access controls
- Unsigned build artifacts
- Excessive pipeline permissions

---

## Example GitHub Actions Workflow

```yaml
name: Secure CI Pipeline

on:
  push:
    branches:
      - main

jobs:
  security:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Run SAST
        run: echo "Scanning source code..."

      - name: Run SCA
        run: echo "Checking dependencies..."

      - name: Scan Container Image
        run: echo "Scanning Docker image..."
```

In a production pipeline, these steps would use tools such as **Semgrep**, **Dependabot**, and **Trivy**.

---

## Popular CI/CD Security Tools

| Category | Tool |
|----------|------|
| CI/CD | GitHub Actions, Jenkins, GitLab CI |
| SAST | Semgrep, SonarQube, CodeQL |
| SCA | Dependabot, Snyk |
| Image Scanning | Trivy, Docker Scout |
| DAST | OWASP ZAP |
| Policy as Code | OPA, Conftest, Kyverno |
| Secrets | GitHub Secrets, HashiCorp Vault |

---

## Advantages

- Detects vulnerabilities early
- Automates security testing
- Reduces manual effort
- Improves software quality
- Strengthens software supply chain security

---

## Limitations

- Requires proper tool integration
- May increase pipeline execution time
- False positives require review
- Security rules need regular updates

---

## Best Practices

- Automate security checks
- Protect CI/CD secrets
- Scan every pull request
- Scan dependencies regularly
- Scan container images before deployment
- Apply Policy as Code
- Use least privilege for pipeline permissions
- Keep CI/CD tools updated

---

## Quick Summary

| Topic | Description |
|--------|-------------|
| Purpose | Secure the software delivery pipeline |
| Focus | Build, test, scan, deploy securely |
| Common Risks | Secrets, vulnerable dependencies, insecure pipelines |
| Popular Tools | GitHub Actions, Trivy, Semgrep, Dependabot, OWASP ZAP |
| Best Practice | Integrate security into every CI/CD stage |

---

## Key Takeaways

- CI/CD Security integrates **security into every stage** of the software delivery pipeline.
- Automated checks such as **SAST, SCA, Image Scanning, DAST, and Policy as Code** improve software security.
- Protecting CI/CD pipelines is essential for preventing software supply chain attacks.
- Secure pipelines enable organizations to deliver software **faster, more safely, and with greater confidence**.
