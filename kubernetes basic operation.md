# Use Kubectl
primary CLi tool
Control your kubernetes Cluster
  Operations : what you want to do 
  Resource :- what you want to do it to
  Output :- if theres output , its format

 # Operations
  apply/create : create resource
  run - start a pod from an image
  explian - documentation of  resources
  delete - delete resource
  get : list resource
  describe : detailes resource information
  exec :- execure a command on a container
  logs : View logs on a container
  
  # Resource
   nodes (no) 
   pods (po)
   services (svc)
   and many more

## Output
 specitfy kubectl's output format
  1) wide - output additional info 
  2) yaml -yaml formatted Api object
  3) json- Json formateed Api Object
  4) dry-run - print an Object without sending it to the Api Server.



   
https://kubernetes.io/docs/reference/kubectl/cheatsheet/
https://kubernetes.io/docs/reference/kubectl/kubectl/

**Listing and inspectiong your cluster**
``` bash
kubctl cluster-info
```

 node information
``` bash
kubectl get nodes -o wide
kubectl get no
```

 pods information
```
kubectl get pods
```

pods information for system pods
```
kubectl get pods --namespace kube-system
```

 all information about the pods 
``` bash
kubectl get all --all-namespaces | more
``` 
list the  resource available to us
``` bash
kubectl api-resources | more
```

find out the alias and all 
``` bash
kubectl api-resources | grep  pod
```

Explain an individual resource in detail.
``` 
 kubectl explain pod | more
 kubectl explain pod.spec | more
 kubectl explain pod.spec.containers | more
 kubectl explian pod --recursive | more
```

Detail information  for the nodes with labels 
``` bash
kubctl describe nodes  c1-cp1 | more
kubectl describe nodes  c1-nodes | more
```

asking for help in kubernetes
``` shell
kubectl -h | more
kubectl get -h | more
kubectl create -h | more
```

setting bash auto-completion

``` shell
sudo apt-get install -y bash-completion
echo "source <(kubectl completion bash)">> ~/.bashrc

source ~/.bashrc

```


# Application Deployment in kubernetes
## Imperative
kubectl create deployement nginx  --image= ngnix
kubectl run nginx --image= nginx

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

``` dry-run 
kubectl create deployment hello-world \
--image= grc.io/google-samples/hello-app:1.0 \
--dry-run= client -o yaml > deployment.yaml

```

how to know how to define what to define :
 https://kubernetes.io/docs/reference/kubernetes-api

# Deployments Demo
Deploying resources imperatively in your cluster
kubecl create deployment , create a deployment with one replica in it 
this is pulling a simple a hello-world app container image from google container registry
```bash

kubectl create deployment hello-world --image=gcr.io/google.samples/hello-app:1.0

```

lets deply a single bare pod thats not manage by a contooler

```bash
kubectl run hello-world-pod --image=gcr.io/google.samples/hello-app:1.0
```

when containerd is your conatiner user crictl to get a listing of the containers running
``` bash
sudo crictl --runtime-endpoint unix:///run/containerd/container.sock ps
```

get the logs for the data 

```bash
 kubectl logs hello-world-pod
```

exec into the pod .
```kubectl 
kubectl exec -it hello-world-pod -- /bin/sh
hostname
ip addr
exit
```

for getting the replicaset
``` bash

kubectl get replicaset

```

describing differnet objects 
``` 
kubectl describe deployement hello-world
kubectl describe replicaSet
```

# Exposing deployment
```  bash
kubectl expose deployenet hello-world \
--port=80 \
--targer-port=8080

```


```bash
kubectl get service hello-world
```

``` bash
kubectl describe service hello-world
```

Get the  represention of the deployment in  yaml.
``` bash
kubectl get deployment hello-world -o yaml | more

```

Removing our services

``` bash

kubectl get all
kubectl delete service hello-world
kubectl delete pod hello-world-pod
kubectl get all

```
# Declarative deployment
```bash
kubectl create deployment hello-world \
    --image=phozzy/hello-app:latest \
    --dry-run=client -o yaml | more
```

saving the file into the file :- 
```bash 
kubectl create deployment hello-world \
    --image=phozzy/hello-app:latest \
    --dry-run=client -o yaml > deployment.yaml


```

applying the deployment
``` bash
kubectl apply -f deployment.yaml
```

creating the service declaratively
``` bash
kubectl expose deployenet hello-world \
--port=80 \
--targer-port=8080  --dry-run=client -o yaml > service.yaml

```

applying the service 
```bash
kubectl apply -f service.yaml
```

we can edit the resources  on the fly with kubectl edit , but this isn't reflected in our yaml.
but this change is persisted in the etcd cluster store . Change 20 to 15.
```bash
kubectl edit deployement hello-world
```

we can also scale a deployement using scale 
```bash
kubectl scale deployment hello-world --replicas = 40
```



# Dry Run
  we have two types of dry0run
1) Server-side
   Processed as a typical request
   Requestes will Not be persisted in storage
2) Client side 
	  writed the  object to be created to stdout 
	  validate manifest syntax.
	  Great for generating syntactically correct yaml manifests 

```
kubectl apply -f deployment.yaml --dry-run=server
kubectl apply -f deployment.yaml --dry-run=client

```

# Diff 
 Generates the deiffernece between 
  Resources running in the cluster 
  Resouces defined in a manifest or stdin 
  outputs the differences to stdout 
  Userful to help you understand whats going to change


``` 
 kubectl diff -f  newdeployment.yaml
 
```