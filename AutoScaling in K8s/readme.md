# AutoScaling
- Autoscaling in kubernets will adjust the number of running pods based on the current load on the Pods.
- It will collects the POD metrics from the metric server.
- Metric server is the default metrics source for autoscaling.
- Below are the types of Autoscaling in kubernets.
  - HPA [Horizontal POD Autoscaler]
  - VPA [Vertical POD Autoscaler]
  - Cluster Autoscaler [using karpenter tool]

## HPA [Horizontal POD Autoscaler]
- HPA automatically scales the no of pods in a deployment, replicaset, statefulsets based on observed `CPU` & `Memory` usage or other custom metrics.
- It will take metrics from the metric server.

## VPA [Vertical POD Autoscaler]
- VPA automatically adjusts the resource requests & limits [CPU,Memory] for the containers in a POD based on the historical usage.
- It won't add or delete the pods, instead it will adjust the resources.
- It will take metrics from the metric server.

## Cluster Autoscaler [using karpenter tool]
- Cluster Autoscaler automatically adjusts the size of the k8s cluster by adding or removing nodes, based on the resource requirements of the workloads running in the cluster.

- In realtime scenario mostly HPA is used.

~~~bash
kubectl get hpa
~~~
- The above command will shows hpa, if configured.

**OutPut**
~~~bash
root@ragh-k8s-control-191b22796c9:~/HPA# ku get hpa
NAME         REFERENCE               TARGETS             MINPODS   MAXPODS   REPLICAS    AGE
deploy-hpa   Deployment/deploy-hpa   2%/50%, 3%/50%       1         10        1          52s
~~~
