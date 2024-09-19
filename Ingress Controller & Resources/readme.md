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
**Verify the ingressClass**
~~~bash
kubectl get ingressClass
~~~
