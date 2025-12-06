# Kubernetes Pods

## 1\. Pod Creation Strategies

There are two primary ways to create pods in Kubernetes:

### A. Imperative (CLI Command)

Best for quick testing or creating a single instance temporarily.

```bash
kubectl run nginx-pod --image=nginx:latest
```

### B. Declarative (YAML Configuration)

Best for production, version control, and reproducibility.

**1. Create the Configuration File (`pod.yaml`)**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    env: demo
    type: frontend
spec:
  containers:
    - name: nginx-container
      image: nginx:latest
      ports:
        - containerPort: 80
```

**2. Apply the Configuration**

```bash
kubectl apply -f pod.yaml
```

### C. Pro Tip: Dry Run (Generate YAML)

Use this to generate a YAML file template without actually creating the pod on the cluster.

```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml > dry-run-pod.yaml
```

> **Note:** The `--dry-run=client` flag ensures the command is simulated locally. The `-o yaml` flag outputs the resource definition in YAML format.

---

## 2\. Inspection & Troubleshooting

Commands to view the status and details of your pods.

| Action               | Command                           | Description                                                 |
| :------------------- | :-------------------------------- | :---------------------------------------------------------- |
| **List Pods**        | `kubectl get pods`                | Lists all pods in the default namespace.                    |
| **Extended Info**    | `kubectl get pods -o wide`        | Shows IP address and the Node where the pod is running.     |
| **Show Labels**      | `kubectl get pods --show-labels`  | Displays labels attached to the pods (useful for grouping). |
| **Detailed Inspect** | `kubectl describe pod <pod-name>` | Shows events, config, and errors (crucial for debugging).   |
| **API Reference**    | `kubectl explain pod`             | Explains the `kind` version and supported fields.           |

---

## 3\. Interaction & Modification

### Edit a Running Pod

Opens the live configuration in your default text editor (vim/nano).

```bash
kubectl edit pod <pod-name>
```

> **Note:** Upon saving and exiting the editor, changes are applied immediately. **However**, many fields in a Pod spec cannot be updated once the pod is running.

### Interactive Shell (Exec)

Get inside the container to run commands directly.

```bash
kubectl exec -it <pod-name> -- /bin/bash
```

- `-i`: Interactive (allows you to pass input)
- `-t`: TTY (allocates a terminal)

---
