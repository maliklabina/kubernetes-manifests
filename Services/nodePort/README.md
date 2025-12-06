# NodePort Service

NodePort is a Kubernetes service type that exposes the service on each node's IP address at a static port (called the NodePort). It allows external traffic to access services running inside the cluster.

## Overview

### Key Features:
- **External Access**: Accessible from outside the cluster
- **Port Range**: Uses ports 30000-32767 (can be custom)
- **All Nodes**: Service is exposed on every node in the cluster
- **Static Port**: Same port on every node
- **Internal + External**: Works like ClusterIP internally, plus external access

### What It Does:
1. Assigns a static port on each node (30000-32767)
2. Routes external traffic to the service
3. Service internally routes to backend pods
4. Traffic can come from any node, reaches any pod

### Use Cases:
- **External Services**: Expose services to external users
- **Development/Testing**: Quick external access for testing
- **On-Premises**: When load balancers aren't available
- **Web Applications**: Frontend services
- **APIs**: External API access

### Traffic Flow:
```
External Client → Node IP:NodePort (30080) → Service → Backend Pods
```



## Deployment

### 1. Deploy the service
```bash
kubectl apply -f service-nodeport.yml
```

### 2. Verify the service
```bash
kubectl get svc -n cluster-services
kubectl describe svc frontend-nodeport -n cluster-services
```

Expected output:
```
Name:                     frontend-nodeport
Namespace:                cluster-services
Labels:                   <none>
Selector:                 app=frontend
Type:                     NodePort
IP:                       10.102.67.171
Port:                     http  80/TCP
TargetPort:               80/TCP
NodePort:                 http  30080/TCP
Endpoints:                10.244.0.13:80,10.244.0.14:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```

## Access Methods

### 1. **Direct Node Access**

Get the node IP:
```bash
# For all nodes
kubectl get nodes -o wide

# Get specific node IP
NODE_IP=$(kubectl get nodes -o jsonpath='{.items[0].status.addresses[?(@.type=="ExternalIP")].address}')
if [ -z "$NODE_IP" ]; then
  NODE_IP=$(kubectl get nodes -o jsonpath='{.items[0].status.addresses[?(@.type=="InternalIP")].address}')
fi

# Access the service
curl http://$NODE_IP:30080
```

### 2. **For Minikube**
```bash
# Get Minikube IP
minikube ip

# Access via Minikube IP
curl http://$(minikube ip):30080

# Or use Minikube service command (opens in browser)
minikube service frontend-nodeport -n cluster-services
```

### 3. **For Docker Desktop**
```bash
# Docker Desktop nodes are at localhost
curl http://localhost:30080

# Or open in browser
http://localhost:30080
```

### 4. **For Cloud Clusters (AWS/GCP/Azure)**
```bash
# Get external IP of any node
kubectl get nodes -o wide

# Access via external IP
curl http://<EXTERNAL-IP>:30080
```

### 5. **Port Forwarding (Alternative)**
```bash
kubectl port-forward svc/frontend-nodeport 8080:80 -n cluster-services

# Then access
curl http://localhost:8080
```

## Verification Commands

### Check service exists
```bash
kubectl get svc frontend-nodeport -n cluster-services
```

### Check service endpoints
```bash
kubectl get endpoints frontend-nodeport -n cluster-services
```

### Check service details
```bash
kubectl describe svc frontend-nodeport -n cluster-services
```

### Check all nodes
```bash
kubectl get nodes -o wide
```

### Test from inside cluster
```bash
kubectl run -it --rm debug --image=busybox --restart=Never -n cluster-services -- wget -O- http://frontend-nodeport:80
```

### Check service events
```bash
kubectl get events -n cluster-services
```

## NodePort vs ClusterIP vs LoadBalancer

| Feature | ClusterIP | NodePort | LoadBalancer |
|---------|-----------|----------|--------------|
| **Access** | Internal only | Internal + External | Internal + External |
| **Port Range** | Any | 30000-32767 | Any |
| **External IP** | None | Node IP | Load Balancer IP |
| **Use Case** | Backend services | Development, testing | Production |
| **Cost** | Free | Free | Cloud costs |


### Traffic Path:
```
External Client:30080 
  → Node:30080 
    → Service:80 
      → Pod:80 
        → Container:80
```


```
nodePort/
├── README.md                    # This file
├── service-nodeport.yml         # NodePort service definition
└── deployment-frontend.yml      # Frontend deployment (optional)
```

