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

# Api Groups
Used for organisation of resource 
Api  Groups
   core Api (Legacy Group)
   Named Api Groups
part of the Api Objects Url in Api Requests. for the named group
<table>
  <thead>
    <tr>
    <th>Core (Legacy)</th>
    <th>Named Api Groups</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Pod</td>
      <td>apps - Deployment </td>
    </tr>
    <tr>
      <td>Node</td>
      <td>storage.k8s.io -StorageClass </td>
    </tr>
    <tr>
      <td>NameSpace</td>
      <td>rbac.authorisation.k8s.io - Role </td>
    </tr>
    <tr>
      <td>PersistentVolume</td>
      <td>  </td>
    </tr>
    <tr>
      <td>PersistentVolumeClaims</td>
      <td>  </td>
    </tr>
  </tbody>
</table>

# Api Versioning
Api is versiones
Provide stability for existing implementations
Enable forward Change
Alpha -> Beta -> Stable
No direct relation to release version for kubernetes.


![[api-versioning.jpg]]

# Demo for api - groups

Api Resource  in a specific api - groups
``` bash
kubectl api-resources --api-group=apps
```
we can use explain to dig further into a specific API Resource and Version
``` bash
kubectl explain deployment --api-version apps/v1 | more
```
print the supported Api versions and groups on the Api server  again in the group/version
``` bash
kubectl api-versions | sort | more
```


# APi Reource Location (Api Paths )
![[Screenshot_2022-07-01-12-36-01-41.jpg]]
![[Screenshot_2022-07-01-12-40-33-45.jpg]]


![[Screenshot_2022-07-01-12-44-06-94.jpg]]


