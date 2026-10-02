# Policy as Code (PaC)

## What is Policy as Code?

**Policy as Code (PaC)** is the practice of defining and enforcing security, compliance, and operational policies using code instead of manual reviews.

Policies are written as code and automatically evaluated during development, CI/CD pipelines, or infrastructure deployment.

This ensures that applications and infrastructure follow organizational security standards before deployment.

---

## Why is Policy as Code Important?

Manual security reviews are time-consuming and prone to human error.

Policy as Code helps:

- Enforce security rules automatically
- Prevent insecure deployments
- Ensure compliance with standards
- Improve consistency across environments
- Detect policy violations early

---

## How Policy as Code Works

```
Developer
     │
     ▼
Code / Infrastructure
     │
     ▼
Policy Engine
(OPA / Conftest)
     │
     ▼
Evaluate Policies
     │
     ▼
Pass ✅        Fail ❌
     │             │
     ▼             ▼
Deploy        Fix Issues
```

The deployment proceeds only if all defined policies are satisfied.

---

## Common Policy Checks

- No public cloud storage buckets
- Containers must not run as root
- Resource limits must be defined
- Approved container images only
- Secrets must not be hardcoded
- Required security labels must exist
- Encryption must be enabled

---

## Example

Example policy:

```text
Rule:
"Containers must not run as the root user."
```

If a Kubernetes deployment contains:

```yaml
securityContext:
  runAsUser: 0
```

The policy engine blocks the deployment until the issue is fixed.

---

## Popular Policy as Code Tools

| Tool | Description |
|------|-------------|
| Open Policy Agent (OPA) | General-purpose policy engine |
| Conftest | Tests configuration files using OPA policies |
| Kyverno | Kubernetes-native policy management |
| Gatekeeper | Kubernetes policy enforcement using OPA |
| HashiCorp Sentinel | Policy framework for Terraform Enterprise |

---

## Where is Policy as Code Used?

- CI/CD pipelines
- Kubernetes clusters
- Terraform deployments
- Cloud infrastructure
- Docker container validation
- Compliance automation

---

## Advantages

- Automates policy enforcement
- Reduces manual reviews
- Prevents configuration mistakes
- Improves compliance
- Integrates easily with CI/CD

---

## Limitations

- Policies require ongoing maintenance
- Poorly written policies may block valid deployments
- Teams must understand policy rules
- Requires integration with existing workflows

---

## Best Practices

- Store policies in version control
- Review and update policies regularly
- Automate policy checks in CI/CD
- Start with simple policies and expand gradually
- Test policies before enforcing them
- Document policy exceptions when necessary

---

## Quick Summary

| Topic | Description |
|--------|-------------|
| Purpose | Enforce security and compliance using code |
| Used For | Infrastructure, Kubernetes, CI/CD |
| Common Checks | Secrets, root users, encryption, resource limits |
| Popular Tools | OPA, Conftest, Kyverno, Gatekeeper |
| Best Practice | Automate policy validation before deployment |

---

## Key Takeaways

- **Policy as Code** automates security and compliance enforcement.
- Policies are evaluated before applications or infrastructure are deployed.
- Tools like **OPA**, **Conftest**, and **Kyverno** help prevent insecure configurations.
- Integrating Policy as Code into CI/CD pipelines improves consistency and reduces deployment risks.
