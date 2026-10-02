# 🚀 Terraform State

> Learn how **Terraform State** tracks your infrastructure and helps Terraform manage resources efficiently.

---

# 📚 Table of Contents

- [What is Terraform State?](#-what-is-terraform-state)
- [Why is State Important?](#-why-is-state-important)
- [State File](#-state-file)
- [How Terraform Uses State](#-how-terraform-uses-state)
- [Common State Commands](#-common-state-commands)
- [Remote State](#-remote-state)
- [Best Practices](#-best-practices)
- [Key Takeaways](#-key-takeaways)

---

## 📦 What is Terraform State?

Terraform State is a file that stores information about the infrastructure managed by Terraform.

It keeps track of the resources Terraform has created and their current state.

By default, Terraform stores this information in:

```text
terraform.tfstate
```

> 💡 Terraform State acts as the **source of truth** for your infrastructure.

---

## 🤔 Why is State Important?

Terraform uses the state file to:

- Track existing resources
- Detect infrastructure changes
- Plan updates correctly
- Prevent duplicate resource creation

Without the state file, Terraform cannot determine what already exists.

---

## 📄 State File

The `terraform.tfstate` file stores information such as:

- Resource IDs
- Resource attributes
- Dependencies
- Current infrastructure configuration

> ⚠️ Never edit the state file manually.

---

## 🔄 How Terraform Uses State

```text
Terraform Code
       │
       ▼
terraform plan
       │
       ▼
Compare with
terraform.tfstate
       │
       ▼
Determine Changes
       │
       ▼
terraform apply
```

Terraform compares your configuration with the state file to decide what needs to be created, updated, or deleted.

---

## ⚙️ Common State Commands

Initialize the project:

```bash
terraform init
```

Show the current state:

```bash
terraform show
```

List managed resources:

```bash
terraform state list
```

Display details of a resource:

```bash
terraform state show aws_instance.web
```

---

## ☁️ Remote State

For team projects, store the state file remotely instead of on your local machine.

Common remote backends include:

- AWS S3
- Azure Storage
- Google Cloud Storage

Benefits:

- Shared state
- Better collaboration
- Improved security
- State locking support

---

## ⭐ Best Practices

- Never edit `terraform.tfstate` manually.
- Do not commit state files to Git.
- Use remote state for team projects.
- Protect sensitive information stored in the state file.
- Back up your state file regularly.

---

## 📝 Key Takeaways

After completing this chapter, you should understand:

- ✅ What Terraform State is
- ✅ Why State is important
- ✅ What `terraform.tfstate` stores
- ✅ How Terraform uses the state file
- ✅ Common state commands
- ✅ Remote State
- ✅ Best practices

---

## 📖 Next Topic

➡️ **08 - Locals & Data Sources**

In the next chapter, you'll learn:

- What are Local Values?
- What are Data Sources?
- When to use Locals
- When to use Data Sources
- Best practices
