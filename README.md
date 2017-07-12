Ansible roles for automation de creation of lvm (Linux logical volume) and pv (OpensShift Persistent Volume).


It can be used with Jenkinsfile in the folder.

Parameters:

- node: Node with the disk storage
- disk: Disk name
- size: Size of 
- kube_nfs_volumes_kubernetes_url: The OpenShift url
- kubernetes_token: The OpenShift token
- do_PV[true|false]: If the pv should be created 
- backup[true|false]: Sets a label with value for backup
- do_Claim[true|false]: If claim should be done
- openshift_namespace: OpenShift namespace (only if the pvc is created)
- claim_name: Name for the persistent volume claim (pvc) (only if the pvc is created)
