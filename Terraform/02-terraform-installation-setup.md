# ⚙️ Terraform Installation & Setup

> In this chapter, you'll install **Terraform**, configure the **AWS CLI**, verify your environment, and create your first Terraform project.

---

# 📚 Table of Contents

- [Prerequisites](#-prerequisites)
- [Install Terraform](#-install-terraform)
- [Verify Installation](#-verify-installation)
- [Install AWS CLI](#-install-aws-cli)
- [Configure AWS CLI](#-configure-aws-cli)
- [Verify AWS Credentials](#-verify-aws-credentials)
- [Recommended VS Code Extensions](#-recommended-vs-code-extensions)
- [Create Your First Terraform Project](#-create-your-first-terraform-project)
- [Project Structure](#-project-structure)
- [Initialize Terraform](#-initialize-terraform)
- [Validate Configuration](#-validate-configuration)
- [Common Installation Issues](#-common-installation-issues)
- [Best Practices](#-best-practices)
- [Key Takeaways](#-key-takeaways)

---

# 📋 Prerequisites

Before installing Terraform, ensure you have:

- Ubuntu / Windows / macOS
- Internet connection
- AWS Account
- IAM User with programmatic access
- Visual Studio Code
- Terminal or PowerShell
- Git installed (recommended)

---

# 💻 Install Terraform

## Ubuntu (Official HashiCorp Repository)

```bash
wget -O- https://apt.releases.hashicorp.com/gpg | \
sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list

sudo apt update

sudo apt install terraform
```

---

## Windows (Chocolatey)

```powershell
choco install terraform
```

---

## Windows (Winget)

```powershell
winget install HashiCorp.Terraform
```

---

## macOS (Homebrew)

```bash
brew tap hashicorp/tap

brew install hashicorp/tap/terraform
```

---

# ✅ Verify Installation

Check the installed version.

```bash
terraform version
```

Example:

```text
Terraform v1.13.x
```

Also verify Terraform is available in your system PATH.

```bash
which terraform
```

Windows:

```powershell
where terraform
```

---

# ☁️ Install AWS CLI

## Ubuntu

```bash
sudo apt update
sudo apt install unzip -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

Verify:

```bash
aws --version
```

Example:

```text
aws-cli/2.x.x
```

---

# 🔑 Configure AWS CLI

Run:

```bash
aws configure
```

Provide:

```text
AWS Access Key ID
AWS Secret Access Key
Default Region
Output Format
```

Example:

```text
Region: us-east-1

Output: json
```

Terraform automatically uses these credentials when communicating with AWS.

---

# ✅ Verify AWS Credentials

Run:

```bash
aws sts get-caller-identity
```

Example Output

```json
{
  "UserId": "...",
  "Account": "123456789012",
  "Arn": "arn:aws:iam::123456789012:user/terraform-user"
}
```

If this command succeeds, AWS CLI authentication is working correctly.

---

# 🛠️ Recommended VS Code Extensions

Install the following extensions for a better development experience:

| Extension | Purpose |
|------------|----------|
| HashiCorp Terraform | Terraform syntax highlighting and IntelliSense |
| AWS Toolkit | AWS integration |
| YAML | YAML support |
| GitLens | Git enhancements |
| Error Lens | Inline error highlighting |

---

# 📁 Create Your First Terraform Project

Create a project directory.

```bash
mkdir terraform-demo

cd terraform-demo
```

---

# 📂 Project Structure

```text
terraform-demo/
│
├── main.tf
├── variables.tf
├── outputs.tf
└── terraform.tfvars
```

Initially, only **main.tf** is required.

---

# 📄 Create main.tf

```hcl
terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "~> 6.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}
```

Save the file.

---

# 🚀 Initialize Terraform

Initialize the project.

```bash
terraform init
```

Terraform will:

- Download provider plugins
- Create the `.terraform` directory
- Generate `.terraform.lock.hcl`
- Prepare the working directory

Expected output:

```text
Terraform has been successfully initialized!
```

---

# ✔️ Validate Configuration

Check the syntax.

```bash
terraform validate
```

Expected output:

```text
Success! The configuration is valid.
```

---

# 📦 Format Configuration

Format Terraform files.

```bash
terraform fmt
```

---

# 🔍 View Installed Providers

```bash
terraform providers
```

---

# 📂 Files Created After Initialization

```text
terraform-demo/
│
├── main.tf
├── .terraform/
├── .terraform.lock.hcl
```

---

# ⚠️ Common Installation Issues

## terraform: command not found

**Cause**

Terraform is not installed or not in the PATH.

**Solution**

Verify installation.

```bash
terraform version
```

---

## Invalid AWS Credentials

Verify credentials.

```bash
aws sts get-caller-identity
```

Run:

```bash
aws configure
```

again if necessary.

---

## Provider Download Failed

Check:

- Internet connection
- Firewall
- Proxy settings

Run again:

```bash
terraform init
```

---

# ⭐ Best Practices

- Install Terraform from the official HashiCorp repository.
- Keep Terraform updated.
- Use IAM users or IAM roles instead of the root account.
- Never hardcode AWS credentials.
- Verify AWS CLI before running Terraform.
- Use Git for version control.
- Run `terraform fmt` before committing.
- Run `terraform validate` before `terraform plan`.
- Add `.terraform/` and `*.tfstate` to `.gitignore`.

---

# 📝 Key Takeaways

After completing this chapter, you should be able to:

- ✅ Install Terraform
- ✅ Verify the installation
- ✅ Install AWS CLI
- ✅ Configure AWS credentials
- ✅ Verify AWS authentication
- ✅ Create your first Terraform project
- ✅ Initialize Terraform
- ✅ Validate Terraform configuration
- ✅ Format Terraform code
- ✅ Troubleshoot common installation issues

---

# 📖 Next Topic

➡️ **03 - Terraform Providers & Resources**

In the next chapter, you'll learn:

- What Providers are
- How Resources work
- Provider configuration
- Creating your first AWS resource
- Understanding the Terraform execution model
