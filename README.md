# Deploy_Kubeadm

### Steps to setup `kubeadm cluster`

**Pre-requisites**
- 3 Virtual Instances, one is acting as `Control Plane` & 2 are `worker nodes`.
- Minimum 2CPU & 2GB Memory.
- Swap memory should be disabled on the Virtual Machines.
- Network connectivity among all machines in the cluster.

## Preparing the hosts

### Ports and Protocols
When running Kubernetes in an environment with strict network boundaries, such as on-premises datacenter with physical network firewalls or Virtual Networks in Public Cloud, it is useful to be aware of the ports and protocols used by Kubernetes components.

![image](https://github.com/user-attachments/assets/ddd6b772-0858-4fd9-897f-9cadf196927d)

Protocol	Direction	Port Range	Purpose	Used By
TCP	Inbound	6443	Kubernetes API server	All
TCP	Inbound	2379-2380	etcd server client API	kube-apiserver, etcd
TCP	Inbound	10250	Kubelet API	Self, Control plane
TCP	Inbound	10259	kube-scheduler	Self
TCP	Inbound	10257	kube-controller-manager	Self
![image](https://github.com/user-attachments/assets/d395ab02-1a00-43c7-ba1f-fee79cc88b0e)


![image](https://github.com/user-attachments/assets/27d634fe-dafe-423f-a096-7a277ca60c34)


### Setup Steps

- [Installing Continer runtime [Docker]. ](https://github.com/RaghavaYerubandi/Deploy_Kubeadm/blob/main/README.md#installing-container-runtime-docker)
- [Install `KUBEADM`,`KUBELET`,`KUBECTL`.](https://github.com/RaghavaYerubandi/Deploy_Kubeadm/blob/main/README.md#installation-of-kubeadm-kubelet--kubectl)
- [Initialize Control Plane.](https://github.com/RaghavaYerubandi/Deploy_Kubeadm/blob/main/README.md#initialize-control-plane)
- [Install POD-Network Add-on [CNI].](https://github.com/RaghavaYerubandi/Deploy_Kubeadm/blob/main/README.md#installing-a-pod-network-add-on)
- [Join Worker Nodes.](https://github.com/RaghavaYerubandi/Deploy_Kubeadm/blob/main/README.md#adding-worker-nodes-to-the-cluster)
- [Deploying an Application [Nginx].](https://github.com/RaghavaYerubandi/Deploy_Kubeadm/blob/main/README.md#adding-worker-nodes-to-the-cluster)

### Installing Container runtime [Docker]
- Install Docker on Control Plane & Worker Nodes
~~~bash
apt update
~~~
~~~bash
apt install docker.io -y
~~~
Once docker installed, Verify with using
~~~bash
docker --version
~~~

**OutPut looks like**
~~~bash
root@CP-1:~# docker --version
Docker version 24.0.7, build 24.0.7-0ubuntu2~20.04.1
~~~

### Installation of `kubeadm`, `kubelet` & `kubectl`.
- `kubeadm`: the command to bootstrap the cluster.
- `kubelet`: the component that runs on all of the machines in your cluster and does things like starting pods and containers.
- `kubectl`: the command line util to talk to your cluster.

Update the system
~~~bash
sudo apt-get update
~~~
Install packages needed to use the Kubernetes apt repository
~~~bash
sudo apt-get install -y apt-transport-https ca-certificates curl gpg
~~~
Create a directory for keyrings, if not present.
~~~bash
sudo mkdir -p -m 755 /etc/apt/keyrings
~~~
Download the public signing key for the Kubernetes package repositories
~~~bash
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
~~~
Add the appropriate Kubernetes apt repository
~~~bash
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
~~~
Update the apt package index, install kubelet, kubeadm and kubectl
~~~bash
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
~~~

### Initialize Control Plane
Initializing your control-plane node [Can be perform on Control Plane only]
~~~bash
kubeadm init
~~~

**OutPut should look something like**
~~~bash
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 172.16.10.69:6443 --token er69y8.jliqf9p297ch7xpa \
        --discovery-token-ca-cert-hash sha256:1b80055e184f4b75e275145bfc7849c22b8cd1eab0d12946ce81f4147d07e8e7
root@CP-1:~#
~~~
- As the above output mentioned copy the token in your notepad, we will need to join worker/slave to the master node.

To make kubectl work for your `non-root` user, run these commands
~~~bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
~~~
If you are the `root` user, you can run
~~~bash
export KUBECONFIG=/etc/kubernetes/admin.conf
~~~

### Installing a Pod network add-on
- Run it on Control Plane only.
- You must deploy a Container Network Interface (CNI) based Pod network add-on so that your Pods can communicate with each other. 
- Cluster DNS (CoreDNS) will not start up before a network is installed.
Below are the list of some available add-ons for CNI.
- Antrea
- Calico
- Flannel
- Cilium
- Canal
- Knitter
- NSX-T
- Weave Net

### Installing CNI `Calico` add-on
Download the Calico
~~~bash
curl https://raw.githubusercontent.com/projectcalico/calico/v3.28.1/manifests/calico.yaml -O
~~~
~~~bash
kubectl apply -f calico.yaml
~~~

**OutPut should look something like:**
~~~bash
poddisruptionbudget.policy/calico-kube-controllers created
serviceaccount/calico-kube-controllers created
serviceaccount/calico-node created
serviceaccount/calico-cni-plugin created
configmap/calico-config created
customresourcedefinition.apiextensions.k8s.io/bgpconfigurations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/bgpfilters.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/bgppeers.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/blockaffinities.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/caliconodestatuses.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/clusterinformations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/felixconfigurations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/globalnetworkpolicies.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/globalnetworksets.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/hostendpoints.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipamblocks.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipamconfigs.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipamhandles.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ippools.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipreservations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/kubecontrollersconfigurations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/networkpolicies.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/networksets.crd.projectcalico.org created
clusterrole.rbac.authorization.k8s.io/calico-kube-controllers created
clusterrole.rbac.authorization.k8s.io/calico-node created
clusterrole.rbac.authorization.k8s.io/calico-cni-plugin created
clusterrolebinding.rbac.authorization.k8s.io/calico-kube-controllers created
clusterrolebinding.rbac.authorization.k8s.io/calico-node created
clusterrolebinding.rbac.authorization.k8s.io/calico-cni-plugin created
~~~
Verify with below Command
~~~bash
kubectl get pods -n kube-system
~~~

**OutPut should look something like:**
~~~bash
root@CP-1:~# kubectl get pods -n kube-system
NAME                                      READY   STATUS    RESTARTS   AGE
calico-kube-controllers-97d84d657-hhtzc   1/1     Running   0          116m
calico-node-4wfnt                         1/1     Running   0          107m
calico-node-dmzrj                         1/1     Running   0          116m
calico-node-j2cwb                         1/1     Running   0          104m
coredns-76f75df574-4mvqt                  1/1     Running   0          10h
coredns-76f75df574-nt8nh                  1/1     Running   0          10h
etcd-cp-1                                 1/1     Running   5          10h
kube-apiserver-cp-1                       1/1     Running   5          10h
kube-controller-manager-cp-1              1/1     Running   5          10h
kube-proxy-7nmm9                          1/1     Running   0          107m
kube-proxy-g57g9                          1/1     Running   0          104m
kube-proxy-pvqw9                          1/1     Running   0          10h
kube-scheduler-cp-1                       1/1     Running   7          10h
~~~
List the Nodes
~~~bash
kubectl get nodes
~~~

**OutPut should look something like:**
~~~txt
root@CP-1:~# kubectl get nodes
NAME   STATUS   ROLES           AGE   VERSION
cp-1   Ready    control-plane   8h    v1.29.8
~~~

**Note:-** As we add only one control plane, its showing only one and we didn't add any workernodes.

### Adding Worker Nodes to the Cluster
Execute the `kubead join` command on Worker Nodes to join with the cluster.
- Token will be generated while initiatizing `kubeadm` on control plane.
- Paste the Join command from the above kubeadm init output
~~~bash
kubeadm join 172.16.10.69:6443 --token er69y8.jliqf9p297ch7xpa \
        --discovery-token-ca-cert-hash sha256:1b80055e184f4b75e275145bfc7849c22b8cd1eab0d12946ce81f4147d07e8e7
~~~
Check the joined nodes 
~~~bash
kubectl get nodes
~~~
or 
~~~bash
kubectl get nodes -o wide
~~~

**OutPut should look something like:**
~~~bash
root@CP-1:~# kubectl get nodes
NAME   STATUS     ROLES           AGE     VERSION
cp-1   Ready      control-plane   8h      v1.29.8
wn-1   Ready      <none>          2m44s   v1.29.8
wn-2   Ready      <none>          41s     v1.29.8
~~~
As Observed the above Output, Roles for worker nodes are showing none, Lets change it.
~~~bash
kubectl label nodes <node_name> node-role.kubernetes.io/worker=true
~~~
~~~bash
kubectl label nodes wn-1 node-role.kubernetes.io/worker=true
kubectl label nodes wn-2 node-role.kubernetes.io/worker=true
~~~
Verify the Node Roles.
~~~
root@CP-1:~# kubectl get nodes
NAME   STATUS   ROLES           AGE    VERSION
cp-1   Ready    control-plane   10h    v1.29.8
wn-1   Ready    worker          103m   v1.29.8
wn-2   Ready    worker          101m   v1.29.8
~~~

### Deploying an Application
- Deploy an nginx sample web application on k8s.
~~~bash
kubectl create deployment app1 --image nginx:latest --replicas 3
~~~
- The above will create a deployment using nginx image with replicas of 3.
Verify the POD status
~~~bash
kubectl get pods
~~~

**OutPut should look something like:**
~~~bash
root@CP-1:~# kubectl get pods
NAME                    READY   STATUS    RESTARTS   AGE
app1-6cf7b4979b-kpzxg   1/1     Running   0          4m46s
app1-6cf7b4979b-ntpd2   1/1     Running   0          4m46s
app1-6cf7b4979b-plksm   1/1     Running   0          4m46s
~~~

# Thank You
