## Namespaces

Namespaces in Kubernetes provide **logical isolation** of resources inside a cluster. They help separate environments, teams, or applications and avoid naming conflicts.

### Default Kubernetes Namespaces

- **default**

  - If no namespace is specified, resources are created in the `default` namespace.

- **kube-system**

  - Contains all **control plane and system-level components** (DNS, scheduler, kube-proxy, etc.).

---

## Create a Namespace

```bash
kubectl create ns <namespace-name>
```

---

## Delete a Namespace

```bash
kubectl delete ns <namespace-name>
```

---

## Deploy Resources in a Namespace

### Create a Deployment in a Specific Namespace

```bash
kubectl create deploy <deployment-name> --image=<image-name> -n <namespace-name>
```

**Example:**

```bash
kubectl create deploy nginx-demo --image=nginx -n demo
```

---

## Create a Service in a Namespace

```bash
kubectl expose <deployment-name> --name=<svc-name> --port <port> -n <namespace-name>
```

**Example:**

```bash
kubectl expose deploy/nginx-demo --name=svc-nginx --port 80 -n demo
```

---

## Accessing Services Across Namespaces

To cURL a service or Pod in another namespace, you must use its **fully qualified domain name (FQDN)**.

### Find DNS Resolution Information

```bash
cat /etc/resolv.conf
```

### cURL Using Full Domain Name Format

```
curl <svc-name>.<namespace>.svc.cluster.local
```

**Example:**

```bash
curl svc-nginx.demo.svc.cluster.local
```

---
