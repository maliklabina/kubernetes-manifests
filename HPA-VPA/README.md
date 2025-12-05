# Kubernetes Autoscaling: HPA and VPA Demo

This repository demonstrates **Horizontal Pod Autoscaler (HPA)** and **Vertical Pod Autoscaler (VPA)** in Kubernetes. Tested on **Minikube**.

---

## Table of Contents

- [Overview](#overview)  
- [Horizontal Pod Autoscaler (HPA)](#horizontal-pod-autoscaler-hpa)  
  - [Install Metrics Server](#install-metrics-server)  
  - [Apply HPA](#apply-hpa)  
  - [Generate Load](#generate-load)  
- [Vertical Pod Autoscaler (VPA)](#vertical-pod-autoscaler-vpa)  
  - [Ways to Use VPA](#ways-to-use-vpa)  
  - [Install VPA](#install-vpa)  
  - [Apply VPA](#apply-vpa)  
  - [Generate Load](#generate-load-vpa)  
  - [VPA Update Modes](#vpa-update-modes)  
- [Feature Comparison](#feature-comparison)  


---

## Overview

Kubernetes provides autoscaling mechanisms to manage pod resources dynamically:

- **HPA (Horizontal Pod Autoscaler):** Automatically scales **number of pods** based on CPU/memory or custom metrics.  
- **VPA (Vertical Pod Autoscaler):** Automatically scales **CPU and memory resources** of pods based on usage.

---

## Horizontal Pod Autoscaler (HPA)

### Install Metrics Server

HPA requires the Metrics Server to get pod CPU/memory metrics:

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

```bash
Verify installation:
```
```bash
kubectl get deployment metrics-server -n kube-system
```

### Apply HPA

```bash
kubectl apply -f deployment.yml
kubectl apply -f vpa.yml
```

### Generate Load

To generate the traffic using another pod, create a pod using 

```bash
kubectl run -it load-generator --image=busybox -n apache
```

And run 
```bash
while true; do wget -q -O /dev/null http://apache-service.apache.svc.cluster.local; done
 ```


 This will continuously send HTTP requests to your service 


## Vertical Pod Autoscaler (VPA)

### Ways to Use VPA

1-Using CRDs (Custom Resource Definitions)
2-Installing the VPA controller

Install VPA
# Clone VPA repository
```bash
git clone https://github.com/kubernetes/autoscaler.git
cd autoscaler/vertical-pod-autoscaler
```

## Install VPA 
```bash
./hack/vpa-up.sh
```


## Apply VPA

```bash
kubectl apply -f deployment.yml
kubectl apply -f vpa-demo.yaml
```

## Generate Load (VPA)

```bash
kubectl run -it load-generator --image=busybox -n apache
```

And run 
```bash
while true; do wget -q -O /dev/null http://apache-service.apache.svc.cluster.local; done
 ```


## VPA Update Modes

```
| Mode    | Updates Pods?        | Evicts Pods? | Recommendations Only? | Use Case                                        |
| ------- | -------------------- | ------------ | --------------------- | ----------------------------------------------- |
| Off     | No                   | No           | Yes                   | Observe recommendations safely                  |
| Initial | Only on pod creation | No           | Partially             | New pods optimized, existing pods untouched     |
| Auto    | Yes                  | Yes          | Yes                   | Dev/test or dynamic workloads, can restart pods |
```

## Feature Comparison: VPA vs HPA
```

| Feature              | VPA                            | HPA                          |
| -------------------- | ------------------------------ | ---------------------------- |
| Scaling Method       | Vertical (CPU/Memory of pods)  | Horizontal (Number of pods)  |
| Pod Behaviour        | Evicts pods to apply resources | Scales pods without eviction |
| Handle Traffic Spike | No automatic pod increase      | Yes, scales number of pods   |
| Optimized Cost       | Optimizes pod resources        | Optimizes number of pods     |
| Best For             | Steady workloads, memory bound | Variable traffic, CPU bound  |

```