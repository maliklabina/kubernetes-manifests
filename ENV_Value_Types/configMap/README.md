# ConfigMaps

## What is a ConfigMap?

A **ConfigMap** is a Kubernetes object used to store non-sensitive configuration data, such as:

- environment variables
- configuration files
- command-line arguments

***ConfigMaps are meant for plain configuration, not secrets.***

## Why Are ConfigMaps Used?
- Decouple configuration from images
Change app configuration without rebuilding container images.
-  Manage configuration centrally
One ConfigMap can be used by multiple pods.
- Support environment variables, files, or config directories
- Easy updates
Pods can reload config when controllers support auto-reload.

## Example: Creating a ConfigMap
```bash
kubectl create configmap app-config \
  --from-literal=APP_MODE=production \
  --from-literal=TIMEOUT=30
```

## YAML:
```bash
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_MODE: "production"
  TIMEOUT: "30"
```


```
| Feature     | Secret                                | ConfigMap                       |
| ----------- | ------------------------------------- | ------------------------------- |
| Purpose     | Store sensitive data                  | Store non-sensitive config data |
| Storage     | Base64-encoded (optionally encrypted) | Plain text                      |
| Use in Pods | Env vars / files                      | Env vars / files                |
| Ideal for   | Passwords, keys, tokens               | App configs, settings           |
| Security    | Higher                                | Lower                           |
```