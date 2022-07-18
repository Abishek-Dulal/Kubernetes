
# Pod Defination
Pod is wrapper around the container  system in kubernatis.

Pod can be looked as :-

1) Unit of scheduling:-
	 It does the work that scheduled to run and It is proces on the cluster
	 that actually consume resources.

2) Unit of Deployment
	  Pod manage the application configuration . As a  unit which we interact . It defines the networking and resources that we use.

## why pods instead of container
 Because it is easier to manage and the information about the network are uniform for us.


## Conatainer within pods
1) [[single-container-pod]]
2) [[mulit-container-pod]]
3) [[init-container-pod]]

if you want know about pod lifecycle look at :-
[[Pod-Lifecycle]]

# Persistency in Pods:-
 1) Configuration is managed externally to the pods .
 2) Pod Maniifests , secrts and config-maps  hold configuration data for the pods.
 3) Data is managed externally : - 
	 1) persistant volume
	 2) persitant volume claims

# How kubernetes maintain resilency
[[Container Resilency Policy]]

# How to define the health of pod in kubernetes.
Kubernetes need information  to know when a pod has **started** when it is **ready** to recieve request and if kubernetes the container are l**ive** . For that we use container probes  
2) [[Liveness Probes ]]
3) [[readiness Probe]]
4) [[startupProbes]]

Type of  probes that can be done by kubernetes :-
1)  exec  with process exit code (non-zero and non-negative)
2) tcpSocker (Sucessfully ope a Port)
3) httpGet (Return code 200=> and <400)
