# Kubernetes Taints and Tolerations — README

Taints and tolerations in Kubernetes allow you to **control which pods can be scheduled on which nodes**.  
They are used to repel or attract pods to specific nodes based on rules.

---

## Key Concepts

| Term | Description |
|------|-------------|
| **Node Taint** | Applied to a **node** to repel pods that do not tolerate it. |
| **Pod Toleration** | Applied to a **pod** to allow it to be scheduled on tainted nodes. |
| **Person** | Analogy: “person” → pod |
| **Bug** | Analogy: pod can be tolerant or intolerant |

---

## Applying a Taint to a Node

```bash
kubectl taint nodes <node-name> <key>=<value>:<effect>
```


### Example:
```bash
kubectl taint nodes node1 dedicated=high-priority:NoSchedule
```



## Taint Effects

-  NoSchedule

    - Pods that do not tolerate the taint will not be scheduled on this node.

-  PreferNoSchedule

    - Scheduler will try to avoid placing pods on this node, but it is not mandatory.

-  NoExecute

    - Pods that do not tolerate the taint will be evicted if already running.

    - Prevents new pods without toleration from being scheduled on the node.


## yaml
```bash
apiVersion: v1
kind: Pod
metadata:
  name: tolerant-pod
spec:
  containers:
  - name: app
    image: nginx
  tolerations:
  - key: "dedicated"
    operator: "Equal"
    value: "high-priority"
    effect: "NoSchedule"
```



### Problems 

- Pods can still go to nodes without a matching toleration if:

    - No other suitable nodes are available

    - Scheduler ignores soft preferences (PreferNoSchedule)