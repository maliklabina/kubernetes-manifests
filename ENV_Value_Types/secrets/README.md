#  Secrets

## What is a Secret?
A **Secret** is a Kubernetes object used to store **sensitive data**, such as:
- passwords  
- API keys  
- certificates  
- tokens  
- SSH keys  

Secrets ensure that confidential information is **not stored directly in Pod specs or container images**.

Secrets are **base64-encoded**, not encrypted by default.  
However, Kubernetes provides encryption-at-rest options if enabled.

---

## Why Are Secrets Used?

### ✔ Keep sensitive data separate from container code  
### ✔ More secure than environment variables inside YAML  
### ✔ Easier rotation and management  
You can update a Secret without redeploying the entire application.
### ✔ Mount as files or inject as environment variables  
Flexible usage depending on your application needs.

---

## Example: Creating a Secret

```bash
kubectl create secret generic db-secret \
  --from-literal=username=admin \
  --from-literal=password=pass123
```

### YAML
```bash
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  username: YWRtaW4=
  password: cGFzczEyMw==
```



## **How to stored hashed value**

### Encode
```bash
echo -n 'root' | base64
->  cm9vdA
```
### Decode
```bash
echo -n 'cm9vdA' | base64 --decode
-> root
```



---
