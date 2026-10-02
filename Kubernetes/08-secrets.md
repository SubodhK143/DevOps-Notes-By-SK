# Secrets

> A Secret is a Kubernetes resource used to store sensitive information such as passwords, API keys, tokens, and certificates securely.

---

# 1. What is a Secret?

A **Secret** is a Kubernetes resource that stores sensitive data separately from application code and container images.

Unlike ConfigMaps, Secrets are designed for confidential information.

Examples include:

- Passwords
- API Keys
- Database credentials
- OAuth Tokens
- TLS Certificates
- SSH Keys

> 💡 **Think of a Secret as a secure storage location for sensitive application data.**

---

# 2. Why Kubernetes Uses Secrets

Applications often require confidential information to function.

For example:

**Development**

```text
Database Password = dev123
```

**Production**

```text
Database Password = P@ssw0rd!@#
```

Instead of storing sensitive information inside container images or YAML files, Kubernetes stores it in Secrets.

Secrets provide:

- Separation of sensitive data from application code
- Better security
- Easier credential management
- Environment-specific configuration
- Centralized secret storage

---

# 3. How Secrets Work

Applications retrieve sensitive information from a Secret instead of storing it inside the container image.

```text
Secret
    │
    ▼
Pod
├── Environment Variables
├── Mounted Files
└── Volume
```

Secrets can be consumed as:

- Environment variables
- Mounted files
- Volumes

---

# 4. Anatomy of a Secret Manifest

## Example

```yaml
apiVersion: v1
kind: Secret

metadata:
  name: db-secret

type: Opaque

data:
  username: YWRtaW4=
  password: cGFzc3dvcmQ=
```

---

## Manifest Fields

| **Field** | **Purpose** |
|-----------|-------------|
| `apiVersion` | Kubernetes API version |
| `kind` | Resource type |
| `metadata` | Secret information |
| `type` | Specifies the Secret type |
| `data` | Stores Base64-encoded values |

> 💡 Secret values stored in the `data` field must be Base64 encoded.

---

# 5. Secret Types

Kubernetes supports multiple Secret types.

| **Type** | **Purpose** |
|-----------|-------------|
| `Opaque` | Default Secret type for arbitrary key-value data |
| `kubernetes.io/tls` | TLS certificates |
| `kubernetes.io/dockerconfigjson` | Docker registry credentials |
| `kubernetes.io/basic-auth` | Username and password |
| `kubernetes.io/ssh-auth` | SSH authentication |
| `kubernetes.io/service-account-token` | Service account credentials |

---

# 6. Ways to Use Secrets

Secrets can be consumed in multiple ways.

---

## 6.1 As Environment Variables

```yaml
env:
  - name: DB_PASSWORD
    valueFrom:
      secretKeyRef:
        name: db-secret
        key: password
```

---

## 6.2 Import All Keys

```yaml
envFrom:
  - secretRef:
      name: db-secret
```

---

## 6.3 Mount as a Volume

```yaml
volumes:
  - name: secret-volume
    secret:
      secretName: db-secret
```

The files are then mounted inside the container using `volumeMounts`.

---

# 7. Creating Secrets

## Using YAML

```bash
kubectl apply -f secret.yaml
```

---

## Using Literals

```bash
kubectl create secret generic db-secret \
  --from-literal=username=admin \
  --from-literal=password=admin123
```

---

## Using a File

```bash
kubectl create secret generic tls-secret \
  --from-file=certificate.crt \
  --from-file=private.key
```

---

## Verify

```bash
kubectl get secrets
```

---

# 8. Viewing Secret Values

Describe a Secret.

```bash
kubectl describe secret db-secret
```

View YAML.

```bash
kubectl get secret db-secret -o yaml
```

Decode a value.

```bash
kubectl get secret db-secret \
-o jsonpath="{.data.password}" | base64 --decode
```

> 💡 Kubernetes stores Secret values as Base64-encoded strings. Base64 encoding is **not encryption**.

---

# 9. Secret Updates

Update an existing Secret.

```bash
kubectl edit secret db-secret
```

Update behavior depends on how the Secret is consumed.

| **Method** | **Updates Automatically?** |
|------------|---------------------------|
| Environment Variables | ❌ No |
| Mounted Volume | ✅ Yes (usually within 30–60 seconds) |

If a Secret is consumed as environment variables, restart the Pod to read updated values.

---

# 10. ConfigMaps vs Secrets

| **ConfigMap** | **Secret** |
|---------------|------------|
| Stores non-sensitive data | Stores sensitive data |
| Plain text | Base64 encoded |
| Configuration | Passwords, tokens, certificates |
| General application settings | Confidential information |

> 💡 Use ConfigMaps and Secrets together to separate configuration from sensitive credentials.

---

# 11. Security Best Practices

- Never store passwords inside container images.
- Never commit Secret values to Git repositories.
- Enable **Encryption at Rest** for Secrets.
- Use RBAC to restrict Secret access.
- Rotate credentials regularly.
- Mount Secrets only where required.
- Use external secret managers for production when possible.

---

# 12. Useful kubectl Commands

```bash
# List Secrets
kubectl get secrets

# Describe Secret
kubectl describe secret db-secret

# View YAML
kubectl get secret db-secret -o yaml

# Edit Secret
kubectl edit secret db-secret

# Delete Secret
kubectl delete secret db-secret

# Explain Secret
kubectl explain secret

# Decode Secret
kubectl get secret db-secret \
-o jsonpath="{.data.password}" | base64 --decode
```

---

# 13. Key Takeaways

- ✅ Secrets store sensitive information.
- ✅ Secret values are Base64 encoded.
- ✅ Base64 encoding is **not encryption**.
- ✅ Secrets can be consumed as environment variables or mounted files.
- ✅ Avoid storing Secrets in source code or container images.
- ✅ Use RBAC and Encryption at Rest to improve Secret security.
- ✅ ConfigMaps and Secrets are commonly used together.

---

# 📚 What's Next?

➡️ **09. Volumes**
