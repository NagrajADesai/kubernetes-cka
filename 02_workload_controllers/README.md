# Kubernetes Workload Controllers

## 1\. ReplicationController (RC)

> **Note:** This is considered a **legacy** controller. It is generally recommended to use Deployments and ReplicaSets instead.

- **Function:** Ensures a specified number of pod replicas are running at all times.
- **Traffic:** Can be used with Services to load balance traffic across replicas.

### Commands

**Check documentation:**

```bash
kubectl explain rc
```

**Create RC:**

```bash
# Create the resource from a file
kubectl apply -f replication_controller.yaml
```

**Verify:**

```bash
kubectl get rc
```

---

## 2\. ReplicaSet (RS)

- **Function:** The next-generation ReplicationController.
- **Key Difference:** It supports richer selector logic (Set-based selectors), allowing it to "adopt" and manage existing pods that match its labels, even if it didn't create them.

### Commands

**Create RS:**

```bash
kubectl apply -f replicaset.yaml
```

**Verify:**

```bash
kubectl get rs
```

**Scale Manually:**
Resize the number of replicas on the fly.

```bash
kubectl scale --replicas=5 rs <replicaset-name>
```

---

## 3\. Deployment (Deploy)

- **Hierarchy:** User manages **Deployment** → Deployment manages **ReplicaSet** → ReplicaSet manages **Pods**.
- **Key Features:** Enables declarative updates, zero-downtime rollouts, and version rollbacks.

### Creation

**Imperative (Dry Run to generate YAML):**

```bash
kubectl create deploy nginx-new --image=nginx:latest --dry-run=client -o yaml > dry_deployment.yaml
```

**Declarative (Apply file):**

```bash
kubectl apply -f deployment.yaml
```

### Inspection

**View Deployments:**

```bash
kubectl get deploy
```

**View Everything (Deploy, RS, Pods):**

```bash
kubectl get all
```

### Updates & Rollouts (Lifecycle)

**1. Update Image (Trigger Rollout):**
Updates the container image, creating a new ReplicaSet and gradually moving pods to it.

```bash
# Syntax: kubectl set image deploy <deploy-name> <container-name>=<new-image>
kubectl set image deploy nginx-deployment nginx-container=nginx:1.9.1
```

**2. Check History:**
View previous versions/revisions of the deployment.

```bash
kubectl rollout history deploy <deployment-name>
```

**3. Rollback:**
Undo the last change and revert to the previous stable state.

```bash
kubectl rollout undo deploy <deployment-name>
```

---

### Summary: Which one to use?

| Controller                | Manages     | Key Feature          | Use Case                        |
| :------------------------ | :---------- | :------------------- | :------------------------------ |
| **ReplicationController** | Pods        | Simple replication   | **Legacy** (Avoid)              |
| **ReplicaSet**            | Pods        | Richer selectors     | Backend for Deployments         |
| **Deployment**            | ReplicaSets | Rollouts & Rollbacks | **Standard** for Stateless Apps |

---
