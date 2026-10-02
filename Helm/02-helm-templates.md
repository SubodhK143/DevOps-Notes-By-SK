# Helm Templates — Go Templating & Dynamic Kubernetes Manifests

Helm templates allow you to create **dynamic Kubernetes manifests** instead of hardcoding values.

Helm uses the **Go template language** to generate Kubernetes YAML from:

- `values.yaml`
- `Chart.yaml`
- Release information
- Template functions
- Built-in objects
- Conditional logic
- Loops
- Named templates

Basic flow:

```text
values.yaml
     +
Chart metadata
     +
Release information
     ↓
Helm Templates
     ↓
Rendered Kubernetes YAML
     ↓
Kubernetes Cluster
```

---

# Why Helm Templates?

Without templates, you may need separate YAML files for every environment:

```text
deployment-dev.yaml
deployment-staging.yaml
deployment-prod.yaml
```

With Helm:

```text
One Template
     +
Different Values
     ↓
Different Environments
```

Example:

```yaml
replicaCount: 2
```

Development:

```yaml
replicaCount: 2
```

Production:

```yaml
replicaCount: 5
```

The same template can be reused.

---

# Helm Template Syntax

Helm templates use:

```text
{{ ... }}
```

Example:

```yaml
replicas: {{ .Values.replicaCount }}
```

If `values.yaml` contains:

```yaml
replicaCount: 3
```

Helm renders:

```yaml
replicas: 3
```

---

# Helm Template Objects

Helm provides built-in objects that expose information to templates.

The most commonly used objects are:

```text
.Values
.Chart
.Release
.Capabilities
.Files
.Template
```

---

# `.Values`

`.Values` accesses configuration from `values.yaml`.

Example:

```yaml
# values.yaml

replicaCount: 3

image:
  repository: nginx
  tag: "1.27"
```

Template:

```yaml
spec:
  replicas: {{ .Values.replicaCount }}

  containers:
    - name: nginx
      image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
```

Rendered output:

```yaml
spec:
  replicas: 3

  containers:
    - name: nginx
      image: "nginx:1.27"
```

---

# `.Chart`

`.Chart` provides information from `Chart.yaml`.

Example:

```yaml
# Chart.yaml

apiVersion: v2
name: my-app
version: 1.0.0
appVersion: "2.0.0"
```

Template:

```yaml
metadata:
  name: {{ .Chart.Name }}
```

Rendered:

```yaml
metadata:
  name: my-app
```

Common `.Chart` properties:

```text
.Chart.Name
.Chart.Version
.Chart.AppVersion
.Chart.Description
```

---

# `.Release`

`.Release` provides information about the current Helm release.

Common properties:

```text
.Release.Name
.Release.Namespace
.Release.Revision
.Release.IsInstall
.Release.IsUpgrade
```

Example:

```yaml
metadata:
  name: {{ .Release.Name }}
  namespace: {{ .Release.Namespace }}
```

If installed using:

```bash
helm install production ./my-app \
  --namespace production
```

the template can produce:

```yaml
metadata:
  name: production
  namespace: production
```

---

# `.Release.IsInstall`

Returns `true` when the release is being installed.

Example:

```gotemplate
{{ if .Release.IsInstall }}
installation is happening
{{ end }}
```

---

# `.Release.IsUpgrade`

Returns `true` when the release is being upgraded.

```gotemplate
{{ if .Release.IsUpgrade }}
upgrade is happening
{{ end }}
```

---

# `.Release.Revision`

Returns the current release revision.

```yaml
revision: {{ .Release.Revision }}
```

Example:

```text
Revision 1 → Install
Revision 2 → Upgrade
Revision 3 → Upgrade
```

---

# `.Capabilities`

`.Capabilities` provides information about the Kubernetes cluster.

For example:

```gotemplate
{{ .Capabilities.KubeVersion.Version }}
```

You can also check whether an API version is available:

```gotemplate
{{ if .Capabilities.APIVersions.Has "networking.k8s.io/v1" }}
```

This is useful when templates need to support different Kubernetes versions.

---

# `.Files`

`.Files` allows Helm templates to access files included in the chart.

Example chart:

```text
my-app/
├── Chart.yaml
├── values.yaml
├── config/
│   └── app.conf
└── templates/
    └── configmap.yaml
```

A file can be accessed using:

```gotemplate
{{ .Files.Get "config/app.conf" }}
```

This can be useful for embedding configuration files into ConfigMaps or Secrets.

---

# `.Template`

`.Template` provides information about the current template.

Example:

```gotemplate
{{ .Template.Name }}
```

