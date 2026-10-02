# Secrets Management

## What is Secrets Management?

**Secrets Management** is the practice of securely storing, managing, and accessing sensitive information used by applications and infrastructure.

Secrets should **never** be hardcoded into source code or configuration files.

### Common Secrets

- API Keys
- Database Passwords
- SSH Keys
- Access Tokens
- OAuth Tokens
- SSL/TLS Certificates

---

## Why is Secrets Management Important?

Hardcoding secrets in code can expose sensitive information if the repository is leaked or shared.

Proper secrets management helps:

- Protect sensitive credentials
- Prevent unauthorized access
- Reduce the risk of data breaches
- Rotate secrets easily
- Meet security and compliance requirements

---

## How Secrets Management Works

```
Developer
      │
      ▼
Store Secret
      │
      ▼
Secret Manager
      │
      ▼
Application Requests Secret
      │
      ▼
Authenticate
      │
      ▼
Secret Returned Securely
```

Instead of storing credentials in code, applications retrieve them securely at runtime.

---

## Where Should Secrets Be Stored?

| Platform | Secret Storage |
|----------|----------------|
| GitHub | GitHub Secrets |
| AWS | AWS Secrets Manager |
| Azure | Azure Key Vault |
| Google Cloud | Secret Manager |
| Kubernetes | Kubernetes Secrets |
| HashiCorp | Vault |

---

## GitHub Secrets Example

❌ Hardcoded Secret

```yaml
env:
  DB_PASSWORD: mypassword123
```

✅ Using GitHub Secrets

```yaml
env:
  DB_PASSWORD: ${{ secrets.DB_PASSWORD }}
```

GitHub encrypts the secret and masks it in workflow logs.

---

## AWS Secrets Manager Example

Instead of storing credentials in your application:

```text
DB_PASSWORD = mypassword123
```

Store the password in **AWS Secrets Manager** and retrieve it securely using the AWS SDK when the application starts.

---

## Best Practices

- Never hardcode secrets
- Never commit `.env` files to Git
- Use environment variables
- Rotate secrets regularly
- Grant least privilege access
- Encrypt secrets at rest and in transit
- Audit secret access
- Remove unused credentials

---

## Common Mistakes

❌ Hardcoding passwords

❌ Committing `.env` files

❌ Sharing secrets through email or chat

❌ Using the same password everywhere

❌ Giving unnecessary access to secrets

---

## Popular Secrets Management Tools

| Tool | Purpose |
|------|---------|
| GitHub Secrets | CI/CD secrets |
| AWS Secrets Manager | Cloud secrets management |
| HashiCorp Vault | Enterprise secrets management |
| Azure Key Vault | Azure cloud secrets |
| Google Secret Manager | GCP secrets management |
| Kubernetes Secrets | Store cluster secrets |

---

## Quick Summary

| Topic | Description |
|--------|-------------|
| Purpose | Securely store and manage sensitive information |
| Examples | Passwords, API Keys, Tokens, Certificates |
| Benefits | Encryption, access control, secret rotation |
| GitHub | GitHub Secrets |
| AWS | AWS Secrets Manager |
| Best Practice | Never hardcode secrets |

---

## Key Takeaways

- Secrets should **never be stored in source code**.
- Use dedicated **Secrets Management** solutions instead of plain text files.
- Applications should retrieve secrets securely at runtime.
- Regular secret rotation and least privilege access improve security.
