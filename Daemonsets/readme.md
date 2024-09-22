# Daemonsets
- A DaemonSet ensures that all Nodes run a copy of a Pod.
- Mostly used for `logging-agents`, `Monitoring-agents`, etc.
- If the node is drainout, then the POD shouldn't start on other node.
- If the node come back's online, the POD again should start on the same node

**Command to get deamonsests**
~~~bash
kubectl get ds -A
~~~
