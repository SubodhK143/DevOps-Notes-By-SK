# DevSecOps Fundamentals

## What is DevSecOps?

**DevSecOps** stands for **Development + Security + Operations**.

It is the practice of integrating security into every stage of the Software Development Life Cycle (SDLC) instead of treating it as a separate step at the end.

The goal of DevSecOps is to build, test, deploy, and maintain applications securely while delivering software faster.

---

## Why DevSecOps?

In traditional software development, security was usually performed only before production. This often resulted in late vulnerability detection, costly fixes, and delayed releases.

DevSecOps shifts security to the beginning of the development process, allowing teams to identify and fix vulnerabilities early.

### Benefits

- Detect vulnerabilities earlier
- Reduce security risks
- Deliver software faster
- Automate security testing
- Improve collaboration between teams
- Support continuous delivery

---

## DevSecOps Lifecycle

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
Security Scan
   │
   ▼
Deploy
   │
   ▼
Monitor
```

Security is integrated into every stage of the lifecycle.

---

## DevOps vs DevSecOps

| DevOps | DevSecOps |
|--------|-----------|
| Focuses on speed and automation | Focuses on speed, automation, and security |
| Security is often added later | Security is integrated from the beginning |
| Separate security team | Shared security responsibility |
| Vulnerabilities found late | Vulnerabilities found early |

---

## Shift Left Security

**Shift Left** means moving security activities earlier in the software development lifecycle.

Instead of waiting until deployment, developers perform security checks during coding, building, and testing.

This helps reduce vulnerabilities before they reach production.

---

## Shared Responsibility

DevSecOps encourages everyone to participate in security.

| Team | Responsibility |
|------|----------------|
| Developers | Write secure code |
| Security Team | Define policies and perform assessments |
| DevOps Engineers | Automate security in CI/CD pipelines |
| Operations Team | Monitor and respond to threats |

---

## Common DevSecOps Practices

- Secure coding
- Secrets management
- Static Application Security Testing (SAST)
- Dynamic Application Security Testing (DAST)
- Software Composition Analysis (SCA)
- Container image scanning
- Infrastructure as Code (IaC) scanning
- Continuous security monitoring

---

## Popular DevSecOps Tools

| Category | Tools |
|----------|-------|
| Source Control | GitHub, GitLab |
| CI/CD | GitHub Actions, Jenkins |
| SAST | SonarQube, Semgrep |
| DAST | OWASP ZAP |
| SCA | Dependabot, Snyk |
| Container Scanning | Trivy, Docker Scout |
| Secrets Management | GitHub Secrets, AWS Secrets Manager, HashiCorp Vault |
| IaC Security | Checkov, tfsec |
| Monitoring | Prometheus, Grafana |

---

## Best Practices

- Shift security left
- Automate security testing
- Never hardcode secrets
- Scan dependencies regularly
- Scan container images before deployment
- Apply the Principle of Least Privilege (PoLP)
- Monitor applications continuously
- Keep dependencies up to date

---

## Quick Summary

| Topic | Description |
|--------|-------------|
| DevSecOps | Integrates security into DevOps |
| Goal | Build secure software faster |
| Key Idea | Security at every stage of SDLC |
| Shift Left | Detect vulnerabilities early |
| Benefits | Faster delivery, reduced risk, automation |
| Popular Tools | GitHub Actions, Trivy, SonarQube, OWASP ZAP, Snyk |

---

## Key Takeaways

- DevSecOps integrates **development, security, and operations** into a single workflow.
- Security is a **shared responsibility** across all teams.
- Automating security improves both software quality and delivery speed.
- Detecting vulnerabilities early reduces cost and minimizes production risks.