This can help when debugging or identifying which template generated a resource.

---

# Template Functions

Helm provides many built-in template functions.

Common functions include:

```text
quote
default
required
upper
lower
trim
replace
toYaml
nindent
indent
include
tpl
lookup
```

---

# `quote`

Adds quotes around a value.

```gotemplate
name: {{ .Values.appName | quote }}
```

If:

```yaml
appName: my-app
```

Output:

```yaml
name: "my-app"
```

---

# `default`

Provides a fallback value if a value is empty.

```gotemplate
replicas: {{ .Values.replicaCount | default 1 }}
```

If:

```yaml
replicaCount: 3
```

Output:

```yaml
replicas: 3
```

If the value is empty:

```yaml
replicas: 1
```

---

# `required`

Forces a value to be provided.

```gotemplate
image: {{ required "image.repository is required" .Values.image.repository }}
```

If the value is missing, Helm returns an error instead of rendering an invalid manifest.

---

# String Functions

## `upper`

```gotemplate
{{ .Values.environment | upper }}
```

Input:

```text
production
```

Output:

```text
PRODUCTION
```

---

## `lower`

```gotemplate
{{ .Values.environment | lower }}
```

Input:

```text
PRODUCTION
```

Output:

```text
production
```

---

## `trim`

```gotemplate
{{ .Values.appName | trim }}
```

Removes leading and trailing whitespace.

---

## `replace`

```gotemplate
{{ .Values.appName | replace "-" "_" }}
```

Example:

```text
my-app
```

Output:

```text
my_app
```

---

# Pipelines

Helm supports pipelines using:

```text
|
```

Example:

```gotemplate
{{ .Values.appName | upper | quote }}
```

The output of one function becomes the input of the next.

Example:

```text
.Values.appName
       ↓
     upper
       ↓
     quote
       ↓
"MY-APP"
```

---

# Conditional Logic

Helm supports conditional statements using:

```gotemplate
{{ if ... }}
{{ else }}
{{ end }}
```

Example:

```yaml
{{ if .Values.ingress.enabled }}
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: {{ .Release.Name }}
{{ end }}
```

If:

```yaml
ingress:
  enabled: true
```

the Ingress is rendered.

If:

```yaml
ingress:
  enabled: false
```

the Ingress is not rendered.

---

# `if / else`

```gotemplate
{{ if .Values.service.enabled }}
serviceEnabled: true
{{ else }}
serviceEnabled: false
{{ end }}
```

---

# Multiple Conditions

Use `and`:

```gotemplate
{{ if and .Values.ingress.enabled .Values.tls.enabled }}
```

Use `or`:

```gotemplate
{{ if or .Values.dev .Values.test }}
```

Use `not`:

```gotemplate
{{ if not .Values.autoscaling.enabled }}
```

---

# Comparison Functions

Common comparison functions:

```text
eq
ne
lt
le
gt
ge
```

Example:

```gotemplate
{{ if eq .Values.environment "production" }}
```

Another example:

```gotemplate
{{ if gt .Values.replicaCount 3 }}
```

---

# `with`

`with` changes the current context.

Instead of:

```gotemplate
{{ .Values.image.repository }}
{{ .Values.image.tag }}
{{ .Values.image.pullPolicy }}
```

you can use:

```gotemplate
{{ with .Values.image }}
repository: {{ .repository }}
tag: {{ .tag }}
pullPolicy: {{ .pullPolicy }}
{{ end }}
```

Inside the `with` block:

```text
.
```

refers to:

```text
.Values.image
```

---

# Important: `.` Context

The dot:

```text
.
```

represents the **current context**.

At the top level:

```text
.
```

represents the complete Helm context.

Inside:

```gotemplate
{{ with .Values.image }}
```

the dot becomes:

```text
.Values.image
```

This is important when working with nested templates.

---

# Loops with `range`

`range` is used to iterate over lists or maps.

Example `values.yaml`:

```yaml
ports:
  - 80
  - 443
  - 8080
```

Template:

```gotemplate
ports:
{{ range .Values.ports }}
  - {{ . }}
{{ end }}
```

Rendered output:

```yaml
ports:
  - 80
  - 443
  - 8080
```

---

# Range with Maps

Example:

```yaml
labels:
  environment: production
  team: devops
  app: nginx
```

Template:

```gotemplate
{{ range $key, $value := .Values.labels }}
{{ $key }}: {{ $value | quote }}
{{ end }}
```

Output:

```yaml
environment: "production"
team: "devops"
app: "nginx"
```

---

# Variables

Helm allows variables using:

```gotemplate
{{- $variable := value }}
```

