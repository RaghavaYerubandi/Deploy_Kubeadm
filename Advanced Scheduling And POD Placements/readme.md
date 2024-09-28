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
- We can select the label and mention in the deployment file, so that the POD will schedule on the same node as per `labels`.
- We can also create custom labels and add to the nodes as well.
~~~bash
kubectl label node <node_name> <label_key>=<label_value>
~~~
~~~bash
kubectl label node ragh-k8s-node-191b227fedd high-memory=yes
~~~
- Assumng that node `ragh-k8s-node-191b227fedd` has high memory performance.

**Verify the labels**
~~~bash
kubectl get nodes ragh-k8s-node-191b227fedd --show-labels
~~~
**OutPut looks similar to this**

~~~bash
root@ragh-k8s-control-191b22796c9:~# ku get nodes ragh-k8s-node-191b227fedd --show-labels
NAME                        STATUS   ROLES    AGE   VERSION   LABELS
ragh-k8s-node-191b227fedd   Ready    worker   24d   v1.28.4   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,high-memory=yes,kubernetes.io/arch=amd64,kubernetes.io/hostname=ragh-k8s-node-191b227fedd,kubernetes.io/os=linux,node-role.kubernetes.io/worker=true
~~~

**Removing labels for node**
~~~bash
kubectl label node <node_name> <label_key> -
~~~
**OutPut looks similar to this**
~~~bash
root@ragh-k8s-control-191b22796c9:~# ku label node ragh-k8s-node-191b227fedd high-memory-
node/ragh-k8s-node-191b227fedd unlabeled
~~~
- Using Nodeselctor we can't deploy using multiple labels or node selector.

## Affinity and Anti-Affinity
- Affinity & Anti-affinity rules allow more comples rules compared to NodeSelector.
- We can define more labels in this type.
- There are 2 types.
   - Node Affinity and Anti-Affinity
   - Pod Affinity and Anti-Affinity

### Node Affinity
- It determines on which nodes a POD should be placed.
- We can use multiple labels.
- Based on below selectors, Node affinity will work.
    - requiredDuringSchedulingIgnoredDuringExecution
    - preferredDuringSchedulingIgnoredDuringExecution
- While deploying with `requiredDuringSchedulingIgnoredDuringExecution` the Pods will deploy on the nodes based on `selectors` & `labels`.
- Here if more PODs are running on the same nodes based on `selectors` & `labels`. The newly created pods will be in `Pending state` as there is no resources on the nodes.
  
- While deploying with `preferredDuringSchedulingIgnoredDuringExecution` Pods will deploy on the nodes based on `weights` & `labels`.
- Here if more PODs are running on the same nodes based on `weights` & `labels`. The newly created pods will be scheduled on the other node.

**Creating a Labels**
~~~bash
kubectl label node ragh-k8s-node-191b227fedd env=dev application=webapp
~~~

### Node Anti-Affinity
- Node Anti-Affinity in Kubernetes is a mechanism that allows you to influence the scheduling of Pods by ensuring that they do not run on specific nodes or are distributed across different nodes.

### POD Affinity
- POD affinity and Anti-affinity are advanced sceduling features that allows us to control how pods are placed relative to other pods.
- POD affinity allows us to define rules for placing a POD on the same node.
- This is useful when you want to group related PODS together to improve performance.

![image](https://github.com/user-attachments/assets/100e3e33-78a6-4e97-9c36-ceefea3f6b33)

- As per above diagram , when the POD affinity enables, both `APP-POD` & `DB-POD` will place on the same node.
- So that it can communicate easily.
- It improves performance, reduce latency, etc.

### POD Anti-Affinity
- Pod anti-affinity ensures that a POD is not scheduled on the same node.
- This can help in improving `fault-tolerance` by spreading out replicas of a service across different nodes.
