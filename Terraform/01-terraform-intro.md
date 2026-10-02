# 🚀 Terraform & Infrastructure as Code (IaC)

> Learn the fundamentals of **Terraform** and **Infrastructure as Code (IaC)**—the modern approach to provisioning, managing, and version-controlling cloud infrastructure.

---

# 📚 Table of Contents

- [What is Terraform?](#-what-is-terraform)
- [What is Infrastructure as Code (IaC)?](#-what-is-infrastructure-as-code-iac)
- [Why Infrastructure as Code Matters](#-why-infrastructure-as-code-matters)
- [Problems Solved by IaC](#-problems-solved-by-iac)
- [Why Terraform?](#-why-terraform)
- [Terraform Architecture](#-terraform-architecture)
- [Terraform Core Concepts](#-terraform-core-concepts)
- [Terraform Lifecycle](#-terraform-lifecycle)
- [Terraform Workflow](#-terraform-workflow)
- [Terraform vs Ansible](#-terraform-vs-ansible)
- [Terraform vs AWS CloudFormation](#-terraform-vs-aws-cloudformation)
- [Terraform vs OpenTofu](#-terraform-vs-opentofu)
- [Where Terraform Fits in Modern DevOps](#-where-terraform-fits-in-modern-devops)
- [Terraform State File](#-terraform-state-file)
- [Terraform Plan Symbols](#-terraform-plan-symbols)
- [Best Practices](#-best-practices)
- [Key Takeaways](#-key-takeaways)

---

# 🌍 What is Terraform?

**Terraform** is an **Infrastructure as Code (IaC)** tool developed by **HashiCorp** that allows you to provision, update, and manage infrastructure using code.

Instead of manually creating resources through cloud consoles, Terraform automates infrastructure deployment using configuration files written in **HashiCorp Configuration Language (HCL)**.

Terraform can manage infrastructure across hundreds of platforms using providers.

Examples include:

- AWS
- Microsoft Azure
- Google Cloud Platform (GCP)
- Oracle Cloud
- VMware
- Kubernetes
- GitHub
- Cloudflare
- DigitalOcean

---

# ☁️ What is Infrastructure as Code (IaC)?

Infrastructure as Code (IaC) is the practice of defining and managing infrastructure using code instead of manually creating resources.

Infrastructure includes:

- Virtual Machines
- Servers
- Networks
- VPCs
- Databases
- Load Balancers
- Storage
- IAM Users & Roles
- Kubernetes Clusters
- DNS Records

---

## Traditional Approach

```
Login
   │
   ▼
AWS Console
   │
   ▼
Create Resources Manually
```

---

## Infrastructure as Code

```
Write Terraform Code
        │
        ▼
terraform apply
        │
        ▼
Infrastructure Created Automatically
```

---

# ✅ Why Infrastructure as Code Matters

Modern cloud environments contain hundreds or even thousands of resources.

Managing them manually is slow, error-prone, and difficult to reproduce.

IaC helps by:

- Automating infrastructure deployment
- Reducing deployment time
- Creating consistent environments
- Tracking infrastructure in Git
- Improving team collaboration
- Reducing human error
- Scaling infrastructure easily
- Reusing infrastructure

---

# ❌ Problems Solved by IaC

| Manual Infrastructure | Infrastructure as Code |
|-----------------------|------------------------|
| Manual clicking | Automated deployment |
| Slow provisioning | Fast provisioning |
| Human errors | Consistent infrastructure |
| Difficult to reproduce | Easily reproducible |
| No version history | Git version control |
| Hard to scale | Highly scalable |

---

# 🚀 Why Terraform?

Terraform has become one of the most popular Infrastructure as Code tools because it is:

- Multi-cloud
- Declarative
- Idempotent
- Modular
- Team-friendly
- Open ecosystem with hundreds of providers
- Easy to integrate into CI/CD pipelines

---

# 🏗️ Terraform Architecture

```
Terraform Configuration (.tf)
              │
              ▼
        Terraform Core
              │
              ▼
         Provider Plugin
              │
              ▼
Cloud Platform APIs
              │
              ▼
AWS / Azure / GCP / Kubernetes
```

### Components

### Terraform Core

- Reads configuration
- Creates execution plan
- Tracks infrastructure state

### Provider

Providers allow Terraform to communicate with cloud platforms.

Examples:

- AWS
- Azure
- Google Cloud
- Kubernetes
- GitHub

---

# 🧠 Terraform Core Concepts

## Declarative

You describe **what** infrastructure you want.

Terraform determines **how** to create it.

Example:

```
Create one EC2 Instance.
```

Terraform automatically calculates the required steps.

---

## Providers

Providers connect Terraform with external platforms.

Examples:

- AWS Provider
- Azure Provider
- Google Provider
- Kubernetes Provider

---

## Resources

Resources are the actual infrastructure objects Terraform manages.

Examples:

- EC2 Instance
- S3 Bucket
- VPC
- IAM User
- Security Group

---

## Cloud Agnostic

The same Terraform workflow works across different cloud providers.

Only the provider changes.

---

# 🔄 Terraform Lifecycle

Terraform follows a predictable lifecycle.

```
Write Code
     │
     ▼
terraform init
     │
     ▼
terraform validate
     │
     ▼
terraform plan
     │
     ▼
terraform apply
     │
     ▼
Infrastructure Running
     │
     ▼
terraform destroy
```

### init

Downloads providers and initializes the project.

### validate

Checks configuration syntax.

### plan

Shows what changes Terraform will make.

### apply

Creates or updates infrastructure.

### destroy

Deletes managed infrastructure.

---

# ⚙️ Terraform Workflow

```
Developer
      │
      ▼
Write Terraform Code
      │
      ▼
Git Repository
      │
      ▼
Terraform Init
      │
      ▼
Terraform Plan
      │
      ▼
Terraform Apply
      │
      ▼
Cloud Infrastructure
```

---

# ⚖️ Terraform vs Ansible

| Terraform | Ansible |
|------------|----------|
| Infrastructure provisioning | Configuration management |
| Uses HCL | Uses YAML |
| Declarative | Mostly procedural |
| Tracks infrastructure state | No state file |
| Creates infrastructure | Configures software |

---

# ☁️ Terraform vs AWS CloudFormation

| Terraform | CloudFormation |
|------------|----------------|
| Multi-cloud | AWS only |
| Uses HCL | Uses JSON / YAML |
| Hundreds of providers | AWS services only |
| Open ecosystem | AWS ecosystem |

---

# 🌱 Terraform vs OpenTofu

| Terraform | OpenTofu |
|------------|-----------|
| Developed by HashiCorp | Community-driven under the Linux Foundation |
| Uses the BUSL license for newer releases | Fully open-source (MPL 2.0) |
| Large ecosystem | Compatible with most Terraform configurations |

> OpenTofu is a community fork of Terraform that aims to remain fully open-source while maintaining compatibility with Terraform workflows.

---

# 🚀 Where Terraform Fits in Modern DevOps

```
Developer
     │
     ▼
GitHub
     │
     ▼
CI/CD Pipeline
     │
     ▼
Terraform
     │
     ▼
Cloud Infrastructure
     │
     ▼
Docker
     │
     ▼
Kubernetes
     │
     ▼
Monitoring
```

Terraform is responsible for **provisioning infrastructure**.

Other tools deploy applications, configure servers, orchestrate containers, and monitor workloads.

---

# 📂 Terraform State File

Terraform stores information about managed infrastructure in a **state file**.

```
terraform.tfstate
```

The state file contains:

- Resource IDs
- Infrastructure metadata
- Resource dependencies
- Current resource values

### Why is it important?

- Tracks deployed infrastructure
- Prevents duplicate resource creation
- Detects infrastructure drift
- Calculates future changes

---

### Never Do These

- ❌ Edit the state file manually
- ❌ Commit state files to GitHub
- ❌ Share state files publicly
- ❌ Delete state files accidentally

---

# 🔍 Terraform Plan Symbols

| Symbol | Meaning |
|----------|---------|
| `+` | Create |
| `-` | Destroy |
| `~` | Update |
| `-/+` | Replace |

Example:

```
~ aws_instance.web

Name:
Old → WebServer
New → ProductionServer
```

---

# ⭐ Best Practices

- Use Git for version control.
- Run `terraform fmt` before committing.
- Always review `terraform plan`.
- Never hardcode secrets.
- Use variables for reusable values.
- Store state remotely for team projects.
- Organize infrastructure into modules.
- Follow consistent naming conventions.
- Add comments for complex configurations.
- Keep modules focused on a single responsibility.

---

# 📝 Key Takeaways

After completing this chapter, you should understand:

- ✅ What Terraform is
- ✅ What Infrastructure as Code (IaC) means
- ✅ Why IaC is important
- ✅ Why Terraform is widely used
- ✅ Terraform architecture
- ✅ Providers and resources
- ✅ Terraform lifecycle
- ✅ Terraform workflow
- ✅ Terraform vs Ansible
- ✅ Terraform vs CloudFormation
- ✅ Terraform vs OpenTofu
- ✅ Terraform's role in DevOps
- ✅ Terraform state file
- ✅ Terraform plan symbols
- ✅ Terraform best practices

---

# 📖 Next Topic

➡️ **02 - Terraform Installation & Setup**

In the next chapter, you'll learn how to:

- Install Terraform
- Install AWS CLI
- Configure AWS credentials
- Verify the installation
- Create your first Terraform project
