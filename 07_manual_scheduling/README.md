# 📘 **Static Pods**

### ✅ **What Are Static Pods?**

- Static Pods are **Pods managed directly by the kubelet**, not by the Kubernetes control plane.
- The **scheduler does NOT manage** static pods.
- They are mostly used to run **control plane components** (like `kube-apiserver`, `kube-controller-manager`, `kube-scheduler`, `etcd`).

---

### ✅ **Who Manages Static Pods?**

- **kubelet** continuously watches a specific directory on the node.
- Any manifest YAML placed in that directory is **automatically created as a Pod**, without needing API server or scheduler.

---

### 🗂️ **Where Are Static Pod Manifests Stored?**

Inside your control plane node (or control plane container in kind/minikube), you will find:

```
/etc/kubernetes/manifests/
```

This directory contains YAML files for:

- kube-apiserver
- kube-controller-manager
- kube-scheduler
- etcd

> Kubelet monitors this folder and ensures all these Pods are always running.

---

### 🐳 **If You Are Using KIND**

First, enter your control-plane Docker container:

```bash
docker exec -it <control-plane-container> /bin/bash
```

Then navigate to:

```bash
cd /etc/kubernetes/manifests/
```

---

### ⚠️ **Static Pods Don’t Need the Scheduler**

Even if the scheduler is down, static pods still run because **kubelet bypasses the scheduler**.

Also, if a Pod manifest (non-static) includes:

```yaml
nodeName: <node-name>
```

…it will be scheduled **directly to that node** without needing the scheduler.

---

<br>

# 🎯 **Labels and Selectors**

### ✅ **What Are Labels?**

- Labels are **key-value pairs** attached to Kubernetes objects.
- Used to organize, identify, and filter resources.

Example:

```yaml
labels:
  app: frontend
  env: prod
```

---

### 🎛️ **Selectors**

Selectors are used by Kubernetes to **find and match resources** based on labels.

Used by:

- Deployments (to manage Pods)
- Services (to route traffic)
- Jobs / CronJobs
- ReplicaSets
- NetworkPolicies

Example of a selector:

```yaml
selector:
  matchLabels:
    app: frontend
```

This will match all Pods that have:

```yaml
app: frontend
```

---

### 🔗 **Why Are Labels & Selectors Important?**

- Connect Deployments → Pods
- Connect Services → Pods
- Group Pods together
- Perform rolling updates correctly
- Filter and query resources easily:

```bash
kubectl get pods -l app=frontend
```
