# Kubernetes Hands-On Practice

This repository contains **Kubernetes manifests** for practicing core Kubernetes concepts.  
The goal is to get hands-on experience with the following resources:

- Namespace
- Pod
- Deployment
- Service
- Persistent Volume (PV)
- Persistent Volume Claim (PVC)


---

## Project Structure

This repo demonstrates a simple **Kubernetes hierarchy**:
```
Namespace
└── Deployment
└── Pod(s)
└── Service
```

### **1. Namespace**
Namespaces are used to **isolate resources** in a cluster.  
Example manifest: `nginx.yaml`

### **2. Pod**

A Pod is the smallest deployable unit in Kubernetes.
Pods contain one or more containers.
Example manifest: pod.yaml

### **3. Deployment**

A Deployment manages replicas of Pods and provides rolling updates.
Example manifest: deployment.yaml

### **4. Service**

Services expose Pods internally or externally.
Example manifest: service.yaml

### **5. Persistent Storage*** 

PersistentVolume (PV) 

PersistentVolumeClaim (PVC) 


How to Use
Apply the namespace first:
``` bash
kubectl apply -f nginx.yaml
```

Apply the Deployment:
```bash
kubectl apply -f deployment.yaml
```

Apply the Service:
```bash
kubectl apply -f service.yaml
```

Apply storage resources:
```bash
kubectl apply -f persistentVolume.yaml
kubectl apply -f persistentVolumeClaim.yaml
```

Verify resources:
```bash
kubectl get ns
kubectl get pods -n nginx
kubectl get deployments -n nginx
kubectl get services -n nginx
kubectl get pv,pvc
```
