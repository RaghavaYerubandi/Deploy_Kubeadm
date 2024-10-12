# Adding nfs_storage to K8s cluster.

- Create a nfs server.
~~~bash
sudo apt update
sudo apt install nfs-kernel-server
~~~
- Create a directory for storage.
~~~bash
mkdir -p /k8s-file-system/nfs-storage
~~~
- Set the permissions
~~~bash
chown -R nobody:nogroup /k8s-file-system/nfs-storage
chown -R 777 /k8s-file-system/nfs-storage
~~~
- Export the Directory
~~~bash
echo "/k8s-file-system/nfs-storage/ *(rw,sync,no_subtree_check,no_root_squash)" >> /etc/exports
~~~
- Apply the NFS Export
~~~bash
exportfs -rav
systemctl restart nfs-kernel-server
~~~

## Install nfs using helm
- Add the Helm repository for the NFS Subdir External Provisioner.
~~~bash
helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/
~~~
- Update the Helm repositories
~~~bash
helm repo update
~~~
- Install the chart.
~~~bash
helm install nfs-subdir-external-provisioner nfs-subdir-external-provisioner/nfs-subdir-external-provisioner \
  --set nfs.server=172.16.5.232 \
  --set nfs.path=/k8s-file-system/nfs-storage/ \
  -n nfs-system-storage \
  --create-namespace \
  --set storageClass.name=nfs-sc-file-system \
  --set storageClass.provisionerName=nfs-pn-file-system \
  --set storageClass.onDelete=delete
~~~

**OutPut**
~~~bash
NAME: nfs-subdir-external-provisioner
LAST DEPLOYED: Sat Oct 12 08:20:35 2024
NAMESPACE: nfs-system-storage
STATUS: deployed
REVISION: 1
TEST SUITE: None
~~~
