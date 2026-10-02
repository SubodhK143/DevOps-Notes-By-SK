# 🚀 Terraform Workspaces

> Learn how **Terraform Workspaces** help manage multiple environments using the same Terraform configuration.

---

# 📚 Table of Contents

- [What are Workspaces?](#-what-are-workspaces)
- [Why Use Workspaces?](#-why-use-workspaces)
- [How Workspaces Work](#-how-workspaces-work)
- [Workspace Commands](#-workspace-commands)
- [Using Workspaces](#-using-workspaces)
- [Best Practices](#-best-practices)
- [Key Takeaways](#-key-takeaways)

---

## 📦 What are Workspaces?

A **Workspace** is an isolated environment that maintains its own Terraform state.

Using Workspaces, you can manage multiple environments (such as **Development**, **Testing**, and **Production**) with the same Terraform configuration.

Examples:

- Development
- Testing
- Staging
- Production

> 💡 Each workspace has its own **terraform.tfstate**, allowing environments to remain independent.

---

## 🤔 Why Use Workspaces?

Without Workspaces, you may need separate Terraform projects for each environment.

With Workspaces, you can reuse the same configuration while keeping each environment's state separate.

Benefits:

- Manage multiple environments
- Reuse the same code
- Keep state files isolated
- Simplify infrastructure management

---

## 🔄 How Workspaces Work

```text
Terraform Configuration
           │
           ▼
     Workspace: dev
           │
           ▼
terraform.tfstate (dev)

----------------------------

Terraform Configuration
           │
           ▼
    Workspace: prod
           │
           ▼
terraform.tfstate (prod)
```

Each workspace uses the same Terraform code but maintains a separate state file.

---

## ⚙️ Workspace Commands

Create a workspace:

```bash
terraform workspace new dev
```

List all workspaces:

```bash
terraform workspace list
```

Switch to a workspace:

```bash
terraform workspace select dev
```

Show the current workspace:

```bash
terraform workspace show
```

Delete a workspace:

```bash
terraform workspace delete dev
```

---

## 🛠 Using Workspaces

Example:

```bash
terraform workspace new dev
terraform apply
```

Switch to another environment:

```bash
terraform workspace new prod
terraform apply
```

Terraform creates separate state files for each workspace while using the same configuration.

---

## ⭐ Best Practices

- Use meaningful workspace names.
- Use Workspaces for environment separation.
- Keep Production and Development isolated.
- Verify the active workspace before running `terraform apply`.
- Use Remote Backends for team projects.

---

## 📝 Key Takeaways

After completing this chapter, you should understand:

- ✅ What Workspaces are
- ✅ Why Workspaces are useful
- ✅ How Workspaces work
- ✅ Common workspace commands
- ✅ Managing multiple environments
- ✅ Best practices

---

## 📖 Next Topic

➡️ **12 - Terraform Meta Arguments**

In the next chapter, you'll learn:

- What are Meta Arguments?
- `count`
- `for_each`
- `depends_on`
- `provider`
- `lifecycle`
- Best practices
