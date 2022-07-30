# When is deployment updated
1) Rolling out a new container image
2) when the changing the pod template.
And things related to deployment spec like replicas doesn't trigger a rollout.

## updating the deployment image
``` bash
kubectl  set image deployemnt hello-world hello-world=hello-app:2.0
```

## recording the changes to the deployment
``` bash
kubectl  set image deployemnt hello-world hello-world=hello-app:2.0 --record
```

## edit the deployment
```bash
kubectl edit deployment hello-world
```

## declarative change
``` bash
kubectl apply -f deployment.yaml
```

# How do we know status for our deployment
``` bash
kubectl rollout status deployment deployment-name
```

we can know the history of a rollout with
```
kubectl rollout deployment deployment-name history 
```

we can also know the details with **describe** command

# How does kubernetes control the rollout in cluster.
 Kubernetes has update strategy . There are two type of update strategy .
 1) RollingUpdate
 2) Recreate 
So with we can control the rate of deployment of pods with: 
1) maxSurge
2) maxUnavailable

```yaml
apiVersion: apps/v1
kind: Deployment
spec:
  replicas: 20
  strategy:
    type:RollingUpdate
    rollingUpdate:
      maxUnavailable: 20
      maxSurge: 5
template: 
  spec:
   containers:
     readinessProbe:
       httpGet:
        path: /index/html
        port: 8080
       intialDelaySeconds: 10

```

# how do pause a  deployment
how do we pause rollout  
``` bash
kubectl rollout pause deployment my-deployment
```
how do we resume rollout
```
kubectl rollout resume deployment my-deployment
```

# Rollout History
We have history of rollout saved in kubernetes which is can be accesed with rollout history command . The rollout has revisionhistoryLimit which defaults to 10 . it is the number of enteries save in rollout history.

This is to see the pod-template information for revision 1 
``` bash
kubectl rollout history deployment my-deploymenet --revision=1 
```

for undoing the deployment
```sh
kubectl rollout undo deployment my-deployment
```

for redoing the deployment to certain revision
``` sh
kubectl rollout undo deployment my-deployment --to-revision=5
```

For restarting the deployment 
```sh
kubectl rollout restart deployment my-deployment
```

