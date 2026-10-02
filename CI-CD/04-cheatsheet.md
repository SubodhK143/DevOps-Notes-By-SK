# CI/CD Cheatsheet

A quick reference guide for commonly used CI/CD concepts, workflows, pipeline stages, and tools.

> **Note:** This cheatsheet is intended for quick revision and daily use. Refer to the individual markdown files for detailed explanations and examples.

---

## 1. CI/CD Overview

```text
CI/CD

↓

Continuous Integration

↓

Continuous Delivery

↓

Continuous Deployment
```

| Term | Purpose |
|------|--------|
| CI | Build and test code changes automatically |
| Continuous Delivery | Prepare applications for reliable releases |
| Continuous Deployment | Automatically deploy applications to production |

---

## 2. Continuous Integration (CI)

```text
Write Code
     |
     v
Push Code
     |
     v
Build Application
     |
     v
Run Tests
     |
     v
Get Feedback
```

### Common Activities

- Build automation
- Automated testing
- Code validation
- Build feedback

---

## 3. Continuous Delivery

```text
Successful Build
        |
        v
Deploy to Staging
        |
        v
Validate Application
        |
        v
Ready for Production Release
```

### Common Activities

- Deployment automation
- Packaging applications
- Release preparation
- Staging deployments

---

## 4. Continuous Deployment

```text
Code Commit
      |
      v
Build Application
      |
      v
Run Automated Tests
      |
      v
Deploy to Production
      |
      v
Application Goes Live
```

### Common Activities

- Automated deployments
- Production releases
- Monitoring and feedback

---

## 5. Complete CI/CD Pipeline

```text
Code Commit
     |
     v
Build Application
     |
     v
Run Automated Tests
     |
     v
Store Build Artifacts
     |
     v
Deploy to Staging
     |
     v
Validate Deployment
     |
     v
Deploy to Production
     |
     v
Monitor Application
     |
     v
Generate Feedback & Alerts
     |
     v
Rollback if Necessary
```

---

## 6. Common Pipeline Stages

| Stage | Purpose |
|------|--------|
| Code Commit | Push application code |
| Build | Compile and package application |
| Testing | Validate application quality |
| Artifact Management | Store deployable build outputs |
| Deployment | Release application |
| Monitoring | Track application health |
| Feedback | Identify issues quickly |
| Rollback | Recover from failures |

---

## 7. Common CI/CD Tools

### Source Code Repositories

```text
GitHub
GitLab
Bitbucket
AWS CodeCommit
```

### Continuous Integration Tools

```text
GitHub Actions
Jenkins
CircleCI
Travis CI
AWS CodeBuild
GitLab CI/CD
```

### Continuous Delivery Tools

```text
AWS CodeDeploy
Spinnaker
Octopus Deploy
Jenkins CD
```

### Containerization Tools

```text
Docker
Docker Compose
```

### Container Orchestration Tools

```text
Kubernetes
Amazon ECS
Amazon EKS
```

### Monitoring Tools

```text
Prometheus
Grafana
ELK Stack
AWS CloudWatch
Datadog
New Relic
```

---

## 8. Real-World CI/CD Workflow

```text
GitHub
   |
   v
GitHub Actions
   |
   v
Docker
   |
   v
Amazon ECR
   |
   v
Amazon ECS
   |
   v
CloudWatch
```

---

## 9. CI/CD Best Practices

- Commit code frequently.
- Automate builds and testing.
- Deploy smaller changes more often.
- Use staging environments.
- Implement monitoring and alerting.
- Maintain rollback strategies.
- Secure CI/CD pipelines properly.
- Continuously improve deployment workflows.

---

## 10. Key Rules

- Always test before deployment.
- Never deploy directly to production without validation.
- Automate repetitive tasks whenever possible.
- Monitor applications after deployment.
- Use version control effectively.
- Implement rollback mechanisms.
- Keep CI/CD pipelines secure and maintainable.

---

## 11. Quick Tips

- CI improves code quality.
- Continuous Delivery improves release reliability.
- Continuous Deployment improves release speed.
- Automated testing is essential for successful CI/CD pipelines.
- Containerization simplifies deployments.
- Monitoring helps detect issues early.
- Smaller and frequent deployments reduce risks.

---

## 12. Quick Summary

| Concept | Purpose |
|--------|--------|
| Continuous Integration | Build and test code automatically |
| Continuous Delivery | Prepare applications for deployment |
| Continuous Deployment | Automate production deployments |
| CI/CD Pipeline | Automate software delivery workflows |
| Build Stage | Generate deployable application files |
| Testing Stage | Validate application quality |
| Deployment Stage | Release applications safely |
| Monitoring Stage | Track performance and health |
| Best Practice | Automate testing and deployments |
