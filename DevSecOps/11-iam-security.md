# Identity and Access Management (IAM) Security

## What is IAM Security?

**Identity and Access Management (IAM)** is the practice of managing **who can access resources** and **what actions they are allowed to perform**.

IAM ensures that only authorized users, applications, and services can access systems and data.

The core principle of IAM is **"Provide the right access to the right user at the right time."**

---

## Why is IAM Security Important?

Without proper access control, unauthorized users may gain access to sensitive resources, leading to data breaches or system compromise.

IAM helps:

- Protect sensitive resources
- Prevent unauthorized access
- Reduce security risks
- Enforce the Principle of Least Privilege (PoLP)
- Improve compliance and auditing

---

## How IAM Works

```
User / Application
         │
         ▼
Authentication
(Who are you?)
         │
         ▼
Authorization
(What can you do?)
         │
         ▼
Access Granted or Denied
         │
         ▼
Resource
```

Authentication verifies identity, while authorization determines the actions the authenticated user is permitted to perform.

---

## Core IAM Components

| Component | Description |
|-----------|-------------|
| Identity | A user, application, or service |
| Authentication | Verifies identity (password, MFA, certificates) |
| Authorization | Determines allowed actions |
| Roles | Collection of permissions assigned to identities |
| Permissions | Specific actions allowed on resources |
| Policies | Rules that define permissions |

---

## Principle of Least Privilege (PoLP)

The **Principle of Least Privilege (PoLP)** means granting only the minimum permissions required to perform a task.

### Example

❌ Administrator Access

```
User
 └── Full access to all resources
```

✅ Least Privilege

```
Developer
 └── Read S3 Bucket
 └── Deploy Application
 └── View Logs
```

Grant only the permissions that are necessary.

---

## Multi-Factor Authentication (MFA)

**Multi-Factor Authentication (MFA)** adds an extra layer of security by requiring two or more verification methods.

Examples:

- Password
- OTP (One-Time Password)
- Authenticator App
- Security Key
- Biometric Authentication

MFA significantly reduces the risk of unauthorized access.

---

## IAM Best Practices

- Follow the Principle of Least Privilege
- Enable Multi-Factor Authentication (MFA)
- Avoid using root accounts for daily tasks
- Use roles instead of sharing credentials
- Rotate access keys regularly
- Remove unused users and permissions
- Monitor login activity and access logs
- Review IAM policies periodically

---

## Popular IAM Services

| Platform | IAM Service |
|----------|-------------|
| AWS | AWS IAM |
| Microsoft Azure | Azure Active Directory (Microsoft Entra ID) |
| Google Cloud | Cloud IAM |
| Kubernetes | RBAC (Role-Based Access Control) |
| GitHub | Repository Roles & Teams |

---

## Advantages

- Controls access to resources
- Improves security
- Supports compliance
- Reduces insider threats
- Simplifies permission management

---

## Limitations

- Misconfigured permissions can create security risks
- Requires regular reviews
- Large environments may have complex policies
- Human errors can lead to excessive permissions

---

## Quick Summary

| Topic | Description |
|--------|-------------|
| Purpose | Manage identities and control access |
| Core Concepts | Authentication, Authorization, Roles, Policies |
| Security Principle | Least Privilege (PoLP) |
| Additional Protection | Multi-Factor Authentication (MFA) |
| Popular Services | AWS IAM, Azure Entra ID, Google Cloud IAM, Kubernetes RBAC |

---

## Key Takeaways

- IAM controls **who can access resources and what actions they can perform**.
- Authentication verifies identity, while authorization determines permissions.
- Always follow the **Principle of Least Privilege (PoLP)**.
- Enable **Multi-Factor Authentication (MFA)** for stronger account security.
- Regularly review and update IAM policies to reduce security risks.
