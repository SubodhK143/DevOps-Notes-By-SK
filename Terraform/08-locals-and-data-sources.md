# 🚀 Terraform Locals & Data Sources

> Learn how **Locals** help reduce code duplication and how **Data Sources** retrieve information from existing infrastructure.

---

# 📚 Table of Contents

- [What are Local Values?](#-what-are-local-values)
- [Why Use Locals?](#-why-use-locals)
- [Local Syntax](#-local-syntax)
- [Using Locals](#-using-locals)
- [What are Data Sources?](#-what-are-data-sources)
- [Data Source Syntax](#-data-source-syntax)
- [Using Data Sources](#-using-data-sources)
- [Locals vs Data Sources](#-locals-vs-data-sources)
- [Best Practices](#-best-practices)
- [Key Takeaways](#-key-takeaways)

---

## 📌 What are Local Values?

**Locals** are named values used to simplify and reuse expressions within your Terraform configuration.

They help avoid repeating the same value multiple times.

Example:

- Project Name
- Environment
- Common Tags
- Naming Prefix

> 💡 Local values improve readability and reduce duplicate code.

---

## 🤔 Why Use Locals?

Without locals:

```hcl
tags = {
  Project = "devops-project"
}
```

Repeated in multiple resources.

With locals:

```hcl
tags = local.common_tags
```

Benefits:

- Reusable values
- Cleaner code
- Easier maintenance
- Consistent configuration

---

## 📝 Local Syntax

Define locals using the `locals` block.

```hcl
locals {
  project_name = "devops-project"
}
```

Reference a local value using:

```hcl
local.project_name
```

---

## ⚙️ Using Locals

Example:

```hcl
locals {
  common_tags = {
    Project = "DevOps"
    Environment = "Development"
  }
}

resource "aws_instance" "web" {
  ami           = "ami-0f58b397bc5c1f2e8"
  instance_type = "t2.micro"

  tags = local.common_tags
}
```

Terraform replaces the local value wherever it is referenced.

---

## 📥 What are Data Sources?

A **Data Source** lets Terraform read information about resources that already exist.

Unlike resources, data sources **do not create infrastructure**.

Common examples:

- Existing AMI
- Existing VPC
- Existing Subnet
- Existing Security Group

> 💡 Data Sources fetch information instead of creating resources.

---

## 📝 Data Source Syntax

```hcl
data "aws_ami" "latest" {
  most_recent = true

  owners = ["amazon"]
}
```

| Part | Description |
|------|-------------|
| `data` | Terraform keyword |
| `aws_ami` | Data source type |
| `latest` | Local name |

---

## ⚙️ Using Data Sources

Example:

```hcl
data "aws_ami" "latest" {
  most_recent = true

  owners = ["amazon"]
}

resource "aws_instance" "web" {
  ami           = data.aws_ami.latest.id
  instance_type = "t2.micro"
}
```

Terraform retrieves the latest Amazon AMI and uses it to create the EC2 instance.

---

## 🔄 Locals vs Data Sources

| Local Values | Data Sources |
|--------------|--------------|
| Store reusable values | Read existing infrastructure |
| Defined in your code | Retrieved from cloud providers |
| Do not query APIs | Query cloud APIs |
| Improve readability | Reuse existing resources |

---

## ⭐ Best Practices

- Use locals to avoid duplicate values.
- Use meaningful local names.
- Use data sources instead of hardcoding resource IDs.
- Keep local values simple and reusable.
- Use data sources only when existing infrastructure is required.

---

## 📝 Key Takeaways

After completing this chapter, you should understand:

- ✅ What Local Values are
- ✅ Why Locals are useful
- ✅ Local syntax
- ✅ What Data Sources are
- ✅ Data Source syntax
- ✅ Using Data Sources
- ✅ Locals vs Data Sources
- ✅ Best practices

---

## 📖 Next Topic

➡️ **09 - Terraform Modules**

In the next chapter, you'll learn:

- What are Modules?
- Module structure
- Root and Child Modules
- Module inputs and outputs
- Reusing infrastructure
- Best practices
