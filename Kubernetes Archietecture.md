
## Kubernetes Networking Requirements
Every Pod will be assigned a unique Ip address.:-
1) Pods on a node can communicate with all pods on all nodes without the the network Address Translation in a cluster.
2) Agents on a Node can communicate with all Pods on that Node.


![[kubernates_network.jpeg]]


# kubenetes installation
 Consideration for  kubernetes :-
**Cluster Networking** 
**Scalabity** 
**High Availability**
**Disaster Recovery**


## Cluster Network Ports
<table>
<tr>
  <th>Component</th>
  <th>Ports</th>
  <th>Used By</th>
</tr>
<tr>
 <td>ApI Server</td>
 <td>6443</td>
 <td>All</td>
</tr>
<tr>
 <td>etcd</td>
 <td>2379-2380</td>
 <td>Api/etcd</td>
</tr>
<tr>
 <td>Scheduler</td>
 <td>10251</td>
 <td>Self</td>
</tr>
<tr>
 <td>Contoller Manager</td>
 <td>10252</td>
 <td>Self</td>
</tr>
<tr>
 <td>kubelet</td>
 <td>10250</td>
 <td>Control Pane</td>
</tr>
<tr>
 <td>NodePort</td>
 <td>30000-32767</td>
 <td>ALL</td>
</tr>
</table>



# Building your Cluster
1) Install and Configure Packages
2) Create your cluster (control plane)
3) Configure Pod Networking
4) Join Nodes to your cluster.

Required Packages for all regardless they are worker or control node:-
 1) containerd
 2) kubelet
 3) kubeadm
 4) kubectl


# Cluster Configuration
 Swap Disabled 
 Hostname set Host File on each.
control Plane Node :-  c1-cp1  172.16.94.10
 Node1  :- c1-node1   172.16.94.11
 Node2  :- c1-node2   172.16.94.12
 Node3  :- c1-node3   172.16.94.13

for every vm in our  cluster
``` bash
#disabling swap
swapoff -a
# comment out the value with stab
sudo vi /etc/fstab
```

installing containerd 

### Configure required modules
```bash
sudo modprobe overlay
sudo modprobe br_netfilter

cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF

```

### Configure required sysctl to persist across system reboots
```bash
cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF

```
### Apply sysctl parameters without reboot to current running enviroment
``` bash
sudo sysctl --system
```

### Install containerd packages
```bash
sudo apt-get update 
sudo apt -y install containerd
```
### Create a containerd configuration file
``` bash
sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml
```

### Set the cgroup driver for runc to systemd
At the end of this section in  /etc/containerd/config.toml
``` txt 
 [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
```

```bash
sudo sed -i 's/            SystemdCgroup = false/            SystemdCgroup = true/' /etc/containerd/config.toml
```

### Restart containerd with the new configuration
```bash
sudo systemctl restart containerd
```



``` bash


## add gpg key where the kubernates gpg key live

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add

## if you get error on curl

sudo apt-get install curl

## add kubernetes

sudo bash -c 'cat << EOF >/etc/apt/sources.list.d/kubernetes.list  
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
'

apt-get update

# find out the version
sudo apt-cache policy  kubelet | head -n 20

## add the version
VERSION=1.21


apt-get install -y kubelet=$VERSION kubeadm=$VERSION kubectl=$VERSION

## different from normal installation , we don't want apt to upgrade the kubernetes
## packages we do it ourself.
apt-mark hold kubelet kubeadm kubectl containerd


# check the service is running
sudo systemctl status kubelet.service
sudo systemctl status containerd.service


# Ensure both are set start when the system starts up 
sudo systemctl enable kubelet.service
sudo systemctl enable containerd.service


```

# BootStrapping a cluster 
``` bash
kubeadm init 
```
![[bootstrapping_cluster.jpg]]
These tasks are done serrially by:
## preflight checks  
- have required system requirement cpu and memory
- have required permission
- checks compatable container runtime

## Creates a Certificate Authority
## Generates Kubconfig files
## Generates Static Pod Manifests
kubelet looks at the location in static pod manifests
## Wait for the Control Plane Pods to Start
## Taints the Control Plane Node
## Generate  a bootstrap token
 used for joining additional node 
