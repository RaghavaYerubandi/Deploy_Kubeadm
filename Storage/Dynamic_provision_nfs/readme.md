# Dynamic Provision using nfs
- To use the NFS storage class `nfs-sc-file-system` with dynamic provisioning in your deployment, you need to define a persistent volume claim (PVC) that refers to the storage class, and then mount that PVC in each pod's container. 
- Initially, when the pod with PVC configured was deployed, it will ask the `CSI-provisioner` that need an storage. 
- And as we defined the PVC already. `Storage-provisioner` will create a PV and the PVC we create will attcahe to the POD and it will claim the storage from the PV.

**NOTE:-** PVC shloud be mentioned in POD defination.
**Steps**
- [Create a Storage Class](https://github.com/RaghavaYerubandi/Deploy_Kubeadm/blob/main/Storage/nfs_storage.md#install-nfs-using-helm)
- Create a PVC
- Create a POD with PVC defined.

**Creating a PVC**
- PVC accessmode should be `ReadWriteMany` as the all the PODs will access the same storage.

~~~yaml
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nfs-pvc
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 1Gi
  storageClassName: nfs-sc-file-system
~~~

