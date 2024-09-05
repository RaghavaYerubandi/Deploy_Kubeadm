# Deploy_Kubeadm
### Steps to setup kubeadm cluster in an `onprem-setup`

**Pre-requisites**
- We took a 3 node cluster, one is ControlPlane and 2 Worker Nodes to setup the `kubeadm_cluster`.
- Min 2 Cpu & 2GB Memory.
- Network connectivity among all machines in the cluster

### Preparing the hosts
- WIP REQ ports for kubeadm
- Install `Container` runtime

### Installing Container runtime
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
OutPut looks like
~~~bash
root@CP-1:~# docker --version
Docker version 24.0.7, build 24.0.7-0ubuntu2~20.04.1
~~~
### Installation of kubeadm, kubelet & kubectl
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
Create a directory for keyrings
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
### Creating a cluster with kubeadm
Initializing your control-plane node #can be perform on Cntrol Plane only.
~~~bash
kubeadm init
~~~
**OutPut looks like below**
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
OutPut Looks Like
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
**OutPut**
~~~bash
NAME                                      READY   STATUS     RESTARTS   AGE
calico-kube-controllers-97d84d657-hhtzc   0/1     Pending    0          2m24s
calico-node-dmzrj                         0/1     Init:0/3   0          2m24s
coredns-76f75df574-4mvqt                  0/1     Pending    0          8h
coredns-76f75df574-nt8nh                  0/1     Pending    0          8h
etcd-cp-1                                 1/1     Running    5          8h
kube-apiserver-cp-1                       1/1     Running    5          8h
kube-controller-manager-cp-1              1/1     Running    5          8h
kube-proxy-pvqw9                          1/1     Running    0          8h
kube-scheduler-cp-1                       1/1     Running    7          8h
~~~
List the Nodes
~~~bash
kubectl get nodes
~~~
**OutPut**
~~~txt
root@CP-1:~# kubectl get nodes
NAME   STATUS   ROLES           AGE   VERSION
cp-1   Ready    control-plane   8h    v1.29.8
~~~
**Note:-** As we add only one control plane, its showing one and we didn't add any workernodes.
### Adding Worker Nodes to the Cluster
Execute the `kubead join` command on Worker Nodes to join with the cluster.
- Token will be generated while initiatizing `kubeadm` on control plane.
~~~bash
kubeadm join 172.16.10.69:6443 --token er69y8.jliqf9p297ch7xpa \
        --discovery-token-ca-cert-hash sha256:1b80055e184f4b75e275145bfc7849c22b8cd1eab0d12946ce81f4147d07e8e7
~~~
Verify the `Nodes` on `Control Plane`
~~~bash
root@CP-1:~# kubectl get nodes
NAME   STATUS     ROLES           AGE     VERSION
cp-1   Ready      control-plane   8h      v1.29.8
wn-1   Ready      Worker1         2m44s   v1.29.8
wn-2   Ready      Worker2         41s     v1.29.8
~~~
