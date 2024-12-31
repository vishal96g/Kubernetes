# Q. What is namespace in kubernetes?

Namespace is like a logical separation and in the one namespace we can keep multiple pods and all the resources related to one application (e.g., Pod, deployment, etc)

![0_2ZZax42mERWEiD1W](https://github.com/user-attachments/assets/b237cde2-5d98-489c-bc17-e0b89409e0e6)

# Q. What is Pod in kubernetes?

Pod is the smallest unit of the Kubernetes inside the containers are running and pod can be a group of one or more containers that share resources and run in a shared context and if a pod fails, Kubernetes can automatically create a new replica of it.

![k8s-pods](https://github.com/user-attachments/assets/bf757784-dd6c-48cb-8de2-58d3663390db)

# Q. What is Deployment in kubernetes?

A Deployment is a Kubernetes resource that provides declarative application updates. It manages the desired state of Pods (e.g., the number of replicas)

![1_KtnpIx1twobr16FP7hvAUg](https://github.com/user-attachments/assets/8b6a9983-9d1d-4732-acf0-9e5f7f67cf09)

# Manifests files for namespace, pod and deployment

Namespace manifests files

```bash
apiVersion: v1
kind: Namespace

metadata:
 name: nginx
```

Pod manifests files

```bash
apiVersion: v1
kind: Pod

metadata:
 name: nginx-pod
 namespace: nginx

spec:
 containers:
  - name: nginx
    image: nginx:latest
    ports:
      - containerPort: 80
```

Deployment manifests files

```bash
apiVersion: apps/v1
kind: Deployment

metadata:
 name: nginx-deployment
 namespace: nginx
 labels:
   app: nginx-app

spec:
 replicas: 5
 selector:
  matchLabels:
    app: nginx-app

 template:
   metadata:
    labels:
     app: nginx-app

   spec:
    containers:
    - name: nginx-pod
      image: nginx:latest
      ports:
      - containerPort: 80
```


# 1. We can apply manifests to the Kubernetes cluster using 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗮𝗽𝗽𝗹𝘆.

```bash
 kubectl apply -f namespace.yml
 kubectl apply -f pod.yml
 kubectl apply -f deployment.yml
```

![1](https://github.com/user-attachments/assets/54eafa31-b389-4f85-b9ef-a6fe754f3934)

# 2. We can verify the resources are created using 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗴𝗲𝘁.

```bash
kubectl get namespaces
kubectl get pods -n my-namespace
kubectl get deployments -n my-namespace
```

![2](https://github.com/user-attachments/assets/442c8b94-4cad-45e6-8064-2389aca4c8a3)

# 3. We can delete/remove the resources from my cluster, we can use 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗱𝗲𝗹𝗲𝘁𝗲.

```bash
kubectl delete -f pod.yml
kubectl delete -f deployment.yml
kubectl delete -f namespace.yml
```

![3](https://github.com/user-attachments/assets/c23c0ef8-f282-498d-b1ad-4ccab2fd4138)




