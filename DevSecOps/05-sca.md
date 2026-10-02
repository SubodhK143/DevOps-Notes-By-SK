# Software Composition Analysis (SCA)

## What is SCA?

**Software Composition Analysis (SCA)** is a security testing method that analyzes an application's **third-party libraries, open-source dependencies, and packages** for known vulnerabilities, outdated versions, and license issues.

Instead of scanning your own code, SCA focuses on the external components your application depends on.

---

## Why is SCA Important?

Modern applications often use hundreds of open-source libraries. If one of these libraries contains a known vulnerability, your application may also be at risk.

SCA helps:

- Detect vulnerable dependencies
- Identify outdated packages
- Check software licenses
- Reduce supply chain risks
- Keep applications secure and up to date

---

## How SCA Works

```
Application
      │
      ▼
Dependency Files
(package.json, pom.xml,
requirements.txt, etc.)
      │
      ▼
SCA Tool Scans Dependencies
      │
      ▼
Compare with Vulnerability Database
      │
      ▼
Security Report Generated
```

The SCA tool compares your project's dependencies against public vulnerability databases such as the **National Vulnerability Database (NVD)**.

---

## Common Dependency Files

| Language | Dependency File |
|----------|-----------------|
| Node.js | package.json |
| Python | requirements.txt |
| Java | pom.xml |
| .NET | .csproj |
| Go | go.mod |

---

## What Does SCA Detect?

- Vulnerable open-source libraries
- Outdated package versions
- License compliance issues
- Dependency risks
- Known CVEs (Common Vulnerabilities and Exposures)

---

## Example

GitHub automatically scans dependencies using **Dependabot**.

Example GitHub Actions workflow:

```yaml
name: Dependency Review

on:
  pull_request:

jobs:
  dependency-review:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/dependency-review-action@v4
```

This workflow checks whether newly added dependencies contain known security vulnerabilities.

---

## Popular SCA Tools

| Tool | Description |
|------|-------------|
| Dependabot | GitHub dependency scanning |
| Snyk | Open-source security platform |
| OWASP Dependency-Check | Detects vulnerable libraries |
| Mend (WhiteSource) | Software composition analysis |
| Trivy | Scans dependencies and container images |

---

## Advantages

- Detects vulnerable dependencies
- Improves software supply chain security
- Supports license compliance
- Easy to automate in CI/CD
- Reduces security risks

---

## Limitations

- Only detects known vulnerabilities
- Cannot identify vulnerabilities in custom code
- May generate false positives
- Requires regular dependency updates

---

## Best Practices

- Keep dependencies updated
- Remove unused packages
- Automate dependency scanning
- Review dependency updates before merging
- Monitor newly disclosed CVEs
- Combine SCA with SAST and DAST

---

## Quick Summary

| Topic | Description |
|--------|-------------|
| Full Form | Software Composition Analysis |
| Scans | Open-source libraries and dependencies |
| Finds | Vulnerabilities, outdated packages, license issues |
| Common Tools | Dependabot, Snyk, Trivy, OWASP Dependency-Check |
| Best Practice | Scan dependencies regularly and update them |

---

## Key Takeaways

- SCA scans **third-party dependencies**, not your application's source code.
- It helps identify **known vulnerabilities** in open-source libraries.
- Automating SCA in CI/CD pipelines improves software supply chain security.
- SCA complements **SAST** and **DAST** for comprehensive application security.
