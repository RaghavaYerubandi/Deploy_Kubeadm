# Metrics
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
