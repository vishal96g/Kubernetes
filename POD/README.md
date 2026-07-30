**What is a Pod?**
+ Pod is the smallest deployable unit in Kubernetes.
+ It contains one or more containers that share the same network and storage.
+ Applications run inside Pods on Worker Nodes.
+ Example: A web application Pod may contain an Nginx container and a sidecar logging container.

<img width="900" height="550" alt="36c9535d-5f24-453f-bfbc-6f339703d771" src="https://github.com/user-attachments/assets/7d090425-88c8-4b02-98df-862b52644e6d" />


```
Yaml file name: pod.yml

kind: Pod
apiVersion: v1
metadata:
  name: nginx-pod
  namespace: magic-vision
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80

```

**Basic Commands**

+ **Create Pod from YAML:** kubectl apply -f pod.yml
+ **Get Pods:** kubectl get pods
+ **List Pods in a Namespace:** kubectl get pods -n my-namespace
+ **Describe Pod:** kubectl describe pod my-pod
+ **Check Pod Logs:** kubectl logs my-pod
+ **Execute Command Inside Pod:** kubectl exec -it my-pod -n my-namespace -- /bin/bash
+ **Delete Pod:** kubectl delete pod my-pod

