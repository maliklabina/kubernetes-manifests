# Kubernetes Persistent Volumes (PV) and Persistent Volume Claims (PVC) on Minikube

This demo shows how to persist pod data using a Persistent Volume and Persistent Volume Claim on a Minikube cluster.



# What is a Persistent Volume (PV)?

A **Persistent Volume** is a piece of storage in the cluster that has been provisioned by an administrator or dynamically created by a storage class.


# What is a Persistent Volume Claim (PVC)?
 A Persistent Volume Claim is a request for storage by a user.
 Pods do not directly use PVs — they use PVCs, which bind to a suitable PV.
 Key points:
### PVC asks for:
- Size (e.g., 500Mi)
- Access mode
- StorageClass
- Kubernetes automatically finds a matching PV.
- If no PV exists, dynamic provisioning may happen (if a StorageClass is present).

---

## Prerequisites
- A VM or local machine with Minikube installed  
- kubectl configured to talk to Minikube  

---

## What We Will Implement
- Create a Persistent Volume (PV)
- Create a Persistent Volume Claim (PVC)
- Attach PVC to a pod to store data
- Verify that the data persists even after pod restart

---

## Start Minikube
```bash
minikube start
```


## Check Minikube status and nodes
```bash
minikube status
kubectl get nodes
```


Apply
```bash
kubectl apply -f persistent-volume.yaml
kubectl apply -f persistent-volume-claim.yaml
kubectl apply -f pod.yaml
```



## Exec into the Pod
```bash
kubectl exec -it nginx-pod -- sh
```
