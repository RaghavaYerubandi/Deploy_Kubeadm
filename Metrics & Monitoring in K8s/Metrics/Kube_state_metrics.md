# Kube State Metrics
- Kube state metrics is a service that listens to the kubernetes api server and generates metrics about the state of the objects, such as `deployments` `nodes` `pods` etc within a kubernrtes cluster.
- It provides high-level metrics related to the state of k8s objects.
- Metrics include the no.of replicas in a deployments, the state of pods[running, failed, pending] & status of the nodes [Ready, not ready].
- kube state metrics is commonly used with prometheus.
- The metrics provide by kube state metrics can be scraped by `prometheus` & used to setup alerts & monitor cluster health. These metrics can then be used in Grafana dashboards or for alerting with Prometheus Alertmanager.
- Kube state metrics runs as a service inside the k8s cluster & it queries the kubernetes api server to retrive info about the state of various objects.

## Installing Kube state metrics.

~~~bash
kubectl apply -f https://github.com/kubernetes/kube-state-metrics/releases/latest/download/kube-state-metrics.yaml
~~~
 Or by Helm.

~~~bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
~~~
**verify**
~~~bash
root@ragh-k8s-control-191b22796c9:~# helm repo list
NAME                            URL
harbor                          https://helm.goharbor.io
nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/
portainer                       https://portainer.github.io/k8s/
prometheus-community            https://prometheus-community.github.io/helm-charts
root@ragh-k8s-control-191b22796c9:~#
~~~

repo update
~~~bash
helm repo update
~~~
Created a ns `mon`
~~~bash
ku create ns mon
~~~
instal prometheus uisng helm

~~~bash
helm install prometheus prometheus-community/kube-prometheus-stack -n mon
~~~

**Verify Pods**
~~~bash
root@ragh-k8s-control-191b22796c9:~# ku get pods
NAME                                                     READY   STATUS    RESTARTS   AGE
alertmanager-prometheus-kube-prometheus-alertmanager-0   2/2     Running   0          5m4s
prometheus-grafana-c9bc9b494-j6hfn                       3/3     Running   0          6m2s
prometheus-kube-prometheus-operator-6b4895c77d-htbnm     1/1     Running   0          6m2s
prometheus-kube-state-metrics-7bd877f74d-hnm5s           1/1     Running   0          6m2s
prometheus-prometheus-kube-prometheus-prometheus-0       2/2     Running   0          5m4s
prometheus-prometheus-node-exporter-ctnzl                1/1     Running   0          6m2s
prometheus-prometheus-node-exporter-vxc5m                1/1     Running   0          6m2s
prometheus-prometheus-node-exporter-wpjcl                1/1     Running   0          6m2s
~~~

