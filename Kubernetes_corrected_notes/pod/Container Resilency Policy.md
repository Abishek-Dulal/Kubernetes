# Container Restart Policy

A container in a pod can reatart independent of  the Pod .
Container retart policy applies to containers inside a pod spec and is defined inside the Pod's spec. The pod is the environment the container runs in so  kubelet is restart the contaner on the same pod in the same node.

So on consequetive failure we have a expontential backoff for 10s , 20s , 40s capped at 5m and reset to 0 after 10 m of sucessfull runtime.  

## Policy types:-
1) Always : will restart all containers inside a pod (default)
2) onFailure - Non - graceful execution.
3) Never - never restart


![[restart-policy.jpg]]