# Role Based Access Control
- RBAC in K8s is a method for regulating access to resource within the cluster.
- It allows us to specify which users or groups can perform certain actions on specific resources.
- RBAC uses `Roles` & `Role Bindings` to grant permission.

**Permissions**

     - Role
     - Cluster Role
- Role will work for specific namespaces.
- Cluster role will work for cluster level.

**Bindings**
- Role Bindings
- Cluster Bindings

- Using permission, we can create access for the resources.
- We need to bind roles to the users, so that the user have limited access.
- Using `RBAC`, we provide access to the below objects.

      - Admins & Developers.
      - PODs Service accounts
