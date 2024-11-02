# Metrics Server
- It provides realtime resource metrics like CPU & Memory usage for Nodes & Pods.
- Metrics server is the default metrics source for the Horizontal Pod Autoscaler [HPA] and Vertical Pod Autoscaler [VPA].
- It Provides the metrics necessary to scale applications up o down based on current resource usage.
- Metric server integrates with the kubernetes API, and making metrics accessible via the kubernetes 'kubectl top' command.

## Working of Metric Server.
- It collects metrics from the `kubelet` on each node in the cluster.
- The kubelet itself gathers metrics from `CAdvisor`, which is integrated into `kubelet` and collects resources usage data from containers.
- The collected metrics are intigrated and made available via the kubernets metrics API, which can be accessed by autoscalers & other components.
- Below are the commands to get the metrics for Nodes & Pods.
**For Nodes**
~~~bash
kubectl top nodes
~~~
**For Pods**
~~~bash
kubectl top pods
~~~

## Deploy Metric Server

~~~bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
~~~

**Verify the deployment**
~~~bash
kubectl get deployment metrics-server -n kube-system
~~~

## Verify the metrics
**For Pods**
~~~bash
root@ragh-k8s-control-191b22796c9:~# ku top pods
NAME                     CPU(cores)   MEMORY(bytes)
docs-5b7d8d6d97-bqclz    30m          46Mi
docs-5b7d8d6d97-hcqxv    25m          47Mi
docs-5b7d8d6d97-sjqq9    11m          48Mi
myapp-58644d7749-gr2hb   0m           4Mi
myapp-58644d7749-qsqd8   0m           9Mi
myapp-58644d7749-wqmvf   0m           9Mi
~~~
**For Nodes**
~~~bash
root@ragh-k8s-control-191b22796c9:~# ku top nodes
NAME                           CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%
ragh-k8s-control-191b22796c9   91m          2%     1380Mi          17%
ragh-k8s-node-191b227cb15      70m          1%     1309Mi          16%
ragh-k8s-node-191b227fedd      73m          1%     1476Mi          18%
~~~
**Optional**
- Add the following flags to bypass TLS verification.

~~~bash
kubectl edit deployment metrics-server -n kube-system
~~~

~~~yaml
spec:
  containers:
  - name: metrics-server
    args:
    - --kubelet-insecure-tls
    - --kubelet-preferred-address-types=InternalIP
~~~
