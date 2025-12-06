## Kubernetes Services

Kubernetes **Services** provide stable networking and load-balancing for Pods. Since Pods are ephemeral and their IPs can change, Services ensure consistent access to your application.

### 1. **ClusterIP (Default)**

- Provides **internal communication** within the cluster.
- Accessible **only inside the cluster**.
- Useful for backend services, internal microservice communication, or DB access.

### 2. **NodePort**

- Exposes a Service **outside the cluster** using a port on each node.
- Kubernetes allocates a port in the range **30000–32767**.
- External traffic → `<NodeIP>:<NodePort>` → Service → Pod.
- Simple but not recommended for production at scale.

### 3. **LoadBalancer**

- Provisions a **cloud provider load balancer**.
- Assigns a **public IP** to your service.
- External traffic is forwarded through the LB → NodePort → Service → Pod.
- Best for production apps where you need direct external access.

### 4. **ExternalName**

- Maps a Kubernetes Service to an **external DNS name**.
- No actual Service proxying; Kubernetes returns a CNAME record.
- Useful for accessing external databases or APIs using standard Kubernetes service names.

---

## What is an Endpoint?

- An **Endpoint** represents the **IP addresses of Pods** that a Service routes traffic to.
- When you create a Service, Kubernetes automatically creates an **Endpoint object**.
- If Pods scale up/down or change IPs, the Endpoint list updates automatically.
- Essentially:
  **Service = stable access point**
  **Endpoints = actual Pod IPs the Service is listening to**

---
