# 📘 **Resource Requests and Limits**

Kubernetes uses **requests and limits** to manage **CPU and memory** usage for Pods.

---

## ✅ **Resource Request**

- The **minimum amount of resources** guaranteed to a Pod.
- Kubernetes **reserves** this resource on the node **at scheduling time**.
- Scheduler uses **requests** to decide **where to place the Pod**.

📌 Example:

```yaml
resources:
  requests:
    cpu: "250m"
    memory: "256Mi"
```

---

## 🚫 **Resource Limit**

- The **maximum amount of resources** a Pod can use.
- Prevents a Pod from consuming **all node resources**.

📌 Example:

```yaml
resources:
  limits:
    cpu: "500m"
    memory: "512Mi"
```

---

## 🧠 **Request vs Limit (Key Difference)**

| Feature             | Request            | Limit              |
| ------------------- | ------------------ | ------------------ |
| Purpose             | Guaranteed minimum | Maximum allowed    |
| Used by Scheduler   | ✅ Yes             | ❌ No              |
| Enforced by kubelet | ❌ No              | ✅ Yes             |
| Exceed behavior     | Allowed            | Throttled / Killed |

---

<br>

# ⚙️ **CPU vs Memory Behavior**

### 🖥️ **CPU**

- Exceeding CPU limit → **Throttling**
- Pod is slowed down, not killed

### 🧠 **Memory**

- Exceeding memory limit → **OOMKilled**
- Pod is terminated immediately

---

<br>

# 📊 **metrics-server**

### ✅ **What Is metrics-server?**

- A cluster-wide component that collects **CPU and memory usage**
- Exposes metrics via Kubernetes Metrics API
- Required for:

  - `kubectl top`
  - HPA (Horizontal Pod Autoscaler)

---

## 🔍 **Check Resource Utilization**

### **Node Resource Usage**

```bash
kubectl top node
```

---

### **Pod Resource Usage**

```bash
kubectl top pod <pod-name> -n <namespace>
```

---

<br>

# 🚦 **Different Scheduling & Runtime Scenarios**

### **1️⃣ App Uses Less Than Requested**

- Pod runs **normally**
- Unused resources stay reserved
- Node may appear underutilized

---

### **2️⃣ App Uses More Than Limit**

- **Memory** → Pod gets `OOMKilled`
- **CPU** → Pod gets throttled

---

### **3️⃣ Requested Resources > Node Capacity**

- Pod stays in **Pending** state
- Scheduler error: `Insufficient resources`

```bash
kubectl describe pod <pod-name>
```

---

<br>

# 📝 **Best Practices**

- Always define **requests** for predictable scheduling
- Set **limits** to protect node stability
- Avoid setting requests == limits for bursty workloads
- Use metrics-server to **monitor and tune resources**
