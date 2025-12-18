# 📘 **ConfigMap and Secret**

ConfigMaps and Secrets are used to **externalize configuration** from application code and container images.

👉 Goal: **Make applications configurable without rebuilding images**

---

## 🔧 **ConfigMap**

### ✅ **What Is a ConfigMap?**

- Stores **non-confidential configuration data**
- Data is stored as **key–value pairs**
- Can be consumed by Pods as:

  - Environment variables
  - Command-line arguments
  - Files (volume mounts)

---

<br>

## ⚙️ **Create ConfigMap (Imperative Way)**

### 📌 **Using Literals**

```bash
kubectl create cm <cm-name> \
  --from-literal=<key>=<value> \
  --from-literal=<key>=<value>
```

### 📌 **Example**

```bash
kubectl create cm app-cm \
  --from-literal=firstname=jupyter \
  --from-literal=lastname=notebook
```

This creates a ConfigMap with:

```text
firstname=jupyter
lastname=notebook
```

---

<br>

## 📦 **Inject ConfigMap into Pod (Environment Variables)**

Add this inside `container.spec` in `pod.yaml`:

```yaml
env:
  - name: FIRSTNAME
    valueFrom:
      configMapKeyRef:
        name: app-cm
        key: firstname
```

Meaning:

- Environment variable `FIRSTNAME`
- Value comes from ConfigMap `app-cm`
- Key used: `firstname`

---

<br>

## 📄 **Create ConfigMap from File**

### 📌 **Command**

```bash
kubectl create cm app-cm --from-file=<file-name>
```

### 🧠 **Behavior**

- File name → key
- File content → value

Example:

```bash
kubectl create cm app-cm --from-file=app.properties
```

This stores:

```text
app.properties: <file content>
```

---

<br>

## 🔐 **Secret (Brief Overview)**

### ✅ **What Is a Secret?**

- Used to store **sensitive data**
- Examples:

  - Passwords
  - API keys
  - Tokens

- Stored in **base64-encoded format**

> ⚠️ Secrets are not encrypted by default (only encoded)

---

## 🔄 **ConfigMap vs Secret**

| Feature   | ConfigMap     | Secret      |
| --------- | ------------- | ----------- |
| Data type | Non-sensitive | Sensitive   |
| Encoding  | Plain text    | Base64      |
| Usage     | App config    | Credentials |
| Security  | Low           | Higher      |

---

<br>

# 📝 **Best Practices**

- Never hardcode config inside images
- Use ConfigMaps for:

  - App configs
  - Feature flags

- Use Secrets for:

  - Passwords
  - Tokens

- Avoid storing Secrets in Git
- Use environment variables or volume mounts
