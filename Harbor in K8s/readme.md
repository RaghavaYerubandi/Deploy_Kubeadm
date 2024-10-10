# Deploying Harbor in Kubernetes
- Harbor is an open-source container image registry that provides enhanced features for managing, securing, and distributing container images.
- It is often used in Kubernetes environments as a private registry to store, scan, and manage container images.
- Harbor adds security, compliance, and management features on top of a basic registry like Docker Hub or the vanilla Docker Registry.

**Prerequisites**
- Helm package manager.

### Installing Helm via `apt` package manager
~~~bash
curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
sudo apt-get install apt-transport-https --yes
echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
~~~

**Verify the helm version**
~~~bash
root@ragh-k8s-control-191b22796c9:~# helm version
version.BuildInfo{Version:"v3.16.1", GitCommit:"5a5449dc42be07001fd5771d56429132984ab3ab", GitTreeState:"clean", GoVersion:"go1.22.7"}
~~~

### Deploy Harbor with Helm
~~~bash
helm repo add harbor https://helm.goharbor.io
helm repo update
helm install my-harbor harbor/harbor --namespace harbor --create-namespace
~~~
**O/P**
~~~bash
root@ragh-k8s-control-191b22796c9:~# helm repo add harbor https://helm.goharbor.io
helm repo update
helm install my-harbor harbor/harbor --namespace harbor --create-namespace
"harbor" has been added to your repositories
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "harbor" chart repository
Update Complete. ⎈Happy Helming!⎈
NAME: my-harbor
LAST DEPLOYED: Thu Oct 10 01:18:34 2024
NAMESPACE: harbor
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Please wait for several minutes for Harbor deployment to complete.
Then you should be able to visit the Harbor portal at https://core.harbor.domain
For more details, please visit https://github.com/goharbor/harbor
root@ragh-k8s-control-191b22796c9:~#
~~~
After deploying Harbor using Helm, it will start various components such as:

- harbor-core: Core logic for managing images and users.
- harbor-portal: The UI for managing Harbor.
- harbor-jobservice: Manages asynchronous tasks like image scanning and replication.
- harbor-registry: The backend registry service for storing container images.
- harbor-db: PostgreSQL database to store metadata.
- harbor-chartmuseum: Optional component for managing Helm charts.
- harbor-notary: For signing and verifying images (optional).
**Verifying Harbor components**
~~~bash
root@ragh-k8s-control-191b22796c9:~# ku get pods -n harbor 
NAME                                   READY   STATUS    RESTARTS      AGE
my-harbor-core-75fc66444-db8fq         0/1     Running   1 (32s ago)   2m34s
my-harbor-database-0                   0/1     Pending   0             2m34s
my-harbor-jobservice-79f94949d-hbrkg   0/1     Pending   0             2m34s
my-harbor-portal-7c4d89969d-wfrhf      1/1     Running   0             2m34s
my-harbor-redis-0                      0/1     Pending   0             2m34s
my-harbor-registry-5c7c85f699-86ct2    0/2     Pending   0             2m34s
my-harbor-trivy-0                      0/1     Pending   0             2m34s
~~~
**Verifying Harbor Service**
~~~bash
root@ragh-k8s-control-191b22796c9:~# ku get svc -n harbor 
NAME                   TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)             AGE
my-harbor-core         ClusterIP   10.102.125.168   <none>        80/TCP              5m56s
my-harbor-database     ClusterIP   10.97.157.60     <none>        5432/TCP            5m56s
my-harbor-jobservice   ClusterIP   10.97.115.38     <none>        80/TCP              5m56s
my-harbor-portal       ClusterIP   10.99.149.133    <none>        80/TCP              5m56s
my-harbor-redis        ClusterIP   10.96.199.11     <none>        6379/TCP            5m56s
my-harbor-registry     ClusterIP   10.105.89.134    <none>        5000/TCP,8080/TCP   5m56s
my-harbor-trivy        ClusterIP   10.105.161.218   <none>        8080/TCP            5m56s
~~~
### Creating a Harbour Ingress
- Create a ingress resource in harbor namespace only.
~~~yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: harbor-ingress
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
spec:
  ingressClassName: "nginx"
  tls:
  - hosts:
    - "myapp.ragh.xyz"
    secretName: tls-secret
  rules:
  - host: harbor.ragh.xyz
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-harbor-portal
            port:
              number: 80
~~~
**Verify Harbor ingress**
~~~bash
root@ragh-k8s-control-191b22796c9:~# ku get ingress
NAME             CLASS   HOSTS                          ADDRESS         PORTS     AGE
harbor-ingress   nginx   harbor.ragh.xyz                45.249.241.12   80, 443   61s
~~~

- Add A name record the DNS.
