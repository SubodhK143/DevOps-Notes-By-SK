# 🚀 Terraform Interview Questions

> A quick revision guide covering the most commonly asked **Terraform interview questions** for beginners.

---

# 📚 Table of Contents

- [Terraform Basics](#-terraform-basics)
- [State & Backends](#-state--backends)
- [Modules & Variables](#-modules--variables)
- [Commands](#-commands)
- [Best Practices](#-best-practices)

---

# 🌱 Terraform Basics

### 1. What is Terraform?

Terraform is an **Infrastructure as Code (IaC)** tool that allows you to provision and manage infrastructure using code.

---

### 2. What is Infrastructure as Code (IaC)?

IaC is the practice of managing infrastructure using configuration files instead of manual processes.

---

### 3. What is a Provider?

A Provider is a plugin that allows Terraform to communicate with cloud platforms like AWS, Azure, GCP, and Kubernetes.

---

### 4. What is a Resource?

A Resource represents an infrastructure component managed by Terraform, such as an EC2 instance, S3 bucket, or VPC.

---

### 5. What is a Module?

A Module is a reusable collection of Terraform configuration files.

---

# 📦 State & Backends

### 6. What is Terraform State?

Terraform State is a file (`terraform.tfstate`) that stores information about the infrastructure managed by Terraform.

---

### 7. Why is the State file important?

It helps Terraform:

- Track resources
- Detect changes
- Plan updates
- Prevent duplicate resource creation

---

### 8. What is a Backend?

A Backend determines where Terraform stores its state file.

Examples:

- Local Backend
- AWS S3 Backend

---

### 9. Why use a Remote Backend?

A Remote Backend provides:

- Shared state
- Team collaboration
- Better security
- State locking

---

# 📥 Modules & Variables

### 10. What are Variables?

Variables allow you to pass input values into Terraform configurations.

---

### 11. What are Outputs?

Outputs display useful information after Terraform creates resources.

Example:

- Public IP
- Instance ID
- VPC ID

---

### 12. What are Local Values?

Locals store reusable values within a Terraform configuration.

---

### 13. What are Data Sources?

Data Sources retrieve information about existing infrastructure without creating new resources.

---

# ⚙️ Commands

### 14. What does `terraform init` do?

Initializes the project and downloads required Providers.

---

### 15. What does `terraform plan` do?

Shows the changes Terraform will make without applying them.

---

### 16. What does `terraform apply` do?

Creates or updates infrastructure.

---

### 17. What does `terraform destroy` do?

Deletes all resources managed by Terraform.

---

### 18. What does `terraform validate` do?

Checks whether the Terraform configuration is syntactically valid.

---

### 19. What does `terraform fmt` do?

Formats Terraform files according to standard style guidelines.

---

# ⭐ Best Practices

### 20. Name a few Terraform best practices.

- Use meaningful resource names.
- Store code in Git.
- Never commit `terraform.tfstate`.
- Use Remote Backends for teams.
- Pin Provider versions.
- Review changes with `terraform plan`.
- Keep configurations modular.
- Avoid hardcoding values.

---

# 📝 Quick Revision

| Question | Short Answer |
|----------|--------------|
| What is Terraform? | Infrastructure as Code tool |
| What is IaC? | Manage infrastructure using code |
| What is a Provider? | Communicates with cloud APIs |
| What is a Resource? | Infrastructure component |
| What is a Module? | Reusable Terraform code |
| What is State? | Infrastructure record file |
| What is a Backend? | Stores Terraform State |
| What is a Variable? | Input value |
| What is an Output? | Displays resource information |
| What is a Local? | Reusable local value |
| What is a Data Source? | Reads existing infrastructure |
| `terraform init` | Initialize project |
| `terraform plan` | Preview changes |
| `terraform apply` | Create/update resources |
| `terraform destroy` | Delete resources |

---

# 🎯 Interview Tips

- Understand **why** Terraform uses a state file.
- Know the difference between **Resources** and **Data Sources**.
- Explain **Variables**, **Outputs**, and **Modules** with examples.
- Practice the Terraform workflow:

```text
Write Code
     │
     ▼
terraform init
     │
     ▼
terraform plan
     │
     ▼
terraform apply
     │
     ▼
terraform destroy
```

- Be comfortable explaining the purpose of common Terraform commands.

---

# 📖 Next Topic

➡️ **17 - Terraform Cheat Sheet**

A one-page reference containing:

- Common commands
- File structure
- Resource syntax
- Variables
- Outputs
- State
- Modules
- Meta Arguments
- Best practices
