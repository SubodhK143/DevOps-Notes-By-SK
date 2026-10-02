# ConfigMaps

> A ConfigMap stores non-sensitive configuration data separately from application containers, making applications easier to configure and manage.

---

# 1. What is a ConfigMap?

A **ConfigMap** is a Kubernetes resource used to store non-sensitive configuration data as **key-value pairs**.

Instead of hardcoding configuration inside a container image, Kubernetes allows applications to read configuration from a ConfigMap.

Examples include:

- Application settings
- Environment variables
- Configuration files
- Command-line arguments

> 💡 **Think of a ConfigMap as an external configuration file for your application.**

---

# 2. Why Kubernetes Uses ConfigMaps

Applications often require different configurations in different environments.

For example:

**Development**

```text
Database = mysql-dev
```

**Production**

```text
Database = mysql-prod
```

Instead of building multiple container images, Kubernetes allows the same image to be reused while changing only the configuration.

ConfigMaps provide:

- Externalized configuration
- Better portability
- Easier updates
- Environment-specific settings
- Separation of application code and configuration

---

# 3. How ConfigMaps Work

Applications read configuration from a ConfigMap instead of storing it inside the container image.

```text
ConfigMap
      │
      ▼
Pod
├── Environment Variables
├── Volume Mount
└── Command-line Arguments
```

A ConfigMap can be consumed in three ways:

- Environment variables
- Mounted files inside a Volume
- Command-line arguments

---

# 4. Anatomy of a ConfigMap Manifest

## Example

```yaml
apiVersion: v1
kind: ConfigMap

metadata:
  name: app-config

data:
  APP_NAME: shopping-app
  APP_ENV: development
  LOG_LEVEL: info
```

---

## Manifest Fields

| **Field** | **Purpose** |
|-----------|-------------|
| `apiVersion` | Kubernetes API version |
| `kind` | Resource type |
| `metadata` | ConfigMap information |
| `data` | Stores text-based configuration as key-value pairs |

---

## ConfigMap Data Types

A ConfigMap can store configuration using two fields.

| **Field** | **Purpose** |
|-----------|-------------|
| `data` | Stores text-based configuration as UTF-8 strings. |
| `binaryData` | Stores binary configuration encoded as Base64. |

> 💡 Most ConfigMaps use the `data` field.

---

# 5. Ways to Use ConfigMaps

A ConfigMap can be consumed in multiple ways.

---

## 5.1 As Environment Variables

```yaml
env:
  - name: APP_ENV
    valueFrom:
      configMapKeyRef:
        name: app-config
        key: APP_ENV
```

---

## 5.2 Import All Keys as Environment Variables

```yaml
envFrom:
  - configMapRef:
      name: app-config
```

Every key inside the ConfigMap becomes an environment variable.

---

## 5.3 Mount as a Volume

```yaml
volumes:
  - name: config-volume
    configMap:
      name: app-config
```

The files can then be mounted inside the container using `volumeMounts`.

---

# 6. Creating ConfigMaps

## Using YAML

```bash
kubectl apply -f configmap.yaml
```

---

## Using Literals

```bash
kubectl create configmap app-config \
  --from-literal=APP_ENV=development \
  --from-literal=LOG_LEVEL=info
```

---

## Using a File

Create a ConfigMap from an existing configuration file.

```bash
kubectl create configmap nginx-config \
  --from-file=default.conf
```

This is commonly used for applications such as **Nginx**, **Apache**, and **HAProxy**.

---

## Verify

```bash
kubectl get configmaps
```

or

```bash
kubectl get cm
```

---

# 7. Mounted Files

When a ConfigMap is mounted as a Volume, each key becomes a separate file.

Example ConfigMap:

```text
APP_ENV=production
LOG_LEVEL=info
```

Mounted inside the container:

```text
/etc/config/

├── APP_ENV
└── LOG_LEVEL
```

Contents:

```text
APP_ENV
└── production

LOG_LEVEL
└── info
```

> 💡 The filename is the ConfigMap key, and the file content is its value.

---

# 8. Updating a ConfigMap

Edit an existing ConfigMap.

```bash
kubectl edit configmap app-config
```

View the updated ConfigMap.

```bash
kubectl describe configmap app-config
```

---

# 9. Update Propagation

How updates reach running Pods depends on how the ConfigMap is consumed.

| **Method** | **Updates Automatically?** |
|------------|---------------------------|
| Environment Variables | ❌ No |
| Mounted Volume | ✅ Yes (usually within 30–60 seconds) |

If a ConfigMap is used as **environment variables**, Pods must be restarted to read the updated values.

If it is mounted as a **Volume**, Kubernetes automatically updates the files.

---

# 10. ConfigMap vs Hardcoded Configuration

| **ConfigMap** | **Hardcoded Configuration** |
|---------------|-----------------------------|
| Configuration stored outside the image | Configuration stored inside the image |
| Easy to update | Requires rebuilding the image |
| Environment-specific | Difficult to reuse |
| Kubernetes native | Less flexible |

---

# 11. ConfigMap Size Limit

A ConfigMap has a maximum size of **1 MiB**.

For larger configuration data:

- Use external storage
- Store files in object storage
- Use a dedicated configuration management solution

---

# 12. Useful kubectl Commands

```bash
# Create a ConfigMap
kubectl apply -f configmap.yaml

# List ConfigMaps
kubectl get configmaps

# Short form
kubectl get cm

# Describe ConfigMap
kubectl describe configmap app-config

# View YAML
kubectl get configmap app-config -o yaml

# Edit ConfigMap
kubectl edit configmap app-config

# Delete ConfigMap
kubectl delete configmap app-config

# Explain ConfigMap
kubectl explain configmap
```

---

# 13. Best Practices

- Store only non-sensitive data in ConfigMaps.
- Keep application code separate from configuration.
- Use meaningful key names.
- Reuse the same container image across environments.
- Version configuration using YAML manifests.
- Keep ConfigMaps small and focused.
- Avoid storing large files inside ConfigMaps.
- Use Secrets for passwords, tokens, API keys, and certificates.

---

# 14. ConfigMaps vs Secrets

| **ConfigMap** | **Secret** |
|---------------|------------|
| Stores non-sensitive data | Stores sensitive data |
| Stored as plain text in etcd unless encryption at rest is enabled | Base64 encoded *(not encrypted by default)* |
| Configuration | Passwords, API keys, tokens, certificates |

> 💡 ConfigMaps and Secrets are commonly used together in production applications.

---

# 15. Key Takeaways

- ✅ ConfigMaps store non-sensitive configuration.
- ✅ Configuration is separated from application code.
- ✅ Applications can consume ConfigMaps as environment variables or mounted files.
- ✅ ConfigMaps can be created from YAML, literals, or files.
- ✅ Mounted ConfigMaps update automatically.
- ✅ Environment variables require Pod restart after updates.
- ✅ ConfigMaps improve portability and reusability.
- ✅ Secrets should be used for sensitive information.

---

# 📚 What's Next?

➡️ **08. Secrets**
