# StatefulSets
- StatefulSets is designed for Applications that requires persistent storage.
- StatefulSets is the workload API object used to manage stateful applications.
- Mostly Used for `Databases` & `Distributed systems`.
- StatefulSets makesure that the POD names doesn't change even they are recreated.
- They follow a sequence when we deploy multiple replicas. It follows ordinal index [0,1,2,3...].
- It will assign the same volume for the POD even after the reboot.
- StatefulSets will use headless service to communicate between them.
