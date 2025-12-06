# Kubernetes HPA & VPA Controller (Horizontal/Vertical Pod Autoscaler) on Minikube/KIND Cluster

### Pre-requisites 
- Setup minikube on it Minikube setup.
- Ensure you have the Metrics Server installed in your Minikube cluster to enable HPA. If not already installed, you can install it using:
```bash
minikube addons enable metrics-server
```
- Check minikube cluster status and nodes :
```bash
minikube status
kubectl get nodes
```
- If you are using a Kind cluster install Metrics Server
```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
- Edit the Metrics Server Deployment
```bash
kubectl -n kube-system edit deployment metrics-server
```
- Add the security bypass to deployment under `container.args`
```bash
- --kubelet-insecure-tls
- --kubelet-preferred-address-types=InternalIP,Hostname,ExternalIP
```
- Restart the deployment
```bash
kubectl -n kube-system rollout restart deployment metrics-server
```
- Verify if the metrics server is running
```bash
kubectl get pods -n kube-system
kubectl top nodes
```
- For VPA
```bash
git clone https://github.com/kubernetes/autoscaler.git
cd autoscaler/vertical-pod-autoscaler
./hack/vpa-up.sh
```
- Verify the pods on VPA
```bash
kubectl get pods -n kube-system
```
#
## What we are going to implement:
create an deployment & service files for Apache and with the help of HPA, it will automatically scale the number of pods based on CPU utilization.
#
### Steps to implement HPA:

- Update the Deployments:
#
- Apply the updated deployments:
```bash
kubectl apply -f deployment.yaml
```
#
- Create HPA Resources
  - create HPA resources for both Apache and NGINX deployments. The HPA will scale the number of pods based on CPU utilization.
#
- Apply the HPA resources:
```bash
kubectl apply -f hpa.yaml
```
#
- port forward to access the Apache service on browser.
```bash
kubectl port-forward svc/apache-service 8081:80 --address 0.0.0.0 &
```
#
- Verify HPA
  - You can check the status of the HPA using the following command:
```bash
kubectl get hpa
```
> This will show you the current state of the HPA, including the current and desired number of replicas.

#
### Stress Testing
#
- To see HPA in action, you can perform a stress test on your deployments. Here is an example of how to generate load on the Apache deployment using 'BusyBox':
```bash
kubectl run -i --tty load-generator --image=busybox /bin/sh
```
#
- Inside the container, use 'wget' to generate load:
```bash
while true; do wget -q -O- http://apache-service.default.svc.cluster.local; done
```

This will generate continuous load on the Apache service, causing the HPA to scale up the number of pods.

#
- Now to check if HPA worked or not, open a same new terminal and run the following command
```bash
kubectl get hpa -w
```

#