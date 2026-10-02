# 🚀 Terraform Provisioners

> Learn how **Terraform Provisioners** execute scripts or commands on local or remote machines after infrastructure is created.

---

# 📚 Table of Contents

- [What are Provisioners?](#-what-are-provisioners)
- [Why Use Provisioners?](#-why-use-provisioners)
- [Types of Provisioners](#-types-of-provisioners)
- [Local-exec Provisioner](#-local-exec-provisioner)
- [Remote-exec Provisioner](#-remote-exec-provisioner)
- [When to Use Provisioners](#-when-to-use-provisioners)
- [Best Practices](#-best-practices)
- [Key Takeaways](#-key-takeaways)

---

## ⚙️ What are Provisioners?

Provisioners allow Terraform to run scripts or commands **after a resource is created or before it is destroyed**.

They are commonly used for:

- Installing software
- Running setup scripts
- Configuring servers
- Executing local commands

> 💡 Provisioners are considered a **last resort**. Whenever possible, use cloud-native tools such as **User Data**, **Cloud-Init**, or configuration management tools like **Ansible**.

---

## 🤔 Why Use Provisioners?

Provisioners help automate tasks that Terraform cannot perform directly.

Examples include:

- Installing packages
- Updating configuration files
- Running shell scripts
- Executing deployment commands

---

## 📦 Types of Provisioners

Terraform provides two commonly used Provisioners.

| Provisioner | Purpose |
|-------------|---------|
| `local-exec` | Runs commands on the local machine where Terraform is executed |
| `remote-exec` | Runs commands on the remote resource (such as an EC2 instance) |

---

## 💻 Local-exec Provisioner

Runs a command on your local machine.

Example:

```hcl
resource "null_resource" "example" {
  provisioner "local-exec" {
    command = "echo Terraform deployment completed!"
  }
}
```

After the resource is created, Terraform executes the command locally.

---

## 🌐 Remote-exec Provisioner

Runs commands on the remote server using SSH or WinRM.

Example:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0f58b397bc5c1f2e8"
  instance_type = "t2.micro"

  provisioner "remote-exec" {
    inline = [
      "sudo apt update",
      "sudo apt install -y nginx"
    ]
  }
}
```

Terraform connects to the instance and runs the commands.

---

## 📌 When to Use Provisioners

Use Provisioners only when necessary.

Good use cases:

- Bootstrap a server
- Run one-time setup scripts
- Execute local automation tasks

Avoid using Provisioners for long-term configuration management.

---

## ⭐ Best Practices

- Use Provisioners only as a last resort.
- Prefer User Data or Cloud-Init for server initialization.
- Use configuration management tools (such as Ansible) for complex setups.
- Keep Provisioner scripts simple.
- Test scripts before running them with Terraform.

---

## 📝 Key Takeaways

After completing this chapter, you should understand:

- ✅ What Provisioners are
- ✅ Why Provisioners are used
- ✅ `local-exec`
- ✅ `remote-exec`
- ✅ When to use Provisioners
- ✅ Best practices

---

## 📖 Next Topic

➡️ **15 - Terraform Best Practices**

In the next chapter, you'll learn:

- Project structure
- Naming conventions
- State management
- Security best practices
- Code organization
- Terraform workflow