Example:

```gotemplate
{{- $appName := .Values.appName }}

metadata:
  name: {{ $appName }}
```

Variables are useful when a value is reused multiple times.

---

# Named Templates

Named templates allow reusable template logic.

They are commonly defined in:

```text
templates/_helpers.tpl
```

Example:

```gotemplate
{{- define "my-app.name" -}}
{{ .Chart.Name }}
{{- end }}
```

The template can then be reused with:

```gotemplate
{{ include "my-app.name" . }}
```

---

# `_helpers.tpl`

A typical `_helpers.tpl` file may contain:

```gotemplate
{{/*
Create the name of the application.
*/}}
{{- define "my-app.name" -}}
{{ .Chart.Name }}
{{- end }}
```

Use it in a Deployment:

```yaml
metadata:
  name: {{ include "my-app.name" . }}
```

Use it in a Service:

```yaml
metadata:
  name: {{ include "my-app.name" . }}
```

This avoids duplicating naming logic.

---

# `include`

`include` executes a named template and returns its output.

Example:

```gotemplate
{{ include "my-app.name" . }}
```

A very common pattern is:

```gotemplate
labels:
  {{- include "my-app.labels" . | nindent 4 }}
```

This is useful because the output can be passed through another function.

---

# `indent`

`indent` adds indentation.

Example:

```gotemplate
{{ .Values.config | indent 4 }}
```

---

# `nindent`

`nindent` adds a newline and indentation.

Example:

```gotemplate
labels:
  {{- include "my-app.labels" . | nindent 4 }}
```

`nindent` is commonly used when inserting YAML generated by another template.

---

# `toYaml`

Converts an object into YAML.

Example `values.yaml`:

```yaml
resources:
  requests:
    cpu: "100m"
    memory: "128Mi"
  limits:
    cpu: "500m"
    memory: "512Mi"
```

Template:

```gotemplate
resources:
  {{- toYaml .Values.resources | nindent 2 }}
```

Rendered:

```yaml
resources:
  requests:
    cpu: "100m"
    memory: "128Mi"
  limits:
    cpu: "500m"
    memory: "512Mi"
```

---

# `tpl`

`tpl` allows a string containing a template expression to be evaluated as a template.

Example:

```yaml
# values.yaml

message: "Hello {{ .Release.Name }}"
```

Template:

```gotemplate
message: {{ tpl .Values.message . | quote }}
```

If the release is:

```text
my-app
```

the output becomes:

```yaml
message: "Hello my-app"
```

---

# Whitespace Control

Helm templates can control whitespace using:

```text
{{- 
-}}
```

Example:

```gotemplate
{{- if .Values.enabled }}
enabled: true
{{- end }}
```

The `-` removes surrounding whitespace.

This is useful for producing clean YAML.

---

# Comments

Helm template comments:

```gotemplate
{{/* This is a Helm template comment */}}
```

They do not appear in the rendered Kubernetes manifest.

YAML comments:

```yaml
# This is a YAML comment
```

remain in the rendered output.

---

# Practical Example — Deployment Template

## `values.yaml`

```yaml
replicaCount: 2

image:
  repository: nginx
  tag: "1.27"
  pullPolicy: IfNotPresent

service:
  port: 80

labels:
  environment: production
  team: devops
```

---

## `templates/deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: {{ .Release.Name }}
  labels:
    app: {{ .Chart.Name }}
    {{- toYaml .Values.labels | nindent 4 }}

spec:
  replicas: {{ .Values.replicaCount }}

  selector:
    matchLabels:
      app: {{ .Chart.Name }}

  template:
    metadata:
      labels:
        app: {{ .Chart.Name }}

    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}

          ports:
            - containerPort: {{ .Values.service.port }}
```

---

# Render the Template

Before deploying, render it locally:

```bash
helm template my-app ./my-app
```

You can save the output:

```bash
helm template my-app ./my-app > rendered.yaml
```

Then inspect:

```bash
cat rendered.yaml
```

This lets you see exactly what Kubernetes YAML Helm will generate.

---

# Debugging Templates

When a template is not rendering correctly, use:

```bash
helm lint ./my-app
```

Then:

```bash
helm template my-app ./my-app --debug
```

For a simulated installation:

```bash
helm install my-app ./my-app \
  --dry-run \
  --debug
```

A useful debugging workflow is:

```text
Template Change
      ↓
helm lint
      ↓
helm template
      ↓
Review YAML
      ↓
helm install --dry-run --debug
      ↓
