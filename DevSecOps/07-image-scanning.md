# Image Scanning

## What is Image Scanning?

**Image Scanning** is the process of analyzing container images to identify known security vulnerabilities, misconfigurations, secrets, and outdated software packages.

It helps ensure that only secure container images are deployed to production.

---

## Why is Image Scanning Important?

Container images often include operating system packages and application dependencies that may contain known vulnerabilities.

Image scanning helps:

- Detect security vulnerabilities
- Identify outdated packages
- Find exposed secrets
- Improve container security
- Prevent vulnerable images from reaching production

---

## How Image Scanning Works

```
Build Container Image
         │
         ▼
Image Scanner
(Trivy, Docker Scout, Grype)
         │
         ▼
Scan Image Layers
         │
         ▼
Compare with Vulnerability Database
         │
         ▼
Generate Security Report
         │
         ▼
Fix Issues & Rebuild Image
```

Image scanners compare installed packages and dependencies against public vulnerability databases such as the **National Vulnerability Database (NVD)**.

---

## What Does Image Scanning Detect?

- Vulnerable operating system packages
- Vulnerable application dependencies
- Known CVEs (Common Vulnerabilities and Exposures)
- Misconfigurations
- Hardcoded secrets (supported by some tools)
- Outdated software versions

---

## Trivy Example

Scan a local Docker image:

```bash
trivy image nginx:latest
```

Scan a Docker image from a registry:

```bash
trivy image my-app:latest
```

Scan the current project directory:

```bash
trivy fs .
```

---

## GitHub Actions Example

```yaml
name: Image Scan

on:
  push:
    branches:
      - main

jobs:
  trivy-scan:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Run Trivy Scanner
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: my-app:latest
```

The workflow automatically scans the container image whenever code is pushed to the `main` branch.

---

## Popular Image Scanning Tools

| Tool | Description |
|------|-------------|
| Trivy | Fast and open-source vulnerability scanner |
| Docker Scout | Docker image security analysis |
| Grype | Container vulnerability scanner |
| Clair | Static container image analysis |
| Anchore | Enterprise container security platform |

---

## Advantages

- Detects vulnerabilities before deployment
- Easy to automate in CI/CD
- Improves container security
- Supports compliance requirements
- Reduces production security risks

---

## Limitations

- Only detects known vulnerabilities
- Requires updated vulnerability databases
- May generate false positives
- Cannot detect runtime attacks

---

## Best Practices

- Scan every image before deployment
- Use minimal and trusted base images
- Keep images updated regularly
- Fail CI/CD pipelines for critical vulnerabilities
- Remove unused packages
- Rebuild images after security updates
- Combine image scanning with runtime monitoring

---

## Quick Summary

| Topic | Description |
|--------|-------------|
| Purpose | Detect vulnerabilities in container images |
| Scans | OS packages, dependencies, image layers |
| Finds | CVEs, outdated software, misconfigurations |
| Popular Tools | Trivy, Docker Scout, Grype, Clair |
| Best Practice | Scan images before every deployment |

---

## Key Takeaways

- Image scanning identifies **known vulnerabilities** before containers are deployed.
- Tools like **Trivy** and **Docker Scout** automate vulnerability detection.
- Integrating image scanning into **CI/CD pipelines** improves software security.
- Regular scanning and image updates help reduce security risks in production.
