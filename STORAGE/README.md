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

# What is Persistent Volume (PV)?
+ A Persistent Volume Claim (PVC) is a request for storage made by a Kubernetes application.
+ PVCs are the requests for storage. Pods use PVCs to bind to PVs.

<img width="1000" height="600" alt="PV in k8s" src="https://github.com/user-attachments/assets/d2f08365-9808-4a98-b3df-87b50c374b77" />

YAML File for PersistentVolumeClaim (PVC)

```
Yaml file name: persistentvolumeclaim.yml


kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: local-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 2Gi
  storageClassName: local-storage

```

**Basic Commands**
+ **Apply PVC YAML:** kubectl apply -f pvc.yaml
+ **List PVCs:** kubectl get pvc
+ **Describe PVC:** kubectl describe pvc <pvc-name>
+ **Delete PVCs:** kubectl delete pvc.yaml

# To use a PersistentVolumeClaim (PVC) in a Deployment, we need:
1. Persistent Volumes (PV).
2. Persistent Volumes Claim (PVC).
3. Reference the PVC in the Deployment under volumes.
4. Mount the volume inside the container using volumeMounts.

YAML File for Deployment (with Volume)

```
Yaml file name: deployment.yml


apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: magic-vision
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        volumeMounts:
        - name: my-data
          mountPath: /var/www/html
      volumes:
      - name: my-data
        persistentVolumeClaim:
          claimName: local-pvc

```
