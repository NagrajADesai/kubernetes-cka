# Persistent Volumes (PV) and Persistent Volume Claims (PVC)

This guide explains how to create and use **Persistent Volumes (PV)** and **Persistent Volume Claims (PVC)** in Kubernetes.
All YAML files referenced below are available in the **current directory**.

---

## 1. Create a Pod with Temporary Storage

To create a pod with **ephemeral (temporary) storage**, apply the following configuration:

```bash
kubectl apply -f redis.yaml
```

This pod will use temporary storage that is **deleted when the pod is removed**.

---

# Persistent Storage in Kubernetes

To store data **persistently**, Kubernetes uses:

- **Persistent Volume (PV)** – The actual storage resource.
- **Persistent Volume Claim (PVC)** – A request for storage by a user/pod.

---

## 2. Access Modes

Kubernetes defines different access modes that control how volumes can be mounted.

### ReadWriteOnce (RWO)

- The volume can be mounted as **read-write by a single node**.
- Multiple pods can access the volume **only if they are running on the same node**.

### ReadOnlyMany (ROX)

- The volume can be mounted as **read-only by multiple nodes**.

### ReadWriteMany (RWX)

- The volume can be mounted as **read-write by multiple nodes simultaneously**.

### ReadWriteOncePod (RWOP)

- The volume can be mounted as **read-write by only one Pod across the entire cluster**.
- Ensures strict **single-pod access**.

---

## 3. Create a Persistent Volume

Apply the PV configuration:

```bash
kubectl apply -f pv.yaml
```

Verify the PV:

```bash
kubectl get pv
```

---

## 4. Create a Persistent Volume Claim

Apply the PVC configuration:

```bash
kubectl apply -f pvc.yaml
```

Verify the PVC:

```bash
kubectl get pvc
```

When the PVC is created, Kubernetes **binds it to a matching PV**.

---

## 5. Use the PV in a Pod

Once the PVC is bound to a PV, it can be mounted inside a pod.

```bash
kubectl apply -f use_pv.yaml
```

This pod will now use **persistent storage**, meaning the data will remain even if the pod restarts.

---

✅ **Summary**

| Component | Purpose                                |
| --------- | -------------------------------------- |
| PV        | Actual storage resource in the cluster |
| PVC       | Request for storage                    |
| Pod       | Uses the storage through the PVC       |

---
