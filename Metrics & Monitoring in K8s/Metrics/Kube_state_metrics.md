# Kube State Metrics
- Kube state metrics is a service that listens to the kubernetes api server and generates metrics about the state of the objects, such as `deployments` `nodes` `pods` etc within a kubernrtes cluster.
- It provides high-level metrics related to the state of k8s objects.
- Metrics include the no.of replicas in a deployments, the state of pods[running, failed, pending] & status of the nodes [Ready, not ready].
- kube state metrics is commonly used with prometheus.
- The metrics provide by kube state metrics can be scraped by `prometheus` & used to setup alerts & monitor cluster health. These metrics can then be used in Grafana dashboards or for alerting with Prometheus Alertmanager.
- Kube state metrics runs as a service inside the k8s cluster & it queries the kubernetes api server to retrive info about the state of various objects.
