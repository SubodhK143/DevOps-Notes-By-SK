# Dynamic Application Security Testing (DAST)

## What is DAST?

**Dynamic Application Security Testing (DAST)** is a security testing method that analyzes a **running application** from the outside, just like an attacker would.

Instead of examining the source code, DAST interacts with the live application to identify security vulnerabilities.

DAST is also known as **Black Box Testing** because it tests the application without needing access to its source code.

---

## Why is DAST Important?

Some security vulnerabilities only appear when an application is running.

DAST helps:

- Detect runtime vulnerabilities
- Identify security misconfigurations
- Validate authentication and authorization
- Improve application security before production
- Complement SAST for complete security testing

---

## How DAST Works

```
Application Running
         │
         ▼
DAST Tool Sends Requests
         │
         ▼
Application Responds
         │
         ▼
Analyze Responses
         │
         ▼
Report Vulnerabilities
```

The application **must be running** for DAST to perform security testing.

---

## Common Vulnerabilities Detected

- SQL Injection (SQLi)
- Cross-Site Scripting (XSS)
- Cross-Site Request Forgery (CSRF)
- Authentication Issues
- Session Management Problems
- Security Misconfigurations
- Insecure HTTP Headers

---

## When is DAST Used?

DAST is typically performed:

- After the application is deployed to a test environment
- During QA testing
- Before production deployment
- During CI/CD pipelines
- As part of continuous security testing

---

## DAST Example

GitHub Actions workflow using **OWASP ZAP**:

```yaml
name: DAST Scan

on:
  workflow_dispatch:

jobs:
  dast:
    runs-on: ubuntu-latest

    steps:
      - uses: zaproxy/action-baseline@v0.13.0
        with:
          target: "https://example.com"
```

The workflow scans the running web application and generates a security report.

---

## Popular DAST Tools

| Tool | Description |
|------|-------------|
| OWASP ZAP | Open-source web application scanner |
| Burp Suite | Web security testing platform |
| Invicti | Automated web vulnerability scanner |
| Acunetix | Web application security testing |
| Nikto | Web server vulnerability scanner |

---

## Advantages

- Detects runtime vulnerabilities
- No source code required
- Simulates real-world attacks
- Finds configuration issues
- Validates deployed applications

---

## Limitations

- Requires a running application
- Cannot identify code-level issues
- May take longer than SAST
- Limited visibility into the source code
- May produce false positives

---

## SAST vs DAST

| SAST | DAST |
|------|------|
| Scans source code | Scans running application |
| White Box Testing | Black Box Testing |
| No application execution | Requires application execution |
| Finds coding issues | Finds runtime vulnerabilities |
| Used during development | Used after deployment |

---

## Best Practices

- Run DAST before production releases
- Automate DAST in CI/CD pipelines
- Scan staging environments regularly
- Review and verify findings
- Fix high-risk vulnerabilities first
- Use DAST together with SAST and SCA

---

## Quick Summary

| Topic | Description |
|--------|-------------|
| Full Form | Dynamic Application Security Testing |
| Type | Black Box Testing |
| Scans | Running application |
| Runs | After deployment or in staging |
| Finds | Runtime vulnerabilities |
| Common Tools | OWASP ZAP, Burp Suite, Acunetix |

---

## Key Takeaways

- DAST tests a **running application** from an attacker's perspective.
- It identifies vulnerabilities that may not be visible through source code analysis.
- DAST is most effective when combined with **SAST** and **SCA**.
- Automating DAST in CI/CD pipelines helps improve application security before production.
