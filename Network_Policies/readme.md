# Network Policies
- Network Policies in K8s are used to control the communication between PODS, defining what kind of traffic is allowed to flow to specific PODs.
- By deafult all PODs in K8s can communicate with each other irrespective of namespace.
- This can be used for shared cluster.
- Using Network Policies, we can allow `Ingress` & `Egress` traffic based on port numbers.
- We can block the `Ingress` & `Egress` traffic as well.
