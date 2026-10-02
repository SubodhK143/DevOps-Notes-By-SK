# 🚀 Terraform Meta Arguments

> Learn how **Meta Arguments** control how Terraform creates, updates, and manages resources.

---

# 📚 Table of Contents

- [What are Meta Arguments?](#-what-are-meta-arguments)
- [Why Use Meta Arguments?](#-why-use-meta-arguments)
- [Common Meta Arguments](#-common-meta-arguments)
- [count](#-count)
- [for_each](#-for_each)
- [depends_on](#-depends_on)
- [provider](#-provider)
- [lifecycle](#-lifecycle)
- [Best Practices](#-best-practices)
- [Key Takeaways](#-key-takeaways)

---

## ⚙️ What are Meta Arguments?

**Meta Arguments** are special arguments that control how Terraform manages resources.

Unlike normal resource arguments (such as `ami` or `instance_type`), Meta Arguments affect Terraform's behavior rather than the resource itself.

> 💡 Meta Arguments help automate resource creation, dependencies, and lifecycle management.

---

## 🤔 Why Use Meta Arguments?

Meta Arguments make Terraform configurations more flexible and reusable.

Benefits:

- Create multiple resources easily
- Manage dependencies
- Use different provider configurations
- Control how resources are created or destroyed

---

## 📦 Common Meta Arguments

Terraform provides several Meta Arguments:

| Meta Argument | Purpose |
|---------------|---------|
| `count` | Create multiple identical resources |
| `for_each` | Create resources from a list or map |
| `depends_on` | Define explicit dependencies |
| `provider` | Use a specific Provider configuration |
| `lifecycle` | Control create, update, and delete behavior |

---

## 🔢 count

Use `count` to create multiple copies of the same resource.

```hcl
resource "aws_instance" "web" {
  count         = 2
  ami           = "ami-0f58b397bc5c1f2e8"
  instance_type = "t2.micro"
}
```

Terraform creates **2 EC2 instances**.

---

## 🔁 for_each

Use `for_each` to create resources from a collection.

```hcl
resource "aws_s3_bucket" "bucket" {
  for_each = toset(["dev", "prod"])

  bucket = "company-${each.key}"
}
```

Terraform creates one bucket for each value.

---

## 🔗 depends_on

Use `depends_on` when a resource must wait for another resource.

```hcl
resource "aws_instance" "web" {
  depends_on = [aws_security_group.web]

  ami           = "ami-0f58b397bc5c1f2e8"
  instance_type = "t2.micro"
}
```

Terraform creates the Security Group before the EC2 instance.

---

## 🌐 provider

Use the `provider` Meta Argument when multiple Provider configurations exist.

```hcl
resource "aws_s3_bucket" "backup" {
  provider = aws.singapore

  bucket = "company-backup"
}
```

Terraform uses the **Singapore AWS Provider** for this resource.

---

## 🔒 lifecycle

The `lifecycle` Meta Argument controls how Terraform manages resource changes.

Example:

```hcl
resource "aws_instance" "web" {
  lifecycle {
    prevent_destroy = true
  }
}
```

This prevents Terraform from accidentally deleting the resource.

---

## ⭐ Best Practices

- Use `count` for identical resources.
- Use `for_each` when resources have unique values.
- Avoid unnecessary `depends_on`.
- Use `provider` for multi-region or multi-account deployments.
- Use `lifecycle` carefully to protect important resources.

---

## 📝 Key Takeaways

After completing this chapter, you should understand:

- ✅ What Meta Arguments are
- ✅ Why Meta Arguments are useful
- ✅ `count`
- ✅ `for_each`
- ✅ `depends_on`
- ✅ `provider`
- ✅ `lifecycle`
- ✅ Best practices

---

## 📖 Next Topic

➡️ **13 - Terraform Functions & Expressions**

In the next chapter, you'll learn:

- Common Terraform functions
- String functions
- Collection functions
- Expressions
- Conditional expressions
- Best practices
