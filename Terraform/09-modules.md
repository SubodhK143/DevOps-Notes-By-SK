# 🚀 Terraform Modules

> Learn how **Terraform Modules** help organize, reuse, and manage infrastructure efficiently.

---

# 📚 Table of Contents

- [What are Modules?](#-what-are-modules)
- [Why Use Modules?](#-why-use-modules)
- [Module Structure](#-module-structure)
- [Root and Child Modules](#-root-and-child-modules)
- [Using a Module](#-using-a-module)
- [Module Inputs](#-module-inputs)
- [Module Outputs](#-module-outputs)
- [Best Practices](#-best-practices)
- [Key Takeaways](#-key-takeaways)

---

## 📦 What are Modules?

A **Module** is a collection of Terraform configuration files that work together to perform a specific task.

Modules help organize and reuse infrastructure instead of writing the same code repeatedly.

Examples:

- EC2 Module
- VPC Module
- S3 Module
- Security Group Module

> 💡 Every Terraform project has a **Root Module**, and you can create additional **Child Modules**.

---

## 🤔 Why Use Modules?

Without modules, you may repeat the same resource configuration across multiple projects.

With modules:

- Reuse code
- Reduce duplication
- Keep projects organized
- Simplify maintenance

---

## 📂 Module Structure

A typical module contains:

```text
modules/
└── ec2/
    ├── main.tf
    ├── variables.tf
    └── outputs.tf
```

Each file has a specific purpose:

- **main.tf** → Resources
- **variables.tf** → Input variables
- **outputs.tf** → Output values

---

## 🌳 Root and Child Modules

Terraform always starts from the **Root Module**.

When the Root Module calls another module, it becomes a **Child Module**.

```text
Root Module
      │
      ▼
 Child Module
      │
      ▼
 AWS Resources
```

---

## ⚙️ Using a Module

Call a module using the `module` block.

```hcl
module "ec2" {
  source = "./modules/ec2"

  instance_type = "t2.micro"
}
```

| Field | Description |
|-------|-------------|
| `module` | Terraform keyword |
| `ec2` | Module name |
| `source` | Module location |

---

## 📥 Module Inputs

Modules receive values through input variables.

Example:

```hcl
module "ec2" {
  source = "./modules/ec2"

  instance_type = "t2.micro"
}
```

The child module uses this value through a variable.

---

## 📤 Module Outputs

Modules can return values using outputs.

Example:

```hcl
output "instance_id" {
  value = aws_instance.web.id
}
```

The Root Module can use these outputs after deployment.

---

## ⭐ Best Practices

- Create small, reusable modules.
- Keep one purpose per module.
- Use variables instead of hardcoding values.
- Return useful information with outputs.
- Organize modules inside a `modules/` directory.

---

## 📝 Key Takeaways

After completing this chapter, you should understand:

- ✅ What Modules are
- ✅ Why Modules are useful
- ✅ Module structure
- ✅ Root and Child Modules
- ✅ Using Modules
- ✅ Module inputs
- ✅ Module outputs
- ✅ Best practices

---

## 📖 Next Topic

➡️ **10 - Terraform Backends & Remote State**

In the next chapter, you'll learn:

- What are Backends?
- Local vs Remote State
- Remote State in AWS S3
- State Locking
- Best practices
