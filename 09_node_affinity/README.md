# 📘 **Node Affinity**

Node affinity is an **advanced way for Pods to choose Nodes** based on **node labels**.

> It is more expressive and flexible than `nodeSelector`.

---

## ✅ **Key Points**

- Node affinity **does NOT affect existing Pods**
- It is evaluated **only during scheduling**
- Works based on **node labels**
- Used when Pods need **specific hardware, zone, or environment**

---

<br>

# 🎯 **Types of Node Affinity**

Node affinity has two main types:

---

## **1️⃣ requiredDuringSchedulingIgnoredDuringExecution**

### 📌 **Meaning**

- Pod **will be scheduled only if** the node matches the label rule
- If no node matches → **Pod stays Pending**

### 🧠 **Behavior**

- **Hard requirement**
- Ignored during execution (Pod won’t be evicted if label changes)

### ✅ **Use Case**

- GPU workloads
- High-memory nodes
- Dedicated nodes

---

## **2️⃣ preferredDuringSchedulingIgnoredDuringExecution**

### 📌 **Meaning**

- Scheduler **tries to place the Pod** on a matching node
- If no matching node is available, Pod is still scheduled elsewhere

### 🧠 **Behavior**

- **Soft requirement**
- No guarantee of placement

### ✅ **Use Case**

- Cost-optimized scheduling
- Performance preference, not strict requirement

---

<br>

# ⚙️ **Why “IgnoredDuringExecution”?**

- Once the Pod is running, **node label changes do NOT affect it**
- Pod will **not be evicted** if labels are removed or changed

---

<br>

# 🔗 **Node Affinity vs Node Selector**

| Feature     | Node Selector | Node Affinity                   |
| ----------- | ------------- | ------------------------------- |
| Flexibility | Simple        | Advanced                        |
| Conditions  | Exact match   | Expressions (In, NotIn, Exists) |
| Soft rules  | ❌ No         | ✅ Yes                          |
| Hard rules  | ✅ Yes        | ✅ Yes                          |

---

<br>

# 🧩 **Using Node Affinity with Taints & Tolerations**

### 🛡️ **Best Practice**

We often use **both** together to ensure **strict node isolation**.

### 🔒 **Why Combine Them?**

- **Taints** → Node rejects unwanted Pods
- **Tolerations** → Pod is allowed on the node
- **Node Affinity** → Pod chooses the correct node

👉 This ensures:

- Only **intended Pods** reach the node
- No accidental scheduling

---

### 🧠 **Real-World Example**

GPU Node Strategy:

- Node has taint: `gpu=true:NoSchedule`
- Pod has toleration for `gpu=true`
- Pod also has node affinity requiring `gpu=true`

✔️ Guaranteed GPU workload isolation

---

<br>

# 📝 **Key Points**

- Node affinity controls **where Pods prefer or must run**
- Does **not affect running Pods**
- Two types:

  - `required` → strict
  - `preferred` → flexible

- Best used with **taints and tolerations** for production clusters
