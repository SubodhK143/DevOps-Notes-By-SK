# 🚀 Terraform Variables

> Learn how **Terraform Variables** make your configurations flexible, reusable, and easier to manage.

---

# 📚 Table of Contents

- [What are Variables?](#-what-are-variables)
- [Why Use Variables?](#-why-use-variables)
- [Variable Syntax](#-variable-syntax)
- [Using Variables](#-using-variables)
- [Variable Types](#-variable-types)
- [Default Values](#-default-values)
- [Variable Values (`terraform.tfvars`)](#-variable-values-terraformtfvars)
- [Environment Variables](#-environment-variables)
- [Best Practices](#-best-practices)
- [Key Takeaways](#-key-takeaways)

---

## 📦 What are Variables?

Variables allow you to store values that can be reused throughout your Terraform configuration.

Instead of hardcoding values, you define them once and use them wherever needed.

Example values:

- AWS Region
- EC2 Instance Type
- AMI ID
- Bucket Name
- Project Name

> 💡 Variables make Terraform code reusable and easier to maintain.

---

## 🤔 Why Use Variables?

Without variables:

```hcl
instance_type = "t2.micro"
```

With variables:

```hcl
instance_type = var.instance_type
```

Benefits:

- Reusable code
- Easy configuration changes
- Less duplication
- Better readability

---

## 📝 Variable Syntax

Define a variable using the `variable` block.

```hcl
variable "instance_type" {
  type = string
}
```

| Part | Description |
|------|-------------|
| `variable` | Terraform keyword |
| `instance_type` | Variable name |
| `type` | Defines the value type |

---

## ⚙️ Using Variables

Reference variables with the `var` keyword.

```hcl
resource "aws_instance" "web" {
  instance_type = var.instance_type
}
```

Terraform automatically replaces the variable with its value.

---

## 📌 Variable Types

Terraform supports different variable types.

| Type | Example |
|------|---------|
| `string` | `"t2.micro"` |
| `number` | `2` |
| `bool` | `true` |
| `list` | `["dev","prod"]` |
| `map` | `{ env = "dev" }` |

---

## 🎯 Default Values

You can assign a default value to a variable.

```hcl
variable "instance_type" {
  type    = string
  default = "t2.micro"
}
```

If no value is provided, Terraform uses the default.

---

## 📄 Variable Values (`terraform.tfvars`)

Store variable values in a `terraform.tfvars` file.

```hcl
instance_type = "t3.micro"
```

Terraform automatically loads this file during execution.

---

## 🌐 Environment Variables

Variables can also be passed using environment variables.

```bash
export TF_VAR_instance_type=t2.micro
```

Terraform automatically recognizes variables prefixed with `TF_VAR_`.

---

## ⭐ Best Practices

- Use meaningful variable names.
- Provide default values when appropriate.
- Store variable values in `terraform.tfvars`.
- Avoid hardcoding values.
- Keep sensitive values out of your code.

---

## 📝 Key Takeaways

After completing this chapter, you should understand:

- ✅ What Variables are
- ✅ Why Variables are used
- ✅ Variable syntax
- ✅ Variable types
- ✅ Default values
- ✅ `terraform.tfvars`
- ✅ Environment variables
- ✅ Best practices

---

## 📖 Next Topic

➡️ **06 - Terraform Outputs**

In the next chapter, you'll learn:

- What are Outputs?
- Output syntax
- Referencing resource attributes
- Common use cases
- Best practices
