# ClusterIP Service

ClusterIP is the default Kubernetes service type that provides internal cluster networking. It exposes the service on a cluster-internal IP address.

## Overview

ClusterIP is the **default and most commonly used** Kubernetes service type. It provides stable, internal networking for pod-to-pod communication within the cluster.

### Key Features:
- **Internal Only**: Accessible only from within the cluster
- **Stable IP**: Each service gets a stable virtual IP address
- **DNS Resolution**: Pods can access via DNS names
- **Load Balanced**: Automatically distributes traffic to pods
- **Default Type**: Used when you don't specify a service type

### What It Does:
1. Creates a stable IP address (virtual IP) for the service
2. Routes traffic to backend pods labeled with the service selector
3. Provides DNS names for service discovery
4. Enables internal microservices communication

### Use Cases:
- **Backend Services**: Databases, caches, queues
- **Internal APIs**: Microservices talking to each other
- **Worker Pools**: Jobs communicating with coordinators
- **Internal Tools**: Monitoring, logging, tracing systems

### Traffic Flow:
```
Pod A → Service (ClusterIP: 10.105.65.164:80) → Backend Pods (10.244.0.19:80, 10.244.0.20:80)
```

## Quick Start

### 1. Deploy with these commands
```bash
kubectl apply -f namespace.yml
kubectl apply -f service-cluster.yml
kubectl apply -f deployment-cluster.yml
```

### 2. Verify the service
```bash
kubectl get svc -n service-ns
kubectl describe svc nginx-service -n service-ns
```

Expected output:
```
Name:                     nginx-service
Namespace:                service-ns
Labels:                   <none>
Annotations:              <none>
Selector:                 app=nginx
Type:                     ClusterIP
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.105.65.164
IPs:                      10.105.65.164
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
Endpoints:                10.244.0.19:80,10.244.0.20:80
Session Affinity:         None
Internal Traffic Policy:  Cluster
Events:                   <none>
```

## Access Methods

**Port Forwarding**

Test the service using port forwarding:
```bash
kubectl port-forward -n service-ns svc/nginx-service 8080:80
```

Then open in browser or curl:
```bash
curl http://localhost:8080
```

Access in browser:
```
http://localhost:8080
```

You should see the service response (nginx welcome page, etc.)


## Verification Commands

### Check service endpoints
```bash
kubectl get endpoints nginx-service -n service-ns
```

### Check service IP
```bash
kubectl get svc nginx-service -n service-ns -o wide
```

### View service details
```bash
kubectl describe svc nginx-service -n service-ns
```

### Check events
```bash
kubectl get events -n service-ns
```


Use when:
- Service is only accessed by other pods
- Backend services (databases, caches)
- Internal microservices communication
- No external access needed

Don't use when:
- External access needed → Use NodePort or LoadBalancer
- Need to expose to internet → Use Ingress

## Service Architecture

### DNS Names
Your ClusterIP service is accessible via:
- **Full DNS**: `nginx-service.service-ns.svc.cluster.local`
- **Short DNS**: `nginx-service` (from same namespace)
- **Namespace DNS**: `nginx-service.service-ns` (cross-namespace)

### Traffic Routing
The service automatically:
1. **Discovers** pods with matching labels (selector: `app: nginx`)
2. **Tracks** their IP addresses as endpoints
3. **Load Balances** incoming requests across all healthy pods
4. **Routes** traffic to port 80 on backend pods




