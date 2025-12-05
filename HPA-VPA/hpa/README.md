To generate the traffic using another pod, create a pod using 

```bash
kubectl run -it load-generator --image=busybox -n apache
```

And run 
```bash
 while true; do wget -q -O /dev/null http://apache-service.apache.svc.cluster.local; done
 ```


 This will continuously send HTTP requests to your service 

