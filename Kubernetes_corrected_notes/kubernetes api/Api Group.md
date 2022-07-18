Api Groups helps better  organistation of reosources in kubernetes.
there are two types of api groups in kubernetes.


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

 A watch on pods will Watch on the resource version api/v1/namespace/default/Pods
``` bash
kubectl get pods --watch -v 6 &


```

We can use -v to increase the verbosity of the request . Display requested resource url focus on the verb , api path and response code.
kubectl get pod hello-world -v 6 

Same output as -v6 with request headers and truncated response body with  -v 8 and like wise.

Start up an kubectl proxy  session , this will authenticate  use to the api server .
``` bash
kubectl proxy &
curl http://localhost:8001/api/v1/namespace/default/pods/hello-world | head -n 20
```


![[core_vs_other_api.jpg]]