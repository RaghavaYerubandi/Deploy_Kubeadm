# Ingress Controller and Ingress Resources
- In K8s ingress controller and ingress resources work together to provide http & https routing to services within a cluster.
- They manage external access to the services, typically through http/https and also handle features like loadbalancing and SSL termination and Name based virtual hosting.

**Advantages**
- Routing based on domain name to different services.
- SSL termination.
- Can use single LoadBalancer instead of multiple.
## Ingress Controller
- An Ingress Controller is a deamon, typically running as a kubernetes pod, that watches changes to ingress resources and configures the underlying loadbalancer accordingly.

**Types of Ingress Controllers**
- Nginx
- HAProxy
- Traefik
- F5
- Contour
- Cloud specific controllers [AWS,AZURE,GCP]
## Ingress Resources
- An Ingress Resource is a K8S API object that manages external access to services within a cluster, typically http.
- It provides a way to define how http/https traffic should be routed based on rules defined in the resource.
- Using Ingress Resources we can redirect the traffic from http to https.

## Deploying an Nginx-ingress controller.

~~~bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
~~~

**Verify the nginx controller**
~~~bash
kubectl get pods -n ingress-nginx
~~~

**Verify the ingress services**
~~~bash
kubectl get svc -n ingress-nginx
~~~
**OutPut was similar to**
~~~bash
root@ragh-k8s-control-191b22796c9:~# kubectl get svc -n ingress-nginx
NAME                                 TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
ingress-nginx-controller             LoadBalancer   10.100.43.27   <External_IP>   80:31064/TCP,443:32397/TCP   2d1h
ingress-nginx-controller-admission   ClusterIP      10.98.176.23   <none>          443/TCP                      2d1h
root@ragh-k8s-control-191b22796c9:~#
~~~

**Verify the ingressClass**
~~~bash
kubectl get ingressClass
~~~
**OutPut was similar to**
~~~bash
root@ragh-k8s-control-191b22796c9:~# ku get ingressClass
NAME    CONTROLLER             PARAMETERS   AGE
nginx   k8s.io/ingress-nginx   <none>       2d1h
~~~
