# Advanced Scheduling And POD Placements
- Advanced Scheduling and POD Placements in K8s involve techniques and configurations that ensure PODs are scheduled on the most appropriate nodes in the cluster.
- Using the techiques we can optimize resources utilization, echance performance and ensure high availability.
- By default PODs will schedule on the workernodes. We can change this scheduling using the below advanced scheduling techniques.

**Key Concepts in Advance scheduling**
- Node Selector
- Affinity and Anti-affinity
- Taints and Tolerations
- POD Topology spread constraints
- Custom Schedulers
- Resource requests and limits
- Daemonsets

## Node Selector
- Node selsctors are the simplest form of scheduling constraints.
- It allows us to specify that aPOD should be scheduled only on nodes that match specific labels.
- These can be acheived by using labels.

**Get the Nodes**
~~~bash
kubectl get nodes
~~~
**OutPut looks similar to this**
~~~bash
root@ragh-k8s-control-191b22796c9:~# kubectl get nodes
NAME                           STATUS   ROLES           AGE   VERSION
ragh-k8s-control-191b22796c9   Ready    control-plane   24d   v1.28.4
ragh-k8s-node-191b227cb15      Ready    worker          24d   v1.28.4
ragh-k8s-node-191b227fedd      Ready    worker          24d   v1.28.4
~~~

**Get the Detailed info about Node**
~~~bash
Kubectl describe node ragh-k8s-node-191b227fedd
~~~

- In the Labels section we can see the labels which all are assigned to the node `ragh-k8s-node-191b227fedd`
~~~bash
root@ragh-k8s-control-191b22796c9:~# ku describe node ragh-k8s-node-191b227cb15
Name:               ragh-k8s-node-191b227cb15
Roles:              worker
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=ragh-k8s-node-191b227cb15
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/worker=true
Annotations:        kubeadm.alpha.kubernetes.io/cri-socket: unix:///var/run/containerd/containerd.sock
                    node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Mon, 02 Sep 2024 09:55:53 +0000
Taints:             <none>
Unschedulable:      false
~~~
