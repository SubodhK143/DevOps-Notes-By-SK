# 🚀 Terraform Cheat Sheet

> A quick reference guide for the most commonly used Terraform concepts, commands, and syntax.

---

# 📚 Table of Contents

- [Terraform Workflow](#-terraform-workflow)
- [Common Commands](#-common-commands)
- [Project Structure](#-project-structure)
- [Basic Syntax](#-basic-syntax)
- [Variables & Outputs](#-variables--outputs)
- [Meta Arguments](#-meta-arguments)
- [State Commands](#-state-commands)
- [Workspace Commands](#-workspace-commands)
- [Quick Revision](#-quick-revision)
- [Best Practices](#-best-practices)

---

## 🔄 Terraform Workflow

```text
Write Code
     │
     ▼
terraform fmt
     │
     ▼
terraform validate
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

---

## ⚙️ Common Commands

| Command | Purpose |
|---------|---------|
| `terraform init` | Initialize the project |
| `terraform fmt` | Format Terraform files |
| `terraform validate` | Validate configuration syntax |
| `terraform plan` | Preview infrastructure changes |
| `terraform apply` | Create or update infrastructure |
| `terraform destroy` | Delete managed infrastructure |
| `terraform show` | Display current state |
| `terraform output` | Show output values |
| `terraform version` | Show Terraform version |
| `terraform console` | Evaluate Terraform expressions |
| `terraform graph` | Generate dependency graph |

---

## 📂 Project Structure

```text
terraform-project/
├── main.tf
├── providers.tf
├── variables.tf
├── outputs.tf
├── terraform.tfvars
├── versions.tf
├── modules/
└── README.md
```

---

## 📝 Basic Syntax

### Provider

```hcl
provider "aws" {
  region = "ap-south-1"
}
```

### Resource

```hcl
resource "aws_instance" "web" {
  ami           = "ami-xxxxxxxx"
  instance_type = "t2.micro"
}
```

### Variable

```hcl
variable "instance_type" {
  type = string
}
```

### Output

```hcl
output "public_ip" {
  value = aws_instance.web.public_ip
}
```

---

## 📥 Variables & Outputs

Reference a variable:

```hcl
var.instance_type
```

Reference a local value:

```hcl
local.project_name
```

Reference a data source:

```hcl
data.aws_ami.latest.id
```

Reference an output:

```bash
terraform output
```

Specific output:

```bash
terraform output public_ip
```

---

## ⚡ Meta Arguments

| Meta Argument | Purpose |
|---------------|---------|
| `count` | Create multiple resources |
| `for_each` | Create resources from a list or map |
| `depends_on` | Define explicit dependencies |
| `provider` | Select a Provider configuration |
| `lifecycle` | Control create/update/delete behavior |

---

## 📦 State Commands

| Command | Purpose |
|---------|---------|
| `terraform show` | Display current state |
| `terraform state list` | List managed resources |
| `terraform state show <resource>` | Show resource details |

---

## 🌐 Workspace Commands

| Command | Purpose |
|---------|---------|
| `terraform workspace new <name>` | Create a workspace |
| `terraform workspace list` | List workspaces |
| `terraform workspace select <name>` | Switch workspace |
| `terraform workspace show` | Display current workspace |
| `terraform workspace delete <name>` | Delete a workspace |

---

## 🧠 Quick Revision

| Topic | Remember |
|--------|----------|
| Provider | Connects Terraform to cloud platforms |
| Resource | Infrastructure component |
| Variable | Input value |
| Output | Displays resource information |
| Local | Reusable local value |
| Data Source | Reads existing infrastructure |
| Module | Reusable Terraform configuration |
| State | Tracks infrastructure |
| Backend | Stores Terraform state |
| Workspace | Separate environment |
| Meta Arguments | Control Terraform behavior |

---

## ⭐ Best Practices

- ✅ Run `terraform fmt` before committing.
- ✅ Validate configurations with `terraform validate`.
- ✅ Review changes using `terraform plan`.
- ✅ Use Variables instead of hardcoded values.
- ✅ Organize reusable code into Modules.
- ✅ Use Remote Backends for team projects.
- ✅ Never commit `terraform.tfstate` to Git.
- ✅ Pin Provider versions.
- ✅ Use meaningful resource names.

---

# 👩‍💻 Author

**Jaishree Chaure**

**AWS Certified Cloud Practitioner • Aspiring DevOps Engineer**

Documented as part of the **#90DaysOfDevOps** challenge (UDAAN Batch 11).

⭐ If you found this repository helpful, consider **starring it on GitHub**.
