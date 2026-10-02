# 🚀 Terraform Resources

> Learn how **Terraform Resources** represent cloud infrastructure and how to create, configure, and manage them using Terraform.

---

# 📚 Table of Contents

- [What is a Resource?](#-what-is-a-resource)
- [Resource Syntax](#-resource-syntax)
- [Resource Block](#-resource-block)
- [Common Resource Types](#-common-resource-types)
- [Resource Arguments](#-resource-arguments)
- [Resource Attributes](#-resource-attributes)
- [Referencing Resources](#-referencing-resources)
- [Resource Dependencies](#-resource-dependencies)
- [Meta Arguments](#-meta-arguments)
- [Best Practices](#-best-practices)
- [Key Takeaways](#-key-takeaways)

---

## 🌍 What is a Resource?

A **Resource** is the basic building block of Terraform.

It represents an infrastructure component that Terraform creates and manages.

Examples include:

- EC2 Instance
- S3 Bucket
- VPC
- Security Group
- IAM User
- RDS Database

> 💡 Every cloud resource in Terraform is defined using a **resource block**.

---

## 📝 Resource Syntax

Every resource follows the same structure.

```hcl
resource "<RESOURCE_TYPE>" "<LOCAL_NAME>" {

}
```

Example:

```hcl
resource "aws_instance" "web" {

}
```

| Part | Description |
|------|-------------|
| `resource` | Terraform keyword |
| `aws_instance` | Resource type (what Terraform creates) |
| `web` | Local name used to reference the resource |

---

## 🧩 Resource Block

A resource block contains the configuration needed to create a resource.

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0f58b397bc5c1f2e8"
  instance_type = "t2.micro"

  tags = {
    Name = "WebServer"
  }
}
```

> **Note:** Replace the AMI ID with one that is valid for your AWS region.

---

## ☁️ Common Resource Types

| AWS Service | Resource Type |
|-------------|---------------|
| EC2 Instance | `aws_instance` |
| S3 Bucket | `aws_s3_bucket` |
| VPC | `aws_vpc` |
| Security Group | `aws_security_group` |
| IAM User | `aws_iam_user` |

---

## ⚙️ Resource Arguments

Arguments define **how** Terraform should create a resource.

Common arguments include:

- AMI
- Instance Type
- Tags
- Region
- Security Groups

Example:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0f58b397bc5c1f2e8"
  instance_type = "t2.micro"
}
```

---

## 📌 Resource Attributes

After a resource is created, Terraform provides useful information called **attributes**.

Examples:

- Instance ID
- Public IP
- ARN
- DNS Name

Example:

```hcl
aws_instance.web.public_ip
```

Resource attributes are values generated after a resource is created. These values can be referenced by other resources.

---

## 🔗 Referencing Resources

Resources can reference one another.

Example:

```hcl
resource "aws_security_group" "web" {
  name = "web-sg"
}

resource "aws_instance" "web" {
  ami                    = "ami-0f58b397bc5c1f2e8"
  instance_type          = "t2.micro"
  vpc_security_group_ids = [aws_security_group.web.id]
}
```

Terraform automatically detects the dependency through the resource reference.

---

## 🔄 Resource Dependencies

Terraform creates resources in the correct order.

```text
VPC
 │
 ▼
Subnet
 │
 ▼
Security Group
 │
 ▼
EC2 Instance
```

When one resource references another, Terraform automatically creates them in the correct order.

---

## ⚡ Meta Arguments

Meta arguments modify how Terraform creates and manages resources. They are supported by most resource types.

| Meta Argument | Purpose |
|---------------|---------|
| `count` | Create multiple resources |
| `for_each` | Create resources from a list or map |
| `depends_on` | Define explicit dependencies |
| `provider` | Select a specific Provider |
| `lifecycle` | Control resource behavior |

---

## ⭐ Best Practices

- Use meaningful resource names.
- Add tags to resources.
- Prefer automatic dependencies.
- Keep resource blocks simple.
- Review changes using `terraform plan` before applying.

---

## 📝 Key Takeaways

After completing this chapter, you should understand:

- ✅ What a Resource is
- ✅ Resource syntax
- ✅ Resource blocks
- ✅ Resource arguments
- ✅ Resource attributes
- ✅ Resource references
- ✅ Resource dependencies
- ✅ Meta arguments
- ✅ Best practices

---

## 📖 Next Topic

➡️ **05 - Terraform Variables**

In the next chapter, you'll learn:

- What are Variables?
- Variable types
- Default values
- Input variables
- `terraform.tfvars`
- Variable best practices
