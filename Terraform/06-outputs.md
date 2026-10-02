# 🚀 Terraform Outputs

> Learn how **Terraform Outputs** display useful information about your infrastructure after it is created.

---

# 📚 Table of Contents

- [What are Outputs?](#-what-are-outputs)
- [Why Use Outputs?](#-why-use-outputs)
- [Output Syntax](#-output-syntax)
- [Using Outputs](#-using-outputs)
- [Viewing Outputs](#-viewing-outputs)
- [Sensitive Outputs](#-sensitive-outputs)
- [Best Practices](#-best-practices)
- [Key Takeaways](#-key-takeaways)

---

## 📤 What are Outputs?

Outputs display values from your Terraform infrastructure after resources are created.

They make it easy to access important information without searching in the cloud console.

Common examples include:

- EC2 Public IP
- Instance ID
- S3 Bucket Name
- VPC ID
- Load Balancer DNS Name

> 💡 Outputs help you quickly retrieve information created by Terraform.

---

## 🤔 Why Use Outputs?

Without outputs, you may need to manually look up resource details.

With outputs, Terraform displays them automatically after `terraform apply`.

Example:

```text
EC2 Public IP = 13.233.xxx.xxx
```

Benefits:

- Quick access to resource information
- Easy integration with other configurations
- Simplifies verification after deployment

---

## 📝 Output Syntax

Define an output using the `output` block.

```hcl
output "instance_public_ip" {
  value = aws_instance.web.public_ip
}
```

| Part | Description |
|------|-------------|
| `output` | Terraform keyword |
| `instance_public_ip` | Output name |
| `value` | Value to display |

---

## ⚙️ Using Outputs

Example:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0f58b397bc5c1f2e8"
  instance_type = "t2.micro"
}

output "public_ip" {
  value = aws_instance.web.public_ip
}
```

After running:

```bash
terraform apply
```

Terraform displays:

```text
public_ip = 13.233.xxx.xxx
```

---

## 👀 Viewing Outputs

Display all outputs:

```bash
terraform output
```

Display a specific output:

```bash
terraform output public_ip
```

---

## 🔒 Sensitive Outputs

Some outputs contain sensitive information such as passwords or access keys.

Mark them as sensitive to prevent them from being displayed in normal output.

```hcl
output "db_password" {
  value     = var.db_password
  sensitive = true
}
```

> ⚠️ Use sensitive outputs for confidential information only.

---

## ⭐ Best Practices

- Use meaningful output names.
- Output only useful resource information.
- Mark sensitive values as `sensitive = true`.
- Avoid exposing secrets unnecessarily.
- Keep outputs simple and descriptive.

---

## 📝 Key Takeaways

After completing this chapter, you should understand:

- ✅ What Outputs are
- ✅ Why Outputs are used
- ✅ Output syntax
- ✅ Viewing outputs
- ✅ Sensitive outputs
- ✅ Best practices

---

## 📖 Next Topic

➡️ **07 - Terraform State**

In the next chapter, you'll learn:

- What is Terraform State?
- State file (`terraform.tfstate`)
- State commands
- Remote state
- State locking
- Best practices
