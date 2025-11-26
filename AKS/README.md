# Kubernetes Manifests – AKS StorageClass & Dynamic Provisioning

This repository contains Kubernetes manifests demonstrating how to configure **StorageClasses**, **PersistentVolumeClaims (PVCs)**, and **dynamic provisioning** in **Azure Kubernetes Service (AKS)** using **Azure Disk**.

## 📁 Files Included

- `storageclass-high-performance.yml` – High-performance StorageClass  
- `storageclass-cost-effective.yml` – Cost-effective StorageClass  
- `high-per-pvc.yml` – PVC using the High Performace StorageClass  
- `cost-eff-pvc.yml` – PVC using the Cost Effective StorageClass  

## 🚀 How Dynamic Provisioning Works

1. A PVC requests storage with a specific StorageClass.
2. AKS dynamically provisions an **Azure Managed Disk** matching the parameters.
3. The PVC automatically binds to the newly created PersistentVolume (PV).
4. Pods can mount the PVC to use the storage.

## 🧪 How to Apply the Manifests

```sh
kubectl apply -f storageclass-high-performance.yml
kubectl apply -f storageclass-cost-effective.yml
kubectl apply -f high-per-pvc.yml
kubectl apply -f cost-eff-pvc.yml
```

Check the PVC status:
```sh
kubectl get pvc
```


All StorageClasses in this repo use Azure Disk as the storage backend (kubernetes.io/azure-disk).
