# This repository contains three multi-container Deployment examples, each demonstrating a different Pod design pattern in Kubernetes:

- 1-Co-located Containers 
- 2-Sidecar Pattern
- 3-Init-Container Pattern

##  What Is a Multi-Container Pod?

A multi-container pod contains **two or more containers that share:**

- The same **Pod network** 
- The same **storage volumes** 
- The same **lifecycle**  
- The same **namespace and IP**

---

# Co-Located Containers 
This Deployment demonstrates a normal multi-container Pod, where all containers run together at the same time and can share storage/network.
## Container Names Used:
- 1- co-located-main
- 2- co-located-helper
## How They Work
- Both containers run in parallel.
- Both stay running for the entire lifespan of the Pod.
- Ideal for workloads that must work side-by-side, such as:
  - workers
  - helper daemons


---
# Sidecar Pattern
Sidecar Starts first , does the job and continue to run with the application
## Container Names Used:
sidecar-main
sidecar
## How They Work
- Sidecar Starts first ,
- Both stay running for the entire lifespan of the Pod.
## Use Cases
- Log processors
- Sync agents
- Monitoring agents

---
# Init-Container Pattern
Init containers run before the main containers start.
They perform setup tasksa, and end.
## Container Names Used:
Init container: regular-init
Main container: regular-main
## How They Work
- Init container runs once → completes → exits.
- Pod then launches regular-main.
- Init container never runs again unless the entire Pod restarts.
## Use Cases
- Waiting for a DB
- Downloading configs





## Files Included
- README.md
- colocated-deployment.yaml
- sidecar-deployment.yaml
- init-deployment.yaml