Deploy
```

---

# Common Template Errors

## 1. Incorrect Indentation

Bad:

```yaml
labels:
{{ toYaml .Values.labels }}
```

Better:

```yaml
labels:
  {{- toYaml .Values.labels | nindent 2 }}
```

---

## 2. Missing Value

If a required value is missing, use:

```gotemplate
{{ required "image.repository is required" .Values.image.repository }}
```

This gives a clear error instead of generating an invalid manifest.

---

## 3. Wrong Context Inside `with`

Example:

```gotemplate
{{ with .Values.image }}
{{ .repository }}
{{ .Release.Name }}
{{ end }}
```

The second expression may not work as expected because `.` now refers to `.Values.image`.

Save the root context when necessary:

```gotemplate
{{- $root := . }}

{{- with .Values.image }}
repository: {{ .repository }}
release: {{ $root.Release.Name }}
{{- end }}
```

---

# Template Functions vs Template Objects

These are different concepts.

### Objects

Provide data:

```text
.Values
.Chart
.Release
.Capabilities
.Files
```

### Functions

Transform or process data:

```text
quote
default
required
toYaml
nindent
include
upper
lower
replace
```

Example:

```gotemplate
{{ .Values.appName | upper | quote }}
```

Here:

```text
.Values.appName → Object
upper           → Function
quote           → Function
```

---

# Helm Template Execution

When Helm processes a chart:

```text
Chart
 │
 ├── Chart.yaml
 ├── values.yaml
 └── templates/
          ↓
      Helm Engine
          ↓
   Template Evaluation
          ↓
   Functions / Conditions
          ↓
    Rendered YAML
          ↓
 Kubernetes API Server
```

Helm does **not** send the Go templates directly to Kubernetes.

Kubernetes receives the **rendered YAML manifests**.

---

# Useful Template Commands

```bash
# Render templates
helm template my-release ./my-app

# Render with custom values
helm template my-release ./my-app \
  -f values-prod.yaml

# Override a value
helm template my-release ./my-app \
  --set replicaCount=5

# Lint chart
helm lint ./my-app

# Debug rendering
helm template my-release ./my-app --debug

# Dry-run installation
helm install my-release ./my-app \
  --dry-run \
  --debug
```

---

# Template Best Practices

### 1. Keep templates readable

Avoid putting too much complex logic inside a single template.

---

### 2. Use `_helpers.tpl`

Move reusable naming and labeling logic into named templates.

---

### 3. Keep configuration in `values.yaml`

Avoid hardcoding environment-specific values.

---

### 4. Use `required` for critical values

```gotemplate
{{ required "image.repository is required" .Values.image.repository }}
```

---

### 5. Use `toYaml` + `nindent` for nested configuration

```gotemplate
{{- toYaml .Values.resources | nindent 12 }}
```

---

### 6. Always render before deploying

```bash
helm template my-app ./my-app
```

---

### 7. Validate the chart

```bash
helm lint ./my-app
```

---

# Quick Revision

```text
.Values
    ↓
Configuration from values.yaml

.Chart
    ↓
Chart metadata

.Release
    ↓
Release information

.Capabilities
    ↓
Kubernetes cluster capabilities

.Files
    ↓
Files inside the chart

Templates
    ↓
Go Template Engine
    ↓
Rendered Kubernetes YAML
```

### Most Important Syntax

```gotemplate
{{ .Values.replicaCount }}

{{ .Chart.Name }}

{{ .Release.Name }}

{{ if .Values.enabled }}
{{ end }}

{{ range .Values.items }}
{{ end }}

{{ with .Values.image }}
{{ end }}

{{ include "my-app.name" . }}

{{ toYaml .Values.resources | nindent 12 }}

{{ .Values.name | quote }}

{{ required "value is required" .Values.value }}
```

---

# Key Takeaways

- Helm templates use the **Go template language**.
- `{{ ... }}` is used to evaluate template expressions.
- `.Values` accesses chart configuration.
- `.Chart` accesses chart metadata.
- `.Release` provides release information.
- `if` enables conditional resource generation.
- `range` creates loops over lists and maps.
- `with` changes the current template context.
- `_helpers.tpl` stores reusable named templates.
- `include` allows reusable template logic.
- `toYaml` converts objects into YAML.
- `nindent` makes generated YAML fit the required indentation.
- Pipelines allow multiple functions to process a value.
- `helm template` is one of the most useful tools for debugging Helm charts.
- Kubernetes receives **rendered manifests**, not Helm templates.

> **Remember:**
>
> **Values = Configuration**  
> **Templates = Structure**  
> **Functions = Transformation**  
> **Objects = Data**  
> **Helm = Render → Generate Kubernetes YAML → Deploy**
