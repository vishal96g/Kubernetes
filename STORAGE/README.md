# What is Persistent Volume (PV)?
+ A Persistent Volume (PV) is a storage resource in Kubernetes that provides permanent storage for applications.
+ It exists independently of Pods, so data remains available even if Pods are deleted or restarted. 

<img width="1000" height="600" alt="PV in k8s" src="https://github.com/user-attachments/assets/d2f08365-9808-4a98-b3df-87b50c374b77" />


YAML File for Persistent Volumes


```
Yaml file name: persistentvolume.yml


kind: PersistentVolume
apiVersion: v1
metadata:
  name: local-pv
spec:
  capacity:
    storage: 2Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: local-storage
  hostPath:
    path: /mnt/data
 
```

**Basic Commands**
+ **Create a PV from YAML:** kubectl apply -f pv.yaml
+ **List all PVs:** kubectl get pv
+ **Describe a specific PV:** kubectl describe pv <pv-name>
+ **Delete a PV:** kubectl delete pv <pv-name>


