# Use Kubectl
primary CLi tool
Control your kubernetes Cluster
  Operations : what you want to do 
  Resource :- what you want to do it to
  Output :- if theres output , its format

   
https://kubernetes.io/docs/reference/kubectl/cheatsheet/
https://kubernetes.io/docs/reference/kubectl/kubectl/



# Application Deployment in kubernetes
## Imperative
```
kubectl create deployement nginx  --image= ngnix
kubectl run nginx --image= nginx
```

Declarative
   Define our desired state in code
   Manifest
   Yaml or Json
``` bash
 kubectl apply -f deployement.yaml  
```

basic Manifest - Deployment
``` yaml
apiVersion: apps/v1
kind: Deployment
metaData:
  name: hello-world
spec:
  replicas:1 
  selector:
    matchLabels:
      app:hello-world
  template:
    metadata:
     labels:
	    app: hello-world
	spec:
	  containers:
	 - image : gcr.io/google-samples/hello-app:1.0
	   name: hello-app
```



when containerd is your conatiner user crictl to get a listing of the containers running
``` bash
sudo crictl --runtime-endpoint unix:///run/containerd/container.sock ps
kubectl describe replicaSet
```


how to know how to define what to define :
 https://kubernetes.io/docs/reference/kubernetes-api