## starts Add-On components : DNS and kube proxy.

# Certificate Authority
 - self signed Certificate Authority
 - can be part of external Pki
 - securing cluster communications
 - Used By Api Server  to secure http 
 - Authentication of users and cluster components
  /etc/kubernetes/pki
  Distributed to each Node.

## kubeadm Created kubeConfig Files
Used to define how to connect to your Cluster
 inside a config we are going to find:-
  - Client Certificates
  - Cluster api server network location
  kubeadm creates a collection of kubeconfig files:-
  /etc/kubernates
  admin.conf (kubernetes-admin)
  kubelet.conf
  controller-manager.conf
  scheduler.conf
  
  ### static pod manifest
   Manifest describes a configuration
   /etcd/kubernetes/manifests
   etcd 
   API Server
   Controller Manager
   Scheduler
   watched by the kubelet started automatically when the system starts and over time.


# Pod Networking
Single un Nated Ip adderss Per Pod 
we could use Direct routing :-
   Configure infrastructure to supprort Ip reachability Pods and Nodes
Overlay networking
  - Flannel - Layer 3 virtual network
  - Calico - Layer 3 and policy based traffic management
  - Weave Net - multi-host network

(Network  range shouldn't overlap )

## Creating a Control Plane Node
```bash
# download  describes the pod network we want to deploy
wget https://docs.projectcalico.org/manifests/calico.yaml
# allocated from the value here network team sets this.
# CALICO _IPV4POOL_CIDR
```
append the code below the data in  calico

```yaml 
    - key: "node-role.kubernetes.io/control-plane"
      effect: NoSchedule


```

``` bash

# create cluster configuration file
kubeadm config print init-defaults | tee ClusterConfiguration.yaml

sed -i 's/ advertiseAddress: 1.2.3.4/ advertiseAddress: 192.168.122.103/' ClusterConfiguration.yaml


cat <<EOF | cat >> ClusterConfiguration.yml
---
apiVersion:kubelet.config.k8s.io/v1beta1
kind: KubeletConfiguration
cgroupDriver: systemd
EOF
```

``` bash

# intialize cluster and pass in the configuration
sudo kubeadm init --config=ClusterConfiguration.yaml 
  # --cri-socket /run/containerd/containerd.sock

# create a directory for config
mkdir -p $HOME/.kube

# copy the config to home directory
sudo cp -i /etc/kubernetes/admin.conf  $HOME/.kube/config

# change the permission for current user
sudo chown $(id -u):$(id -g) $HOME/.kube/config

# create the pod network
kubectl apply -f calico.yaml

#Look for all the SYstem pods and Calico pods to change to Running
# The DNS pod won't start until pod netword is deployed and running
kubectl get pods --all-namespaces

## watch the pods overtime
kubectl get pods --all-namespaces --watch

## checkotking kubelet is running
sudo systemctl status kubelet.service

# looking at the static pod mainifests
ls /etc/kubernetes/manifests
sudo more /etc/kubernetes/manifests/etcd.yaml
sudo more /etc/kubernetes/manifests/admin-server.yaml

# where the directory for the kube config files in the control plane pods.
ls /etc/kubernetes
```





## Joining the Cluster

![[Node_addition_to_cluster.jpg]]

```bash

# on the control plane
kubeadm token list

# Generate a new kubeadm token
kubeadm token create

# getting the new ca cert
openSSL x509 -pubkey -in /etc/kubernetes/pki/ca.cert | openssl rsa -pubin -outform der 2>/dev/null openssl dgst -sha256 -hex | sed 's/^.*//*'


# to print the join commanad
kubeadm token create --print-join-command


kubeadm join 192.168.122.103:6443 --token 4re9di.xkzyftpulxovsur9 -discovery-token-ca-cert-hash sha256:2cf432645ca4c949e047bb018f121401ac40040e52c880f46adbde2a391e4937


# ip address of the control plane
kubeadm join 172.16.94.10:6443 \
# bootstrap token 
--token iopr88\
# ca cert hasj
--discovery-token-ca-cert-hash  \
sha256:9156f12323dfdf


# check the node is running
kubectl get nodes

# check wheter the pods are running
kubectl get pods --all-namespaces --watch


```

