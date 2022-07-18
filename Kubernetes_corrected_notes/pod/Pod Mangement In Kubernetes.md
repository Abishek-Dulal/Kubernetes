# How are Pod managed in kubernetes
Pods are managed on kubernates by the following way :-
1) **Controllers** :  Controllers keep the pod in the desired state . So it is job of the controller to start and stop the pods. Because of this the controllers are responisble for application scaling and recovery.
2) **Bare Pods** :  these are the pod that are created by the user in the cluster that are not associated with the controller. These pod sole responsibitiy of the user deploying the pod, so no restarts on failure.
4) **Static Pods** : These are the pods managed by the kubelet specificied in the kubelet staticpodpath  configuration .

# Static Pods : - 
  These are the pods managed by the kubelet specificied in the kubelet staticpodpath  configuration.  The path of the configuration is generally in
  **/etc/kubernetes/manifests**
 This is how the control plane pods started  by kubeadm bootstraping process.
 we can change path for the static pod mainfests with :-
 **/var/lib/kubelet/config.yaml** 
 The specified path is watched by the kubelet that means the the change is manifests are updated on the cluster.

<div style=" color: blue ; font-weight: bold">
The api created by the static pods are created by the kubelet and mirror pods are created in the apiserver so that we know which pods are created but we cannot change the pods using kubectl commands.
</div>

<div style = "color:green; font-weight:bold">
    add pod manifest in staticpodPath to create staticpods
</div>


