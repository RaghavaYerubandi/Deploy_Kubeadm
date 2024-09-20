# Probes in K8s
- Probes are used to check the health of containers running in a pod.
- They ensure that the applications are running smoothly and can automatically restart the pod, if it fails.

**Types of Probes**
- Liveness Probe
- Rediness Probe
- Startup Probe

## Liveness Probe
- Liveness probe will check health of te container running inside the POD is still alive or not.
- Liveness probe can restart the POD.
- Liveness Probe will check the health status of the POD using `http get`, `tcp socket`, `command`.

## Rediness Probe
- Readiness Probe checks the container inside the POD is ready to serve the traffic.
- If the POD fails or freezes, the readiness probe will stop the tarffic to the POD.
- Readiness Probe also will check the health status of the POD using `http get`, `tcp socket`, `command`.

## Startup Probe
- A startup Probe checks whether the application within the container has started or not, if the startup probe fails then K8S will kill the container and restart it according to the restart policy.
- This can be used for applications that have a long startuptime. 
