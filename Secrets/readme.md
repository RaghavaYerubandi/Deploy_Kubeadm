# Secrets
- Secrets are use to store and manage sensitive informations, such as `Passwords`, `TLS-certificates`, `SSH-keys`, `OAuth tokens`.
- Storing sensitive data in secrets is more secure than storing it directly in POD specifications or configuring files.
- Secrets are applicable for only one namespaces.If required we can copy to other namespace also.

**Types of Secrets**
- TLS Secrets
- Opaque Secrets
- Container registry or docker registry secrets
- Service account token secrets.

**To list**
~~~bash
Kubectl get secret
~~~

## TLS Secrets
- Used to store TLS certificates and keys.
- Mostly used in ingress.
- Annotation used `kubernetes.io/tls`.
**To create TLS-secret**

~~~bash
kubectl create secret tls my-tls-secret --cert=path/to/cert/file --key=path/to/key/file
~~~

## Opaque Secrets
- This is the default secrets in kubernets.
- We can store `Passwords`, `API-keys`, etc.
- We can pass the information to the PODs also.

**To create Opaque Secrets**
~~~bash
kubectl create secret generic empty-secret
~~~

## Container registry Secrets
- Used to store the container registry credentials.
- By using this kubernets will pull images from private container registry.

**To create Container registry Secrets**

~~~bash
kubectl create secret docker-registry secret-tiger-docker \
  --docker-email=tiger@acme.example \
  --docker-username=tiger \
  --docker-password=pass1234 \
  --docker-server=my-registry.example:5000
~~~

## ServiceAccount token Secrets
- It is used to store a token credential that identifies a ServiceAccount.
