## Daemonset in Kubernetes

### What is a daemonset?
- A DaemonSet in Kubernetes is a workload controller that ensures a pod runs on all or some nodes in a cluster
- Example: If you create a daemonset in a cluster of 3 nodes, then 3 pods will be created. No need to manage replicas.
- If you add another node to the cluster, a new pod will be automatically created on the new node.

### How it works
- A DaemonSet controller monitors for new and deleted nodes, and adds or removes pods as needed.

### What it's used for
- Node monitoring