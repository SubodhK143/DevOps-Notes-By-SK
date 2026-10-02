# Software Bill of Materials (SBOM)

## What is an SBOM?

A **Software Bill of Materials (SBOM)** is a detailed inventory of all the components, libraries, packages, and dependencies used to build a software application.

It provides complete visibility into what software contains, making it easier to identify vulnerabilities, manage dependencies, and meet compliance requirements.

Think of an SBOM as an **ingredient list** for your software.

---

## Why is SBOM Important?

Modern applications rely heavily on open-source components. If one of these components has a vulnerability, it can affect your application.

SBOM helps:

- Improve software transparency
- Identify vulnerable components quickly
- Strengthen software supply chain security
- Support compliance and audits
- Speed up vulnerability response

---

## How SBOM Works

```
Application Source Code
          │
          ▼
Dependencies & Libraries
          │
          ▼
SBOM Generator
          │
          ▼
SBOM File
(SPDX / CycloneDX)
          │
          ▼
Security & Compliance Tools
```

The generated SBOM lists every software component included in the application.

---

## What Information Does an SBOM Contain?

- Application name
- Software components
- Package versions
- Licenses
- Suppliers
- Dependency relationships
- Component identifiers

---

## Common SBOM Formats

| Format | Description |
|---------|-------------|
| SPDX | Software Package Data Exchange |
| CycloneDX | Security-focused SBOM format |

---

## Example

A simplified SBOM:

```text
Application: Shopping API

Dependencies:
- Express 5.1.0
- MongoDB Driver 6.18.0
- dotenv 17.2.1
- JWT 9.0.2
```

This inventory helps security teams identify affected components when new vulnerabilities are discovered.

---

## Popular SBOM Tools

| Tool | Description |
|------|-------------|
| Syft | Generates SBOMs for containers and filesystems |
| Trivy | Generates SBOMs and scans for vulnerabilities |
| Docker Scout | Creates SBOMs for Docker images |
| CycloneDX CLI | Generates CycloneDX SBOMs |
| SPDX Tools | Creates and validates SPDX documents |

---

## Advantages

- Complete software inventory
- Faster vulnerability detection
- Better dependency management
- Improved compliance
- Stronger software supply chain security

---

## Limitations

- Must be updated regularly
- Does not fix vulnerabilities automatically
- Requires integration into the development pipeline
- Accuracy depends on the quality of generated data

---

## Best Practices

- Generate an SBOM for every release
- Keep SBOMs updated
- Store SBOMs securely
- Automate SBOM generation in CI/CD
- Combine SBOMs with vulnerability scanning
- Monitor components for newly disclosed CVEs

---

## Quick Summary

| Topic | Description |
|--------|-------------|
| Full Form | Software Bill of Materials |
| Purpose | Inventory of software components |
| Contains | Dependencies, versions, licenses |
| Common Formats | SPDX, CycloneDX |
| Popular Tools | Syft, Trivy, Docker Scout |
| Best Practice | Generate an SBOM for every software release |

---

## Key Takeaways

- An **SBOM** is a complete inventory of a software application's components.
- It improves visibility into dependencies and supports software supply chain security.
- SBOMs help organizations respond quickly to newly discovered vulnerabilities.
- Generating SBOMs as part of the **CI/CD pipeline** is considered a DevSecOps best practice.
