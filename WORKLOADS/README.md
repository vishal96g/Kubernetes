# 1. Deployment

**What is a Deployment?**
+ A Deployment is a Kubernetes resource used to deploy, manage, and update applications.
+ It manages ReplicaSets.
+ It ensures that the desired number of Pod replicas are always running.
+ It supports rolling updates, allowing application updates with zero or minimal downtime.
+ It supports rollbacks, so you can quickly revert to the previous version if a deployment fails.
+ It allows you to scale applications by increasing or decreasing the number of replicas.
+ It continuously ensures that the actual state matches the desired state.

**How Deployment Works**
+ You create a Deployment.
+ The Deployment creates a ReplicaSet.
+ The ReplicaSet creates the required number of Pods.
+ If a Pod fails, the ReplicaSet creates a new one.
+ During an application update, the Deployment performs a rolling update by replacing Pods gradually.


<img width="1000" height="700" alt="Deploments" src="https://github.com/user-attachments/assets/77876336-66c5-4b74-919a-369265a86203" />


YAML File for Deployment

```
Yaml file name: deployment.yml


kind: Deployment
apiVersion: apps/v1
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
      name: nginx-dep-pod 
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80

```

**Basic Commands**

+ **Create Deployment:** kubectl apply -f deployment.yml
+ **Get Deployments:** kubectl get deployments -n namespace
+ **Describe Deployment:** kubectl describe deployment my-deployment -n namespace
+ **Scale Deployment:** kubectl scale deployment my-deployment -n namespace --replicas=5
+ **Update Image:** kubectl set image deployment/my-deployment -n namespace nginx-container=nginx:1.25
+ **Delete Deployment:** kubectl delete deployment my-deployment




# 2. StatefulSets 
# 3. DaemonSets
# 4. ReplicaSets
# 5. Jobs
# 6. CronJobs


