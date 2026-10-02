# DevSecOps Cheat Sheet

> A quick reference for essential DevSecOps concepts, tools, and best practices.

---

# DevSecOps Lifecycle

```
Plan
  │
  ▼
Develop
  │
  ▼
Build
  │
  ▼
Test
  │
  ▼
Secure
  │
  ▼
Deploy
  │
  ▼
Monitor
```

---

# Security Testing

| Type | Full Form | Purpose | Example Tools |
|------|-----------|---------|---------------|
| SAST | Static Application Security Testing | Scan source code | Semgrep, SonarQube, CodeQL |
| DAST | Dynamic Application Security Testing | Scan running applications | OWASP ZAP, Burp Suite |
| SCA | Software Composition Analysis | Scan dependencies | Dependabot, Snyk |

---

# Container Security

✅ Use trusted base images

✅ Keep images updated

✅ Run containers as a non-root user

✅ Remove unnecessary packages

✅ Scan images before deployment

---

# Image Scanning

### Popular Tools

- Trivy
- Docker Scout
- Grype
- Clair

### Scan Command

```bash
trivy image nginx:latest
```

---

# Secrets Management

Never store:

- Passwords
- API Keys
- Tokens
- SSH Keys
- Certificates

Use:

- GitHub Secrets
- AWS Secrets Manager
- HashiCorp Vault

---

# SBOM

**SBOM (Software Bill of Materials)**

Contains:

- Software Components
- Package Versions
- Licenses
- Dependencies

Popular Tools:

- Syft
- Trivy

---

# Policy as Code

Purpose:

- Enforce security policies automatically

Popular Tools:

- OPA
- Kyverno
- Conftest
- Gatekeeper

---

# IAM Security

### Authentication

Who are you?

### Authorization

What are you allowed to do?

### Best Practices

- Least Privilege (PoLP)
- Enable MFA
- Use Roles
- Rotate Credentials

---

# Kubernetes Security

Focus Areas

- RBAC
- Secrets
- Network Policies
- Security Context
- Pod Security

---

# CI/CD Security

Secure Every Stage

```
Code
   │
   ▼
SAST
   │
   ▼
SCA
   │
   ▼
Build
   │
   ▼
Image Scan
   │
   ▼
SBOM
   │
   ▼
Policy Check
   │
   ▼
Deploy
   │
   ▼
DAST
```

---

# Common DevSecOps Tools

| Category | Tools |
|----------|------|
| Source Control | Git, GitHub |
| CI/CD | GitHub Actions, Jenkins |
| SAST | Semgrep, SonarQube |
| DAST | OWASP ZAP |
| SCA | Dependabot, Snyk |
| Secrets | GitHub Secrets, Vault |
| Containers | Docker |
| Image Scanning | Trivy, Docker Scout |
| SBOM | Syft |
| Policy as Code | OPA, Kyverno |
| Orchestration | Kubernetes |
| Monitoring | Prometheus, Grafana |

---

# DevSecOps Best Practices

- Shift security left
- Automate security testing
- Never hardcode secrets
- Scan dependencies regularly
- Scan container images
- Generate SBOMs
- Apply Policy as Code
- Use Least Privilege (PoLP)
- Enable Multi-Factor Authentication (MFA)
- Monitor production continuously

---

# Complete DevSecOps Pipeline

```
Developer
     │
     ▼
GitHub
     │
     ▼
GitHub Actions
     │
     ├── Secrets Scan
     ├── SAST
     ├── SCA
     ├── Build Docker Image
     ├── Image Scan
     ├── SBOM
     ├── Policy as Code
     ├── Deploy
     └── DAST
           │
           ▼
      Production
           │
           ▼
Monitoring
```

---

# Key Takeaways

- Shift Security Left
- Automate Security
- Secure the CI/CD Pipeline
- Scan Code, Dependencies, and Images
- Protect Secrets
- Apply Least Privilege
- Monitor Continuously
