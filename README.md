# Kubernetes Manifests Collection

This repository contains various Kubernetes manifest files organized by topic and use case.

---

## Directory Structure

- **AKS**  
  Manifests specific to Azure Kubernetes Service (AKS) deployments and configurations.

- **DaemonSet**  
  YAML files to deploy DaemonSets, which ensure a pod runs on every node.

- **ENV_Value_Types**  
  Examples of different ways to pass environment variables into pods and containers.

- **Health-probes**  
  Manifests demonstrating liveness, readiness, and startup probes to monitor container health.

- **HPA-VPA**  
  Horizontal Pod Autoscaler (HPA) and Vertical Pod Autoscaler (VPA) configurations for automatic scaling.

- **Ingress**  
  Files defining Ingress resources for routing external traffic to services inside the cluster.

- **multi-containers**  
  Example manifests showing pods or deployments with multiple containers.

- **namespace**  
  Namespace definitions to logically isolate Kubernetes resources.

- **NodeAffinity**  
  Configurations using node affinity to control pod scheduling based on node labels.

- **persistentVolumes**  
  Manifests for PersistentVolumes and PersistentVolumeClaims to manage storage.

- **ReplicaSet**  
  Examples of ReplicaSet resources that maintain a stable number of pod replicas.

- **Services**  
  Service manifests for exposing pods internally or externally.

- **Taints-Tolerations**  
  Configurations demonstrating taints on nodes and tolerations in pods to control scheduling.

---

## Usage

- Each folder contains YAML manifest files relevant to the topic.  
- Apply manifests using `kubectl apply -f <folder_or_file>` command.  
- Modify manifests as needed to suit your Kubernetes cluster and application requirements.

---

## Notes

- These manifests cover foundational Kubernetes concepts and are useful for learning, testing, and real-world deployments.  
- Review and adjust resource names, labels, and configurations before deploying to production environments.

---

Feel free to explore each folder for detailed examples and best practices!

