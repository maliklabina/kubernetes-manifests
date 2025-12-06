## What is Ingress?
Ingress is a Kubernetes API object that manages external access to services running inside the cluster.  
It acts like a smart router or entry point, allowing you to expose HTTP and HTTPS routes from outside the cluster to internal services.

Ingress allows you to:
- Route traffic based on URL paths
- Route traffic based on hostnames
- Perform SSL/TLS termination
- Load balance multiple backend services

Instead of exposing each service individually, Ingress provides a single, unified way to control traffic entering your cluster.

---

## Why is Ingress Used?
Ingress is used because it simplifies and centralizes external access for your applications.

### Benefits:
- **Reduced number of LoadBalancers** (one Ingress can handle many services)
- **Flexible routing rules** (paths, domains, rewrites)
- **Centralized HTTPS/TLS management**
- **Lower cost** compared to multiple LoadBalancers
- **Cleaner, organized traffic flow for microservices**

---

## What is an Ingress Controller?
An Ingress Controller is a component inside Kubernetes that watches and implements Ingress rules.  
The Ingress resource by itself does nothing — the controller performs the real work.

Popular Ingress Controllers:
- NGINX Ingress Controller
- AWS ALB Ingress Controller
- GKE Ingress Controller
- Traefik
- Istio Gateway (advanced)

---

## Why Use an Ingress Controller?
You need an Ingress Controller because Kubernetes does **not** include one by default.

The controller is responsible for:
- Reading Ingress YAML definitions
- Creating routing rules
- Managing load balancers or proxies
- Handling TLS termination
- Applying paths, host rules, rewrites, redirects
- Logging and metrics for incoming traffic

Without an Ingress Controller, the Ingress object is ignored and routing will not work.

---

## Different Routing Options in Ingress

### 1. Host-based Routing
Routes traffic based on the hostname.

```yaml
host: wear.example.com
```


### 2. Path-based Routing
Routes traffic based on URL paths.

```bash
paths:
  - path: /wear
  - path: /watch
```

