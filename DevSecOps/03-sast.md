# Static Application Security Testing (SAST)

## What is SAST?

**Static Application Security Testing (SAST)** is a security testing method that analyzes an application's **source code, bytecode, or binaries** without executing the application.

It helps developers identify security vulnerabilities early in the Software Development Life Cycle (SDLC).

SAST is also known as **White Box Testing** because it examines the application's internal code.

---

## Why is SAST Important?

Finding vulnerabilities during development is much easier and less expensive than fixing them after deployment.

SAST helps:

- Detect security issues early
- Improve code quality
- Reduce security risks
- Support secure coding practices
- Integrate security into CI/CD pipelines

---

## How SAST Works

```
Developer Writes Code
          │
          ▼
Source Code
          │
          ▼
SAST Tool Scans Code
          │
          ▼
Detect Vulnerabilities
          │
          ▼
Developer Fixes Issues
```

The application is **not executed** during a SAST scan.

---

## Common Vulnerabilities Detected

- SQL Injection
- Cross-Site Scripting (XSS)
- Hardcoded Secrets
- Weak Cryptography
- Command Injection
- Buffer Overflow
- Insecure Code Patterns

---

## When is SAST Used?

SAST is typically performed:

- During development
- Before code is merged
- During pull requests
- During CI/CD builds
- Before deployment

---

## SAST Example

GitHub Actions workflow using **Semgrep**:

```yaml
name: SAST Scan

on:
  push:
    branches:
      - main

jobs:
  sast:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: returntocorp/semgrep-action@v1
```

The workflow scans the source code automatically whenever code is pushed to the `main` branch.

---

## Popular SAST Tools

| Tool | Description |
|------|-------------|
| SonarQube | Code quality and security analysis |
| Semgrep | Fast static code analysis |
| Checkmarx | Enterprise SAST platform |
| Fortify | Application security testing |
| CodeQL | GitHub's semantic code analysis |

---

## Advantages

- Detects vulnerabilities early
- No need to run the application
- Easily integrates with CI/CD
- Improves code quality
- Reduces remediation costs

---

## Limitations

- Cannot detect runtime vulnerabilities
- May generate false positives
- Limited visibility into application behavior
- Requires developers to review findings

---

## Best Practices

- Scan every pull request
- Automate SAST in CI/CD pipelines
- Fix high-severity issues first
- Keep SAST rules updated
- Combine SAST with DAST and SCA
- Review false positives before ignoring them

---

## Quick Summary

| Topic | Description |
|--------|-------------|
| Full Form | Static Application Security Testing |
| Type | White Box Testing |
| Scans | Source code, bytecode, binaries |
| Runs | Before application execution |
| Finds | Coding vulnerabilities |
| Common Tools | SonarQube, Semgrep, CodeQL, Checkmarx |

---

## Key Takeaways

- SAST analyzes **source code without running the application**.
- It helps developers identify vulnerabilities early in the SDLC.
- SAST is commonly integrated into CI/CD pipelines for automated security scanning.
- For complete application security, SAST should be combined with **DAST** and **SCA**.
