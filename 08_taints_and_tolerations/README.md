# 📘 **Taints and Tolerations**

Taints and tolerations work together to **control which Pods can be scheduled on which Nodes**.

- **Taint → applied on Nodes**
- **Toleration → applied on Pods**

---

## 🚫 **Taints (Node Side)**

### ✅ **What Is a Taint?**

- A taint marks a **node** so that **pods are NOT scheduled** on it unless they explicitly allow it.
- Think of it as:
  👉 _“Do not place pods here unless they can tolerate me.”_

---

### 🏷️ **Taint Format**

```
key=value:effect
```

Example:

```
gpu=true:NoSchedule
```

---

### 📌 **Apply a Taint on a Node**

```bash
kubectl taint node <node-name> key=value:effect
```

Example:

```bash
kubectl taint node node1 gpu=true:NoSchedule
```

This means:

- Node `node1` accepts only Pods that **tolerate gpu=true**

---

### 🔍 **Verify Taint**

```bash
kubectl describe node <node-name> | grep -i taint
```

---

### ❌ **Remove (Untaint) a Node**

Add a `-` at the end:

```bash
kubectl taint node node1 gpu=true:NoSchedule-
```

---

<br>

# 🧩 **Tolerations (Pod Side)**

### ✅ **What Is a Toleration?**

- A toleration allows a **Pod** to be scheduled on a **tainted node**.
- Toleration does **not force scheduling**, it only allows it.

---

### 📄 **Add Toleration in Pod YAML**

```yaml
tolerations:
  - key: "gpu"
    operator: "Equal"
    value: "true"
    effect: "NoSchedule"
```

This means:

- Pod can run on nodes tainted with `gpu=true:NoSchedule`

---

<br>

# ⚙️ **Taint Effects (Scheduling Behavior)**

Taint **effect** decides how strictly the node rejects Pods.

### **1️⃣ NoSchedule**

- New Pods **will not be scheduled**
- Existing Pods are unaffected

### **2️⃣ PreferNoSchedule**

- Scheduler **tries to avoid** the node
- Not guaranteed (soft rule)

### **3️⃣ NoExecute**

- Affects **both new and existing Pods**
- Existing Pods without toleration are **evicted**

> ⚠️ Effect must **match** between taint and toleration.

---

<br>

# 🎯 **Node Selector (Pod Chooses Node)**

NodeSelector is used when **Pods decide where they want to run** based on node labels.

---

## 🏷️ **Add Label to Node**

```bash
kubectl label node <node-name> key=value
```

Example:

```bash
kubectl label node node1 gpu=false
```

---

### 🔍 **Check Node Labels**

```bash
kubectl get nodes --show-labels
```

---

## 📄 **Add nodeSelector in Pod YAML**

```yaml
nodeSelector:
  gpu: "false"
```

This means:

- Pod will be scheduled **only on nodes with `gpu=false`**

---

<br>

# 🔁 **Taint vs NodeSelector (Quick Comparison)**

| Feature                 | Taint & Toleration  | NodeSelector            |
| ----------------------- | ------------------- | ----------------------- |
| Applied on              | Node & Pod          | Node & Pod              |
| Who controls scheduling | Node rejects Pods   | Pod chooses Node        |
| Use case                | Restrict access     | Target specific nodes   |
| Common usage            | GPU / Special nodes | Environment-based nodes |
