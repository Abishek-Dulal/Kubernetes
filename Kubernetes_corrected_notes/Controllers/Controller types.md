# Controller Manager
Controller manager are to keep the kubernetes cluster in desired state by control loops. 
we have two types of kube controller manager:-
1) kube-controller-manager
2) cloud controller manager 






# What are the controller types available in kubernetes
Kubernetes has many controllers available in kubernetes that are listed as below:-
## Pod Controllers
1) Daemonset
2) Jobs
3) CronJobs
4) StatefulSets
5) Deployment
## Other Controller
1) Node
2) Service



# DaemonSet :- 
 DaemonSet are a type of controller that ensure that a pod runs on all node or some of the nodes in a cluster meaning it is effectively used for background services.
## Example workload:-
1) Log collectors
2) metric servers
3) Resource monitoting agents
4) Storage daemons
``` yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: hello-world-ds
spec:
  selector:
    matchLabels:
      app: hello-world-app
   template:
     metadata:
        labels:
           app: hello-world-app
     spec:
       nodeSelector:
         node: hello-world-ns
       containers:
        - name: nginx
          image: nginx
```

## How do you Handle daemonset update:-
1) Rolling Update
2)  onDelete 

# Jobs
Jobs are a type of controller that create one or more pods which is used to complete the program in a contrainer to completion . It ensures that the specified number of Pods complete sucessfully. 
## What is the types of task the job perform
1) adhoc
2) batch
3) data oriented task.

# what ensured the jobs run to completion
we have restart policy that ensure  the pod to restart  on failure.

# what is the lifecycle for a job
Lifecycle of the job proceds as follows:-
1) Job are started
2) Job completes sucessfully 
3) the job status is set to completed
4) the job object remains
5) the pod for the job are not deleted (for logs , info etc)

``` yaml
apiVersion: batch/v1
kind: job
metadata:
  name: some-name
spec:
  template:
    spec:
     containers:
     - name: ubuntu
	   image: ubutu
	   command:
	     - "bin/bash"
	     - '-c'
	     - "/bin/echo Hello from Pod "
	 restartPolicy: onFailure

```

# CronJob
CronJob are a controller that run a job at a given time based schedule .

## What is corn Job used for
1) Periodic workloads and scheduled tasks

``` yaml 
apiVersion: batch/v1
kind: CronJob
metadata:
 name: hello-world-cron
spec:
  schedule: ""
  jobTemplate:
    spec:
      template:
	    spec:
	     containers:
       
```