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
  - log shippers
  - workers
  - helper daemons
  - heartbeat containers

---
# Sidecar Pattern
Sidecar containers add functionality to the main container.
They run in parallel with the main application.
## Container Names Used:
sidecar-main
sidecar
## How They Work
- sidecar-main serves an NGINX website using /usr/share/nginx/html.
- sidecar writes timestamps into a shared volume every 5 seconds.
- Both containers share an emptyDir volume.
- Both containers must be healthy for the Pod to stay running.
## Use Cases
- Log processors
- Sync agents
- Monitoring agents
- File updaters
- Reverse proxies

---
# Init-Container Pattern
File: init-deployment.yaml
Init containers run before the main containers start.
They perform setup tasks, validations, configuration, or waiting operations.
## Container Names Used:
Init container: regular-init
Main container: regular-main
## How They Work
- Init container (regular-init) runs once → completes → exits.
- Pod then launches regular-main.
- Init container never runs again unless the entire Pod restarts.
## Use Cases
- Waiting for a DB
- Downloading configs
- Preparing volumes
- Pre-processing files




## Files Included
- README.md
- colocated-deployment.yaml
- sidecar-deployment.yaml
- init-deployment.yaml