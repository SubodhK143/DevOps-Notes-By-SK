# 🚀 DevOps Notes by SK

> **A practical DevOps learning repository containing notes, commands, configurations, troubleshooting guides, hands-on labs, and useful resources.**

Welcome to **DevOps Notes by SK** 👋

This repository is my personal collection of **DevOps and Cloud Computing notes**, created while learning and practicing technologies used in real-world DevOps environments.

The goal is simple:

**Learn → Practice → Document → Build → Improve**

---

## 📚 What's Inside?

This repository covers a growing collection of DevOps technologies, tools, commands, concepts, and hands-on practice.

| 📂 Topic          | 🔎 What You'll Find                                      |
| ----------------- | -------------------------------------------------------- |
| ☁️ AWS / Cloud    | Cloud concepts, services, architecture & practical notes |
| 🐧 Linux          | Commands, administration & troubleshooting               |
| 🐳 Docker         | Containers, images, Dockerfiles & commands               |
| ☸️ Kubernetes     | Pods, Deployments, Services, ConfigMaps, Secrets & more  |
| 🏗️ Terraform     | Infrastructure as Code, resources & configurations       |
| 🔧 Ansible        | Automation, playbooks & configuration management         |
| 🔄 CI/CD          | Continuous Integration & Continuous Deployment concepts  |
| ⚙️ GitHub Actions | Workflow automation & CI/CD pipelines                    |
| 🔐 DevSecOps      | Security practices in DevOps                             |
| ⎈ Helm            | Kubernetes package management & charts                   |
| 📝 YAML           | YAML syntax and DevOps configuration examples            |
| 📖 Resources      | Documentation, references & useful learning material     |

---

## 🗂️ Repository Structure

```text
DevOps-Notes-By-SK/
│
├── Ansible/
├── CI-CD/
├── DevSecOps/
├── Docker/
├── Github Actions/
├── Helm/
├── Kubernetes/
├── Official-Docs/
├── Resources/
├── Terraform/
├── YAML/
│
├── LICENSE.md
└── README.md
```

---

# 🐧 Linux

Linux is one of the most important foundations for DevOps.

Topics include:

* Linux commands
* File & directory management
* Users and groups
* Permissions
* Processes
* Services
* Networking
* SSH
* Package management
* System troubleshooting
* Shell scripting

### Useful Commands

```bash
ls
cd
pwd
cp
mv
rm
mkdir
touch
cat
grep
find
chmod
chown
ps
top
df -h
free -m
systemctl
journalctl
```

---

# 🐳 Docker

Docker is used to package applications and their dependencies into portable containers.

Topics covered include:

* Docker architecture
* Images
* Containers
* Dockerfile
* Docker commands
* Docker volumes
* Docker networks
* Docker Compose
* Container troubleshooting
* Application containerization

Example:

```bash
docker build -t myapp .
docker images
docker ps
docker run -d -p 8080:80 myapp
docker logs <container-id>
docker exec -it <container-id> /bin/bash
```

---

# ☸️ Kubernetes

Kubernetes is a major part of modern container orchestration.

Topics include:

* Kubernetes architecture
* Pods
* Deployments
* ReplicaSets
* Services
* ConfigMaps
* Secrets
* Namespaces
* Ingress
* Volumes
* Probes
* Scaling
* Helm
* Troubleshooting

Example:

```bash
kubectl get pods
kubectl get deployments
kubectl get services
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl apply -f deployment.yaml
kubectl delete -f deployment.yaml
```

---

# 🏗️ Terraform

Terraform is used for **Infrastructure as Code (IaC)**.

Topics include:

* Providers
* Resources
* Variables
* Outputs
* Data sources
* Modules
* State
* Terraform commands
* AWS infrastructure provisioning
* Troubleshooting

Basic workflow:

```bash
terraform init
terraform validate
terraform plan
terraform apply
terraform destroy
```

---

# 🔧 Ansible

Ansible is used for configuration management and automation.

Topics include:

* Inventory
* Playbooks
* Tasks
* Variables
* Modules
* Handlers
* Roles
* Remote execution
* Server configuration

Example:

```bash
ansible all -m ping
ansible-playbook playbook.yml
```

---

# 🔄 CI/CD

Continuous Integration and Continuous Deployment are essential DevOps practices.

This section contains notes and practical examples related to:

* CI/CD concepts
* Build automation
* Testing
* Deployment
* Pipeline design
* Jenkins
* GitHub Actions
* Automated deployments

Typical pipeline:

```text
Developer
    ↓
Git Push
    ↓
Build
    ↓
Test
    ↓
Docker Build
    ↓
Security Scan
    ↓
Deploy
    ↓
Monitoring
```

---

# ⚙️ GitHub Actions

GitHub Actions can automate software development workflows directly from GitHub.

Topics include:

* Workflow files
* Jobs
* Steps
* Actions
* Secrets
* Environment variables
* CI pipelines
* CD pipelines
* Docker automation
* Deployment workflows

Example workflow structure:

