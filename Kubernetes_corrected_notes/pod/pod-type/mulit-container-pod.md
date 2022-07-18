
# Multi Container Pods
MultiContainer pod are used for tightly coupled applications where the process must be scheduled with respect to each other.  This may be the case when one is producing data and other is consuming data .  
<div style="color:red">Don't use this to influence scheduling of pods,</div>

``` yaml
apiVersion: v1
kind : Pod
metadata:
  name: multicontainer-pod
spec:
  containers:
    - name: producer
	  image: ubuntu
	  command: ["/bin/bash"]
	  args: []
	  volumenMounts:
	  - name: webContent
		mountPaths: /var/log
	- name: consumer
	  image : nginx
	  ports: 
	   - containerPorts : 80
	  volumenMounts:
	    -name: webContent
	    mountPath: /var/share/nginx/html
   volumens:
    - name: webContent
    - emptyDirs: {} 
```
Defination for  Multi Container Pod

### Things to know about multicontainer node 
1) container in pod share the same operating system level namespace . So container communicate with eachother with localhost.
2) Each Container image has its own file ssystem .
3) Volumes in multicontainer pod are defined in the pod level  but can be mounted on the container filesystem .