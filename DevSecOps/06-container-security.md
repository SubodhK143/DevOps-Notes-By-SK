# Container Security

## What is Container Security?

**Container Security** is the practice of protecting containerized applications, container images, runtimes, and the underlying infrastructure from security threats.

It involves securing the entire container lifecycle—from building images to deploying and running containers in production.

---

## Why is Container Security Important?

Containers are lightweight and portable, but they can still contain vulnerabilities if not properly secured.

Container security helps:

- Prevent unauthorized access
- Reduce attack surface
- Protect sensitive data
- Detect vulnerable images
- Secure production workloads

---

## Container Security Lifecycle

```
Build Image
     │
     ▼
Scan Image
     │
     ▼
Store in Registry
     │
     ▼
Deploy Container
     │
     ▼
Monitor & Update
```

Security should be applied at every stage of the container lifecycle.

---

## Common Container Security Risks

- Vulnerable base images
- Outdated packages
- Hardcoded secrets
- Running containers as the root user
- Exposed container ports
- Excessive Linux capabilities
- Untrusted container images

---

## Container Security Best Practices

- Use trusted base images
- Keep images small and up to date
- Scan images for vulnerabilities
- Never store secrets inside images
- Run containers as a non-root user
- Remove unnecessary packages
- Use read-only file systems when possible
- Apply the Principle of Least Privilege (PoLP)

---

## Example

❌ Running as the root user

```dockerfile
FROM ubuntu:24.04

CMD ["python3", "app.py"]
```

✅ Running as a non-root user

```dockerfile
FROM python:3.12-slim

RUN useradd -m appuser

USER appuser

WORKDIR /app

COPY . .

CMD ["python", "app.py"]
```

Running containers as a non-root user reduces the impact of potential attacks.

---

## Popular Container Security Tools

| Tool | Purpose |
|------|---------|
| Trivy | Container image vulnerability scanner |
| Docker Scout | Image analysis and recommendations |
| Grype | Vulnerability scanner |
| Clair | Static image analysis |
| Falco | Runtime threat detection |

---

## Advantages

- Reduces attack surface
- Detects vulnerable images
- Improves application security
- Supports secure deployments
- Strengthens DevSecOps pipelines

---

## Limitations

- Requires continuous image updates
- Misconfigurations can still introduce risks
- Runtime attacks require additional monitoring
- Security depends on proper configuration

---

## Best Practices

- Use official or trusted base images
- Scan every image before deployment
- Keep base images updated
- Avoid running as root
- Remove unused packages and tools
- Store secrets securely using a secrets manager
- Monitor running containers continuously

---

## Quick Summary

| Topic | Description |
|--------|-------------|
| Purpose | Secure containerized applications |
| Focus | Images, runtime, registry, infrastructure |
| Common Risks | Vulnerable images, root user, exposed ports |
| Common Tools | Trivy, Docker Scout, Grype, Clair, Falco |
| Best Practice | Scan images and follow least privilege |

---

## Key Takeaways

- Container security protects applications throughout the **container lifecycle**.
- Always use **trusted, minimal, and regularly updated images**.
- Avoid running containers as the **root user**.
- Scan container images before deployment and monitor containers during runtime.
- Container security is a critical part of every modern DevSecOps pipeline.
