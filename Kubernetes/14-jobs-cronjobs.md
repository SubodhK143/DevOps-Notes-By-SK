# Jobs & CronJobs

> Jobs and CronJobs are Kubernetes workload controllers used to run one-time or scheduled tasks instead of continuously running applications.

---

# 1. What is a Job?

A **Job** is a Kubernetes controller that creates one or more Pods to perform a specific task.

Unlike Deployments, a Job finishes when its task completes successfully.

Common use cases include:

- Database migrations
- Batch processing
- Data imports and exports
- Backup operations
- Report generation

> 💡 **Think of a Job as a task that runs once and then stops.**

---

# 2. Why Kubernetes Uses Jobs

Many applications need to execute temporary tasks instead of running continuously.

Jobs provide:

- Reliable task execution
- Automatic Pod recreation if a task fails
- Completion tracking
- Parallel task execution
- Retry mechanisms

---

# 3. How Jobs Work

A Job creates one or more Pods to execute a task.

```text
Job
 │
 ▼
Pod
 │
 ▼
Task Executes
 │
 ▼
Completed
```

If a Pod fails before completing, Kubernetes creates another Pod until the Job succeeds or reaches its retry limit.

---

# 4. Anatomy of a Job Manifest

## Example

```yaml
apiVersion: batch/v1
kind: Job

metadata:
  name: hello-job

spec:
  template:
    spec:
      restartPolicy: Never

      containers:
        - name: hello
          image: busybox
          command: ["sh", "-c", "echo Hello Kubernetes"]
```

---

## Manifest Fields

| **Field** | **Purpose** |
|-----------|-------------|
| `apiVersion` | Kubernetes API version |
| `kind` | Resource type |
| `metadata` | Job information |
| `template` | Pod template |
| `restartPolicy` | Restart behavior for Pods |

> 💡 Jobs commonly use `restartPolicy: Never` or `OnFailure`.

---

# 5. Job Completion

A Job is considered successful when the required number of Pods complete successfully.

Common states:

```text
Running
     │
     ▼
Completed
```

or

```text
Running
     │
     ▼
Failed
```

---

# 6. Parallel Jobs

Jobs can execute multiple Pods simultaneously.

```yaml
spec:
  completions: 5
  parallelism: 2
```

Meaning:

- Total tasks = **5**
- Maximum Pods running simultaneously = **2**

This is useful for batch processing.

---

# 7. Creating Your First Job

## Step 1

Create the Job.

```bash
kubectl apply -f job.yaml
```

---

## Step 2

Verify.

```bash
kubectl get jobs
```

---

## Step 3

View Pods.

```bash
kubectl get pods
```

---

## Step 4

View Logs.

```bash
kubectl logs <pod-name>
```

---

# 8. What is a CronJob?

A **CronJob** creates Jobs automatically according to a schedule.

It works similarly to the Linux **cron** scheduler.

Examples include:

- Daily backups
- Log cleanup
- Scheduled reports
- Database maintenance
- Health checks

> 💡 **Think of a CronJob as a scheduled Job.**

---

# 9. How CronJobs Work

```text
Cron Schedule
      │
      ▼
CronJob
      │
      ▼
Job
      │
      ▼
Pod
```

At the scheduled time:

1. CronJob creates a Job.
2. The Job creates a Pod.
3. The Pod executes the task.
4. The Job completes.

---

# 10. Cron Schedule Format

CronJobs use the standard cron expression format.

```text
* * * * *
│ │ │ │ │
│ │ │ │ └── Day of Week (0–7)
│ │ │ └──── Month
│ │ └────── Day of Month
│ └──────── Hour
└────────── Minute
```

Examples:

| **Schedule** | **Meaning** |
|--------------|-------------|
| `* * * * *` | Every minute |
| `0 * * * *` | Every hour |
| `0 0 * * *` | Every day at midnight |
| `0 9 * * 1` | Every Monday at 9:00 AM |

---

# 11. Anatomy of a CronJob Manifest

## Example

```yaml
apiVersion: batch/v1
kind: CronJob

metadata:
  name: backup-job

spec:
  schedule: "0 2 * * *"

  jobTemplate:
    spec:
      template:
        spec:
          restartPolicy: OnFailure

          containers:
            - name: backup
              image: busybox
              command:
                - sh
                - -c
                - echo "Running backup..."
```

---

## Manifest Fields

| **Field** | **Purpose** |
|-----------|-------------|
| `schedule` | Cron expression |
| `jobTemplate` | Template used to create Jobs |
| `restartPolicy` | Pod restart behavior |

---

# 12. Job vs CronJob

| **Job** | **CronJob** |
|----------|-------------|
| Runs once | Runs on a schedule |
| Manually created | Automatically creates Jobs |
| Executes a single task | Executes recurring tasks |
| No schedule | Uses cron expressions |

---

# 13. Useful kubectl Commands

```bash
# Create Job
kubectl apply -f job.yaml

# List Jobs
kubectl get jobs

# Describe Job
kubectl describe job hello-job

# View Job logs
kubectl logs <pod-name>

# Delete Job
kubectl delete job hello-job

# Create CronJob
kubectl apply -f cronjob.yaml

# List CronJobs
kubectl get cronjobs

# Short form
kubectl get cj

# Describe CronJob
kubectl describe cronjob backup-job

# Delete CronJob
kubectl delete cronjob backup-job

# Explain Job
kubectl explain job

# Explain CronJob
kubectl explain cronjob
```

---

# 14. Best Practices

- Use Jobs for one-time tasks.
- Use CronJobs for recurring tasks.
- Set appropriate retry policies.
- Monitor Job completion status.
- Keep scheduled tasks lightweight.
- Remove old Jobs when they are no longer needed.

---

# 15. Key Takeaways

- ✅ Jobs execute one-time tasks.
- ✅ CronJobs execute scheduled tasks.
- ✅ Jobs automatically recreate failed Pods.
- ✅ CronJobs create Jobs based on cron schedules.
- ✅ Jobs are ideal for batch processing.
- ✅ CronJobs are ideal for recurring maintenance tasks.

---

# 📚 What's Next?

➡️ **15. Ingress**
