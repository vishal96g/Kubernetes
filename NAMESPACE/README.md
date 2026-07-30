
**What is a Namespace?**
+ A Namespace is a logical way to divide and organize resources within a Kubernetes cluster.
+ It helps isolate resources between different teams, projects, or environments.
+ Resources like Pods, Deployments, Services, ConfigMaps, and Secrets can exist in different namespaces without conflicting with each other.
+ It allows you to apply resource quotas, RBAC (Role-Based Access Control), and network policies for better management.
+ By default, if no namespace is specified, resources are created in the default namespace.

<img width="1000" height="650" alt="Namespace" src="https://github.com/user-attachments/assets/18add12d-ec2e-4c55-97a4-3559fdb7b624" />

**YAML File for Namespace**

```
Yaml file name: namespace.yml


kind: Namespace
apiVersion: v1
metadata:
  name: magic-vision
```
**Basic Commands**
+ **Apply Namespace:** kubectl apply -f namespace.yml
+ **Switching Namespace Context:** kubectl config set-context --current --namespace=my-namespace
+ **Listing Namespaces:** kubectl get namespaces OR kubectl get ns 
+ **Deleting a Namespace:** kubectl delete namespace my-namespace

