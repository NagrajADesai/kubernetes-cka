# Multi-Container Pod in Kubernetes

A **multi-container pod** allows multiple containers to run together inside a single pod. These containers share the same network namespace, storage volumes, and lifecycle. Common patterns include:

- **Init Containers**
  Containers that run **before** the main container starts. They are used for setup tasks such as waiting for dependencies, initializing files, or running checks.

- **Sidecar / Helper Containers**
  Containers that run **alongside** the main application to provide supporting functionality (logging agent, proxy, sync process, etc.).

---

## Creating a Multi-Container Pod

You can define a multi-container pod using a YAML file such as:
[`multi-container-pod.yaml`](./multi-container-pod.yaml)

When you deploy this pod, Kubernetes will ensure that **init containers run first**.
Until all init containers complete successfully, the pod status will stay in:

```
Init:0/1
```

---

## Creating a Service for Init Container Dependency

The init container in this example waits for a service named **myservice**.
To create this service, follow these steps:

### 1. Create a Deployment

```sh
kubectl create deploy nginx-deploy --image nginx --port 80
```

### 2. Expose the Deployment as a Service

```sh
kubectl expose deploy nginx-deploy --name myservice --port 80
```

This creates a ClusterIP service called **myservice** that your init container can wait for.

---

## Checking Logs of Init Container

If your pod is stuck in the init phase, check the init container logs:

```sh
kubectl logs pod/<pod-name> -c <init-container-name>
```

### Example:

```sh
kubectl logs pod/multi-container-pod -c init-myservice
```

These logs will help you understand why the init container is waiting or failing.

---

## Final Behavior

- The **main container runs only after** all init containers finish successfully.
- If any init container fails, the pod will restart it until completion.

---
