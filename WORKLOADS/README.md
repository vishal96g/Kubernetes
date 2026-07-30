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


# 2. ReplicaSets
**What is a ReplicaSets?**
+ A ReplicaSet is a Kubernetes resource that ensures a specified number of Pod replicas are running at all times.
+ Its main purpose is to provide high availability by maintaining the desired number of Pods.
+ If a Pod fails or is deleted, the ReplicaSet automatically creates a new Pod.
+ If there are more Pods than required, the ReplicaSet terminates the extra Pods.
+ It uses Labels and Selectors to identify and manage Pods.
+ ReplicaSets are usually not created directly in production; they are managed by a Deployment.
+ ReplicaSet supports scaling by increasing or decreasing the number of Pod replicas.


<img width="1000" height="700" alt="Replicasets" src="https://github.com/user-attachments/assets/ae1cf707-f8ef-4104-8799-7ed444442751" />


YAML File for ReplicaSets

```
Yaml file name: replicaset.yml


kind: ReplicaSet
apiVersion: apps/v1
metadata:
  name: nginx-replicasets
  namespace: magic-vision
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      name: nginx-rep-pod 
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
+ **Create ReplicaSet:** kubectl apply -f replicaset.yml
+ **Get ReplicaSets:** kubectl get replicaset OR kubectl get rs
+ **Describe ReplicaSet:** kubectl describe rs my-replicaset
+ **Delete ReplicaSet:** kubectl delete rs my-replicaset


# 3. DaemonSets

**What is a DaemonSets?**
+ A DaemonSet is a Kubernetes workload resource that ensures one Pod runs on every eligible Worker Node in the cluster.
+ As new nodes are added to the cluster, Kubernetes automatically creates the DaemonSet Pod on those nodes. If a node is removed, the corresponding Pod is automatically removed.

<img width="1000" height="700" alt="DaemonSet" src="https://github.com/user-attachments/assets/5019ef37-7081-4416-9a15-e31d2c2e5533" />


YAML File for DaemonSets

```
Yaml file name: daemonset.yml


kind: DaemonSet
apiVersion: apps/v1
metadata:
  name: nginx-daemonset
  namespace: magic-vision
spec:
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      name: nginx-dmn-pod 
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

+ **Create DaemonSet:** kubectl apply -f daemonset.yml
+ **Get DaemonSets:** kubectl get daemonsets
+ **Describe DaemonSet:** kubectl describe daemonset my-daemonset
+ **Check Pods Created by DaemonSet:** kubectl get pods -o wide
+ **Delete DaemonSet:** kubectl delete daemonset my-daemonset




# 2. StatefulSets 


# 5. Jobs
# 6. CronJobs


