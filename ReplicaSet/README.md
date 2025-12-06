# Kubernetes ReplicaSet — README

This document explains **what a ReplicaSet is, why it is used, and how it works** in Kubernetes.

---

## What is a ReplicaSet?

A **ReplicaSet (RS)** is a Kubernetes object that ensures a **specified number of pod replicas** are running at any given time.  

It continuously monitors the cluster and:

- Creates new pods if the number of replicas falls below the desired count
- Deletes extra pods if there are more than desired
- Maintains **high availability** of applications

> ReplicaSets work at the **pod level**, ensuring the desired number of identical pods are always running.

---

## Why Use a ReplicaSet?

- **Ensure high availability**: If a pod crashes, the ReplicaSet automatically replaces it.  
- **Scalability**: Easily increase or decrease the number of pod replicas.  
- **Declarative management**: You define the desired state (number of replicas) and Kubernetes maintains it.  
- **Foundation for Deployments**: Deployments use ReplicaSets to manage rolling updates and scaling.

---

## ReplicaSet Anatomy

### Key Fields

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-app-rs
spec:
  replicas: 3                  # Desired number of pods
  selector:                    # Label selector for pods
    matchLabels:
      app: my-app
  template:                    # Pod template
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: my-app:latest
        ports:
        - containerPort: 80
```

### Explanation:

- replicas – How many identical pods you want running.

- selector – Defines which pods the ReplicaSet manages. Must match the pod template labels.
 
- template – The pod specification to use when creating new pods.


### How ReplicaSets Work

- 1 You create a ReplicaSet with replicas: N.

- 2 Kubernetes compares N with the number of running pods matching the selector.

- 3 Kubernetes creates or deletes pods to match the desired replica count.

- 4 If a pod crashes or a node fails, the ReplicaSet creates a replacement pod on another node.


### Scaling a ReplicaSet
Using kubectl scale:
```bash
kubectl scale rs my-app-rs --replicas=5
```
This increases the pod count to 5.
Kubernetes will automatically add 2 more pods to reach the desired state.


### Checking ReplicaSet Status
```bash
kubectl get rs
kubectl describe rs my-app-rs
kubectl get pods -l app=my-app
```