```yaml
name: CI Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Build
        run: echo "Building application..."
```

---

# ⎈ Helm

Helm is the package manager for Kubernetes.

Topics include:

* Helm charts
* Chart structure
* Values
* Templates
* Releases
* Helm commands
* Kubernetes application deployment

Useful commands:

```bash
helm create mychart
helm install myapp ./mychart
helm list
helm upgrade myapp ./mychart
helm uninstall myapp
```

---

# 🔐 DevSecOps

DevSecOps integrates security throughout the DevOps lifecycle.

Topics include:

* Secure CI/CD
* Secrets management
* Image scanning
* Dependency security
* Infrastructure security
* Kubernetes security
* Security automation

General approach:

```text
Plan
 ↓
Code
 ↓
Build
 ↓
Test
 ↓
Security Scan
 ↓
Deploy
 ↓
Monitor
```

---

# 📝 YAML

YAML is widely used across DevOps tools.

You'll find YAML examples related to:

* Kubernetes
* GitHub Actions
* Ansible
* CI/CD
* Configuration files

Example:

```yaml
app:
  name: devops-app
  environment: production

resources:
  cpu: "500m"
  memory: "512Mi"
```

---

# ☁️ AWS & Cloud

The repository also supports my hands-on learning journey with AWS Cloud and DevOps.

Areas of practice include:

* EC2
* S3
* IAM
* VPC
* RDS
* Lambda
* CloudWatch
* Load Balancers
* Auto Scaling
* CloudFront
* Route 53
* ECR
* ECS
* EKS
* DynamoDB
* KMS
* API Gateway
* Terraform-based AWS infrastructure

---

# 🧪 Hands-on Learning

This repository is not intended to be only theoretical.

I focus on:

```text
📖 Learn the Concept
       ↓
💻 Practice the Commands
       ↓
🧪 Perform Hands-on Labs
       ↓
🏗️ Build Projects
       ↓
🐛 Troubleshoot Errors
       ↓
📝 Document the Solution
       ↓
🚀 Apply in Real Projects
```

---

# 🎯 Learning Goals

My current focus is to strengthen practical skills in:

* ☁️ AWS Cloud
* 🐧 Linux
* 🐳 Docker
* ☸️ Kubernetes
* 🏗️ Terraform
* 🔧 Ansible
* 🔄 CI/CD
* ⚙️ GitHub Actions
* 🔐 DevSecOps
* ⎈ Helm
* 📊 Monitoring & Observability
* 🤖 Cloud Automation

---

# 🛠️ DevOps Toolset

```text
Cloud       → AWS
OS          → Linux
SCM         → Git / GitHub
Containers  → Docker
Orchestration → Kubernetes
IaC         → Terraform
Automation  → Ansible
CI/CD       → GitHub Actions / Jenkins
Packaging   → Helm
Security    → DevSecOps
Scripting   → Bash / Python
Monitoring  → Prometheus / Grafana
```

---

# 📖 Official Documentation

A dedicated `Official-Docs` section is included for keeping references to official documentation and reliable technical resources.

Whenever possible, prefer official documentation when learning or troubleshooting a technology.

---

# 🚀 How to Use This Repository

Clone the repository:

```bash
git clone https://github.com/SubodhK143/DevOps-Notes-By-SK.git
```

Move into the repository:

```bash
cd DevOps-Notes-By-SK
```

Explore the topic you want to learn:

```text
Docker/
Kubernetes/
Terraform/
Ansible/
CI-CD/
DevSecOps/
Helm/
Github Actions/
```

---

# 👨‍💻 About Me

**Subodh Kumar**

AWS Cloud Support Engineer | AWS Certified | DevOps & Cloud Enthusiast

I'm continuously building my knowledge through:

* Hands-on AWS labs
* DevOps projects
* Cloud automation
* Kubernetes practice
* Infrastructure as Code
* CI/CD implementation
* Troubleshooting real-world scenarios
* Continuous learning and documentation

---

## 🏆 Certifications

* ☁️ AWS Certified Solutions Architect – Associate
* ☁️ AWS Certified Cloud Practitioner
* 🤖 AWS Certified AI Practitioner

---

# 🔗 Connect With Me

### 💼 LinkedIn

**Subodh Kumar**

https://www.linkedin.com/in/subodh-kumar-aws-certified/

### 🐙 GitHub

https://github.com/SubodhK143

---

# ⭐ Support

If you find these notes useful:

⭐ **Star this repository**

🍴 **Fork the repository**

📢 **Share it with other DevOps learners**

Contributions, corrections, suggestions, and improvements are welcome.

---

## 📌 Disclaimer

These notes are created for **learning, practice, and reference purposes**.

Commands and configurations should be tested in a suitable environment before being used in production.

---

## 🚀 Keep Learning. Keep Building. Keep Automating.

> **"Don't just learn DevOps — practice it, automate it, troubleshoot it, and build with it."**

---

**Made with ❤️ by Subodh Kumar**

*Learning DevOps one hands-on lab at a time.*
