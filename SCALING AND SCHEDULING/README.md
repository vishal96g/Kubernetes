
# Resource Quotas
+ A ResourceQuota is a Kubernetes object that limits the total amount of resources (CPU, memory, storage) that can be used within a Namespace.
+ ResourceQuota is defined per namespace.
+ Your Deployment’s Pods must request resources 

**Why Do We Need a ResourceQuota?**
+ Imagine a Kubernetes cluster is shared by three teams:
 + Team A
 + Team B
 + Team C

**Without a ResourceQuota:**
+ Team A creates 200 Pods.
+ Team A uses all CPU and memory.
+ Teams B and C cannot create new Pods because the cluster has no resources left.

**With a ResourceQuota:**
+ Each team gets a fixed amount of resources.
+ No team can consume more than its assigned quota.
+ Resources are shared fairly.
  
<img width="1100" height="700" alt="resousequota" src="https://github.com/user-attachments/assets/b7835cb6-8485-468e-b7bc-0558cb9e35f2" />


YAML File for ResourceQuota

```
Yaml file name: resourcequota.yml

apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
  namespace: my-namespace
spec:
  hard:
    requests.cpu: "2"
    requests.memory: 4Gi
    limits.cpu: "4"
    limits.memory: 8Gi
    pods: "10"

```

**Example Deployment with Resource Requests**

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  namespace: my-namespace
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx
        resources:              # ResourceQuota applied in the container inside the deployment YAML file.
          requests:
            cpu: "500m"           # Minimum Required
            memory: "512Mi"
          limits:
            cpu: "1"
            memory: "1Gi"         # Maximum Uses 
```
