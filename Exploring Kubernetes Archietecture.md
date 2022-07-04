
 ## Exploring Kubernates Archietecture
 Cluster Components :- 
 1) ControlPlane Node.
	 Control function  of the cluster . 
	 implements custor operation, monitoring  hot scheduling.![[control_plane_components.jpg]]
 1) Node :- 
	 Application actually run here .
	 Implements networking  ensures reachablity.
	 either virtual or physical machine.


![[Node_Componenet.jpg]]



![[Pod_Operation.jpg]]

![[pod_failure.jpeg]]
![[service.jpeg]]
# Cluster Add on Pods
 **DNS  Server pods** : for domain name service for application inside kubernates.
 **Ingress** : Layer 7 load balancer.
 **Dashboard**