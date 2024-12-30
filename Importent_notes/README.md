
# Q. What is Pod in kubernetes?

1. Pod is the smallest unit of the kuberntes inside the containers are running.
2. Pod can be group of one or more containers that share resources and run in a shared context. 
3. If a pod fails, Kubernetes can automatically create a new replica of it.

To get Pod:

```bash
kubectl get pod
kubectl get pod -n default
``` 
Where -n is used for the namespace and default is the namespace name and inside the default namespace my container is running.

Pod running inside namepace default:

![image](https://github.com/user-attachments/assets/1c5e6a14-72b9-4f67-84ec-bdb308da4e70)

![k8s-pods](https://github.com/user-attachments/assets/bf757784-dd6c-48cb-8de2-58d3663390db)

# Q. What is Namespace in kubernetes?

1. Namespace is like a logincal sepration.
2. In the one namespace we can keep muliple pod and all the resources related to one application.
3. Example: Pod, deployment, service etc.

To get all the namespace:

```bash
kubectl get namespaces
```

To get all the pods running particular under namespace:

```bash
kubectl get pods -n default
```

Namespace Screenshot:

![image](https://github.com/user-attachments/assets/7ad17e8b-2d81-42b6-8f1e-3542bd15e7e6)

Note: while creating any container if have't specify the namespace then by default that container will run under the default namespace.


![0_2ZZax42mERWEiD1W](https://github.com/user-attachments/assets/b237cde2-5d98-489c-bc17-e0b89409e0e6)
