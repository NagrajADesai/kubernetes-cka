# 📘 **Health Probes in Kubernetes**

Health probes are used by Kubernetes to **monitor the health of containers** and **take corrective actions automatically**.

👉 Goal: **Keep applications healthy and available**

---

## ✅ **Why Health Probes Are Important**

- Detect unhealthy containers
- Automatically restart failed applications
- Prevent traffic to unready Pods
- Support zero-downtime deployments

---

<br>

# 🩺 **Types of Health Probes**

Kubernetes supports **three types of probes**, each serving a different purpose.

---

## **1️⃣ Startup Probe**

### 📌 **Purpose**

- Used for **slow-starting or legacy applications**
- Tells Kubernetes **when the application has successfully started**

### 🧠 **Behavior**

- While startup probe is failing:

  - Liveness and readiness probes are **disabled**

- Once startup probe succeeds:

  - Liveness and readiness probes begin

### ✅ **Use Case**

- Applications that take a long time to initialize (e.g., JVM apps, large models loading)

---

<br>

## **2️⃣ Readiness Probe**

### 📌 **Purpose**

- Checks whether the application is **ready to accept traffic**

### 🧠 **Behavior**

- If readiness probe fails:

  - Pod is **removed from Service endpoints**
  - Traffic is **stopped**

- Pod is **NOT restarted**

### ✅ **Use Case**

- Temporary unavailability
- Dependency failures (DB, cache, API)

---

<br>

## **3️⃣ Liveness Probe**

### 📌 **Purpose**

- Checks whether the application is **still running correctly**

### 🧠 **Behavior**

- If liveness probe fails:

  - Kubernetes **restarts the container**

### ✅ **Use Case**

- Deadlocks
- Infinite loops
- Application crashes

---

<br>

# 🔍 **Health Check Methods**

Each probe can perform **three types of checks**.

---

## **1️⃣ HTTP Probe**

- Sends an HTTP request to a specific endpoint
- Success based on HTTP status code (200–399)

Example:

```yaml
httpGet:
  path: /health
  port: 8080
```

---

## **2️⃣ TCP Probe**

- Checks if a TCP connection can be established
- Useful for non-HTTP applications

Example:

```yaml
tcpSocket:
  port: 3306
```

---

## **3️⃣ Exec (Command) Probe**

- Executes a command inside the container
- Exit code `0` = success

Example:

```yaml
exec:
  command:
    - cat
    - /tmp/healthy
```

---

<br>

# 🔄 **Probe vs Action (Quick Comparison)**

| Probe Type | Failure Action           |
| ---------- | ------------------------ |
| Startup    | Blocks other probes      |
| Readiness  | Removes Pod from traffic |
| Liveness   | Restarts container       |

---

<br>

# ⚙️ **Best Practices**

- Always configure **readiness probes** for production
- Use **startup probes** for slow applications
- Avoid aggressive **liveness probes**
- Separate readiness and liveness logic
- Tune `initialDelaySeconds`, `periodSeconds`, and `failureThreshold`

---

<br>

# 📌 **Summary**

- Health probes ensure application stability
- Three probe types:

  - Startup → app has started
  - Readiness → app is ready
  - Liveness → app is alive

- Three check methods:

  - HTTP
  - TCP
  - Command

- Probes enable self-healing and zero-downtime deployments
