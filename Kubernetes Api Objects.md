
 ## Kubernates Api
 ***Api Objects**  - Represents the primitives that represent our system state
 we can have two ways to define api objects :-
- declaratively
- Imperatively

Kubernates communicates using rest  api over Http using Json . The Json is serialised and persisted .

kubernetes Objects are Organized by :-
Kind - Pod , Service ,Deployment
Group - core , apps , storage
version - v1, beta, alpha


**Primitives for  Kubernates** :-
Pods -  Pods are collection of container that is deployed as a single unit .
Controllers - This is way to control the desired state of the application. 
Services - Persisent access point to the application provides by the pods
Storage-  Storage that is persistant across our application.
Deployment :- declarativity declare  deploy for our application   

**Pods** 
   One or more Containers.
   It is the application which is deployed.
   The most basic unit of  work.
   This is the unit of scheduling.
   Ephemeral - no pod is ever redeployed . (Doesn't hold state)
   Atomicity - they're there or not . (It is either running or not)
   Kubernets job is keeping your pods running.  it tracks state such as:-
	   1) State :- is the pod up and running.
	   2) Health: is the application inside throwing errors .(health probe.)
	   
**Controllers** :
  Defines your desired state 
  Create and manage for you.
  Respond to pod state and Health.
    1) ReplicaSets 
	    Number of replicas   for a collection of pods are running all time .
	    ex 3 pods replicaset . THese three replica is always available.
	2) Deployment 
	    ReplicaSets are not directly created we create Deployment. 
	    Deployement controller manages the state of the replica set.
	    It manage the rollout of replicasets.  for ex change the application version from 1 to 2.
	3) Services :- 
		Adds Peristency to out ephemeral pods .
		Networking abstraction for pod acess. 
		Ip and DNS name for the Appliecation  Service on the pods.
		Dynamically updates based on Pod lifecycle.
		Scaled by adding/removind pods/
		load balancing.
	4) Storages:- 
		Volumes : storage backed by physical media directly access by the pod 	
		Persistent Volume: - Pod inpendent storage defined by the service administrator at the cluster level.  Persistent Volume Claim  (I want 10 gb of this type of storage from the underlying cluster.)
