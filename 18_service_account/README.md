# Kubernetes Service Accounts & ImagePull Secrets

## 🔹 Service Accounts

Service Accounts are used by applications, bots, or non-human users to interact with the Kubernetes API.

### Common Examples

- Jenkins
- Prometheus
- Datadog

---

## 📌 View Service Accounts

```bash
kubectl get sa
```

View service accounts across all namespaces:

```bash
kubectl get sa -A
```

Filter default service accounts:

```bash
kubectl get sa -A | grep default
```

Describe a service account:

```bash
kubectl describe sa default
```

---

## 🛠 Create a New Service Account

```bash
kubectl create sa <service-account-name>
```

### Example:

```bash
kubectl create sa build-sa
```

Verify:

```bash
kubectl get sa
```

---

## 🔐 Create a Secret

Apply a secret from a YAML file:

```bash
kubectl apply -f secret.yaml
```

List secrets:

```bash
kubectl get secret
```

Describe a specific secret:

```bash
kubectl describe secret build-robot-secret
```

---

## 🔎 Check Permissions (RBAC)

Check if a service account can perform an action:

```bash
kubectl auth can-i get pods --as=system:serviceaccount:default:build-sa
```

If you get **"no"**, you need to create a Role and RoleBinding.

---

## 🧩 Create Role and RoleBinding

### Create Role

```bash
kubectl create role build-role \
  --verb=list,get,watch \
  --resource=pods
```

### Create RoleBinding

```bash
kubectl create rolebinding rb \
  --role=build-role \
  --serviceaccount=default:build-sa
```

---

### ✅ Verify Permissions Again

```bash
kubectl auth can-i get pods --as=system:serviceaccount:default:build-sa
```

Now you should get:

```
yes
```

Test it:

```bash
kubectl get pods --as=system:serviceaccount:default:build-sa
```

---

## ❌ Delete a Service Account

List service accounts:

```bash
kubectl get sa
```

Delete a service account:

```bash
kubectl delete sa <service-account-name>
```

Example:

```bash
kubectl delete sa build-sa
```

---

# 🔑 Create ImagePull Secret

An ImagePull Secret is required when pulling images from a **private container registry**.

---

## 📌 Create Docker Registry Secret

```bash
kubectl create secret docker-registry my-registry-secret \
  --docker-server=<registry-server> \
  --docker-username=<username> \
  --docker-password=<password> \
  --docker-email=<email>
```

### Example (Docker Hub):

```bash
kubectl create secret docker-registry dockerhub-secret \
  --docker-server=https://index.docker.io/v1/ \
  --docker-username=myusername \
  --docker-password=mypassword \
  --docker-email=myemail@example.com
```

Verify:

```bash
kubectl get secret
```

---

## 🔗 Attach ImagePull Secret to a Service Account

```bash
kubectl patch serviceaccount build-sa \
  -p '{"imagePullSecrets": [{"name": "dockerhub-secret"}]}'
```

Verify:

```bash
kubectl describe sa build-sa
```

---

## 🧾 Use ImagePull Secret in a Pod (Alternative Method)

Instead of attaching it to a Service Account, you can define it directly in your Pod YAML:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: private-app
spec:
  containers:
    - name: app
      image: myprivateimage:latest
  imagePullSecrets:
    - name: dockerhub-secret
```

Apply:

```bash
kubectl apply -f pod.yaml
```

---

# ✅ Summary

- Service Accounts are used by applications inside Kubernetes.
- RBAC (Role & RoleBinding) controls permissions.
- Use `kubectl auth can-i` to verify access.
- ImagePull Secrets allow pulling images from private registries.
- Attach ImagePull Secrets to Service Accounts or define them in Pod specs.
