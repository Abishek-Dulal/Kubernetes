
# Init Containers
It is variation on the multicontainer pod . Init Containers runs befors the main Application container .The main Container can only run after all the init container are run sucessfully.
when the init container fils the container restart policy applies. 
Init Containers are mainly used for :-
1)  Run tools or utilities
2) seperation of duties
3) block pod atartup.

``` yaml

apiVersion: V1
kind: Pod
spec:
  initContainers:
   - name: init-service
	 image: ubuntu
   - name: init-database:
	 image: ubuntu
  containers:
    - name: app-container
      image: nginx

```