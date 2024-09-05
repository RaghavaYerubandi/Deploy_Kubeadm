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
- https://kubernetes.io/docs/concepts/cluster-administration/networking/#how-to-implement-the-kubernetes-network-model
