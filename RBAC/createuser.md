## Creating rsa key for user
~~~bash
openssl genrsa -out user.key 2048
~~~

**O/P**
~~~bash
openssl genrsa -out dev.key 2048
Generating RSA private key, 2048 bit long modulus (2 primes)
................+++++
....................................................................................+++++
e is 65537 (0x010001)
~~~
## Creating csr for user
~~~bash
openssl req -new -key user.key -out user.csr -subj "/CN=username/O=groupname"
~~~
**O/P**

~~~bash
openssl req -new -key dev.key -out dev.csr -subj "/CN=dev/O=development"
root@ragh-k8s-control-191b22796c9:~/auths# ls
ca.crt  ca.key  dev.csr  dev.key
~~~

## Creating crt for a user
~~~bash
openssl x509 -req -in user.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out user.crt -days 365
~~~

**O/P**
~~~bash
 openssl x509 -req -in dev.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out dev.crt -days 365
Signature ok
subject=CN = dev, O = development
Getting CA Private Key
root@ragh-k8s-control-191b22796c9:~/auths# ls
ca.crt  ca.key  dev.crt  dev.csr  dev.key
~~~
