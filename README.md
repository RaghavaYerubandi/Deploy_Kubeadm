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
