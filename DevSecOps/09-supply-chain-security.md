# Supply Chain Security

## What is Supply Chain Security?

**Supply Chain Security** is the practice of protecting every stage of the software development and delivery process from security threats.

It focuses on securing the source code, dependencies, build process, container images, CI/CD pipelines, and software artifacts before they reach production.

The goal is to ensure that only trusted and secure software is delivered to users.

---

## Why is Supply Chain Security Important?

Modern applications depend on open-source libraries, third-party packages, build tools, and container images.

If any component is compromised, the entire application may become vulnerable.

Supply Chain Security helps:

- Prevent software tampering
- Protect against malicious dependencies
- Secure CI/CD pipelines
- Reduce supply chain attacks
- Improve trust in software releases

---

## Software Supply Chain

```
Developer
     │
     ▼
Source Code
     │
     ▼
Dependencies
     │
     ▼
Build Process
     │
     ▼
Container Image
     │
     ▼
CI/CD Pipeline
     │
     ▼
Deployment
     │
     ▼
Production
```

Every stage should include security checks.

---

## Common Supply Chain Risks

- Vulnerable open-source libraries
- Malicious third-party packages
- Compromised container images
- Leaked secrets
- Insecure CI/CD pipelines
- Tampered software artifacts
- Outdated dependencies

---

## How to Secure the Software Supply Chain

- Scan source code using SAST
- Scan dependencies using SCA
- Scan running applications using DAST
- Scan container images before deployment
- Generate an SBOM for every release
- Digitally sign software artifacts
- Store secrets securely
- Keep dependencies updated

---

## Common Security Checks

| Stage | Security Check |
|--------|----------------|
| Source Code | SAST |
| Dependencies | SCA |
| Build | Secrets Scanning |
| Container Images | Image Scanning |
| Deployment | DAST |
| Release | SBOM Generation |
| Runtime | Continuous Monitoring |

---

## Popular Supply Chain Security Tools

| Tool | Purpose |
|------|---------|
| GitHub Advanced Security | Code and secret scanning |
| Dependabot | Dependency updates |
| Trivy | Vulnerability scanning |
| Syft | SBOM generation |
| Cosign | Container image signing |
| Sigstore | Software signing and verification |

---

## Advantages

- Improves software integrity
- Prevents supply chain attacks
- Protects software releases
- Increases customer trust
- Supports compliance requirements

---

## Limitations

- Requires multiple security tools
- Continuous monitoring is necessary
- Security policies must be maintained
- Cannot eliminate all risks

---

## Best Practices

- Use trusted dependencies
- Keep packages updated
- Verify container images
- Generate an SBOM for every release
- Digitally sign software artifacts
- Protect CI/CD pipelines
- Rotate secrets regularly
- Continuously monitor for vulnerabilities

---

## Quick Summary

| Topic | Description |
|--------|-------------|
| Purpose | Secure the software development and delivery process |
| Protects | Code, dependencies, CI/CD, images, artifacts |
| Common Risks | Vulnerable packages, compromised images, leaked secrets |
| Popular Tools | Dependabot, Trivy, Syft, Cosign, Sigstore |
| Best Practice | Secure every stage of the software supply chain |

---

## Key Takeaways

- Supply Chain Security protects the **entire software delivery lifecycle**, not just the application.
- Security should be integrated from **development to production**.
- Combining **SAST, DAST, SCA, Image Scanning, and SBOM** provides stronger protection.
- Verifying artifacts, securing CI/CD pipelines, and using trusted dependencies reduce the risk of supply chain attacks.
