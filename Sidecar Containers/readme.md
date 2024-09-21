# Sidecar Containers
- Sidecar containers are nothing but, where additional containers are run alongside with the main container inside the POD.
- Mainly they are used for `Logging`,`Monitoring`, `Proxy` etc.

### Types of sidecar containers
- Init Containers
- Adapter Containers
- Ambassador containers [Replaced with proxy containers]

## Init Containers
- Always starts before the main container and complete before the main container starts.
- We can have multiple init containers and they execute 
