
# Kubernetes api :-
Single surtace area over the resources in your data center
Api Objects :
 Collection of primiives to represrnt your system state 
 Enables configuration of state 
 Api Server 
  the sole way to interact with your cluster.
  the sole way kubernetes interacts with the cluste
  Client/ Server archietecture
  Restful Api over Http using Json
  client submits requests over Http /Https
  Stateless
   configuratrion are SeriaFlezed and the persisted in the cluster store


# Demo 
Get inFormation about the current  cluster context . 
```
 kubectl config get-context
```
chane context 
```
kubectl config use-context kubernetes-admin@kubernetes
```
Get intformation about the Api Server for  our current context which should be the kubernetes -admin
``` bash
kubectl cluster-info 
```

Getting Api Resources 
```  bash
kubectl get api-resources | more
```

# APi Reource Location (Api Paths )
![[response_code_from_api.jpg]]


![[anatomy_of_kubernetes_api_request.jpg]]

