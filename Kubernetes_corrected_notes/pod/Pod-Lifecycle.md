
# Pod LifeCycle

![[PodLifeCycle.jpg]]

![[pod-termination.jpg]]

## pod deletion  with grace period
``` bash
 kubectl delete pod podname --grace-period=<seconds>
```

## force deletion of the pods
``` bash
kubectl delete pod podname --grace-period= 0 --force
```

![[podterminationGracePeriod.jpg]]