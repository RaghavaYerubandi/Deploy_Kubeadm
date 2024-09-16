# DePloyment stratagies in K8S
- Max surge
- Max unavailable

## **Max Surge**
- Max surge specifies the maximum no.of pods that can be created during an update.
- If the Max surge is set to 2, while updating the deployment a 2 new pods will be created with an updates and 2 old pods will terminate.
- A new replicaset also be created[with updated pods].

## **Max Unavailable**
- Max Unavailable specifies the maximum no of pods that can be unavailable during update process.
- The available pods can handle thw traffic mean while.

**Example**
Currently the deployment is running with 9 pods, which means replica set is set to 9.
~~~bash
root@ragh-k8s-control-191b22796c9:~/deployments# ku get pods
NAME                     READY   STATUS    RESTARTS   AGE
myapp-58644d7749-22l8q   1/1     Running   0          29s
myapp-58644d7749-7dmp2   1/1     Running   0          29s
myapp-58644d7749-9ght8   1/1     Running   0          63s
myapp-58644d7749-9knpx   1/1     Running   0          29s
myapp-58644d7749-b7kgt   1/1     Running   0          29s
myapp-58644d7749-bxhrd   1/1     Running   0          29s
myapp-58644d7749-j9mm5   1/1     Running   0          63s
myapp-58644d7749-jjrhd   1/1     Running   0          63s
myapp-58644d7749-ksz9k   1/1     Running   0          29s
myapp-58644d7749-lk6f6   1/1     Running   0          29s
myapp-58644d7749-q8x4h   1/1     Running   0          29s
myapp-58644d7749-s4nnq   1/1     Running   0          29s
myapp-58644d7749-sch2t   1/1     Running   0          29s
myapp-58644d7749-v58m9   1/1     Running   0          29s
myapp-58644d7749-vbw7k   1/1     Running   0          29s
~~~
Applying a Rolling Update.
~~~bash
ku apply -f deploy_maxsurge.yaml
~~~
Rolling Update was success.
~~~bash
root@ragh-k8s-control-191b22796c9:~/deployments# ku rollout status                                                                     deployment myapp
deployment "myapp" successfully rolled out
~~~
Verifying the PODs in new update.
~~~bash
root@ragh-k8s-control-191b22796c9:~/deployments# ku get pods                                                                          NAME                     READY   STATUS    RESTARTS   AGE
myapp-859fc568c7-l2j5s   1/1     Running   0          15s
myapp-859fc568c7-ndj4b   1/1     Running   0          16s
myapp-859fc568c7-vvwnm   1/1     Running   0          16s
~~~
Verifying the RS[replica sets]
~~~bash
root@ragh-k8s-control-191b22796c9:~/deployments# ku get rs
NAME               DESIRED   CURRENT   READY   AGE
myapp-58644d7749   0         0         0       3d14h
myapp-859fc568c7   3         3         3       3d13h
~~~
**Note:-**
- Here a new replicaset [myapp-859fc568c7] has been deployed with new pods.
- We can see the old replicaset [myapp-58644d7749] here.

## Rollback
- We can rollback the updates as well.
~~~bash
ku rollout undo deployment myapp
deployment.apps/myapp rolled back
~~~
Verify the Pods & RS.
~~~bash
root@ragh-k8s-control-191b22796c9:~/deployments# ku get pods
NAME                     READY   STATUS    RESTARTS   AGE
myapp-58644d7749-7jw7l   1/1     Running   0          67s
myapp-58644d7749-89jkw   1/1     Running   0          68s
myapp-58644d7749-cs2wj   1/1     Running   0          3s
myapp-58644d7749-gmw8p   1/1     Running   0          3s
myapp-58644d7749-h5z62   1/1     Running   0          3s
myapp-58644d7749-hjdfm   1/1     Running   0          68s
myapp-58644d7749-j2d2f   1/1     Running   0          3s
myapp-58644d7749-mf6bn   1/1     Running   0          3s
myapp-58644d7749-zvhdr   1/1     Running   0          3s
~~~
**Verify the Replicaset**

~~~bash
root@ragh-k8s-control-191b22796c9:~/deployments# ku get rs
NAME               DESIRED   CURRENT   READY   AGE
myapp-58644d7749   9         9         9       3d15h
myapp-859fc568c7   0         0         0       3d14h
~~~
- As the rolling updates were undo, the pods are running on the old replicaset.
