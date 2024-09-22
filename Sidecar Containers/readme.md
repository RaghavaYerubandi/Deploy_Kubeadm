# Sidecar Containers
- Sidecar containers are nothing but, where additional containers are run alongside with the main container inside the POD.
- Mainly they are used for `Logging`,`Monitoring`, `Proxy` etc.

### Types of sidecar containers
- Init Containers
- Adapter Containers
- Ambassador containers [Replaced with proxy containers]

## Init Containers
- Always starts before the main container and complete before the main container starts.
- We can have multiple init containers and they execute sequencitially.
- Main containers start after once init containers complete their job.
- Main Containers & Init containers won't run simultaneously.
- Init Containers are used for checking dependencies.
  
## Adapter Containers.
- Run along with Main Containers.
- Used for provide metrics collection, Tracing, logs Copy etc.
- Good for proxy which can be provide mutual TLS with service mesh.
