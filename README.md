# What is Kubernetes?

1. Kubernetes (also called K8s) is an open-source container orchestration platform.<br>
2. It is used to automate the deployment, Instead of managing containers manually, Kubernetes automatically handled scheduling, scaling, self-healing, and load balancing.

## Key Features:
**1. Automated Deployment:** Automated Deployment: Deploys applications across cluster nodes.<br>
**2. Auto Scaling:** Increases or decreases pods based on CPU or memory usage using the Horizontal Pod Autoscaler (HPA).<br>
**3. Self-Healing:** Restarts failed containers, replaces unhealthy pods, and reschedules workloads if a node fails.<br>
**4. Load Balancing:** Distributes traffic across multiple pods using Services.<br>
**5. Rolling Updates & Rollbacks:** Updates applications without downtime and allows quick rollback if an issue occurs.<br>
**6. Service Discovery:** Pods communicate with each other using Kubernetes Services and DNS.<br>

## Important Kubernetes Objects
**1. Pod:** Smallest deployable unit containing one or more containers.<br>
**2. Deployment:** Manages pod creation, updates, and scaling.<br>
**3. ReplicaSet:** Ensures the desired number of pod replicas are running.<br>
**4. Service:** Exposes applications internally or externally.<br>
**5. ConfigMap:** Stores non-sensitive configuration.<br>
**6. Secret:** Stores sensitive data like passwords and API keys.<br>
**7. Ingress:** Routes external HTTP/HTTPS traffic to services.<br>
**8. Namespace:** Logically separates resources within the cluster.<br>

# What is kubectl in Kubernetes?
+ kubectl (pronounced "cube-control") is the command-line tool used to communicate with and manage a Kubernetes cluster.
+ It allows you to create, view, update, and delete Kubernetes resources such as Pods, Deployments, and Services.

## Common kubectl Commands
```
kubectl get pods
kubectl get services
kubectl create deployment nginx --image=nginx
kubectl delete pod nginx
kubectl describe pod nginx
kubectl logs nginx
```

# Kubernetes architecture
+ Kubernetes follows a Master-Worker architecture (also called Control Plane and Worker Nodes).
+ **Control Plane (Master Node):** Manages the entire Kubernetes cluster. It makes decisions about scheduling Pods, maintaining the desired state, and monitoring the cluster.
+ **Worker Nodes:** Run the actual application containers inside Pods.

![kubernetes](https://github.com/user-attachments/assets/1968bb04-9cff-42b0-aad5-91905069cdb1)

## Control Plane (Master Node)
**1. API Server:** 
+ API server is the entry point to the Kubernetes cluster.
+ All commands (kubectl, CI/CD pipelines, or REST API calls) go through the API Server.
+ It validates requests and updates the cluster state in etcd.
+ Example: When you run **"kubectl apply -f deployment.yaml"** the request first goes to the API Server.

**2. etcd:**
+ etcd is a distributed key-value database used by Kubernetes to store all cluster information and configuration data. 
+ If etcd is lost and no backup exists, the cluster cannot be recovered.

**3. Scheduler:**
+ Assigns newly created Pods to the most suitable Worker Node.
+ Makes decisions based on CPU, memory, taints, tolerations, node affinity, and resource availability.

**4. Controller Manager:**
+ Continuously checks whether the cluster's actual state matches the desired state.
+ If a Pod crashes, it creates a new Pod automatically.
+ Examples of controllers:
  + Deployment Controller
  + ReplicaSet Controller
  + Node Controller
  + Job Controller
    
**5. Cloud Controller Manager (Optional):**
+ Used in cloud environments like AWS EKS, Azure AKS, or Google GKE.
+ Manages cloud resources such as Load Balancers, storage volumes, and nodes.

## Worker Node
**1. kubelet:**
+ Kublet is an agent running on every Worker Node.
+ Receives instructions from the API Server.
+ Ensures the required Pods are running.

**2. Kube-Proxy:**
+ Kube-proxy is the Kubernetes component (a process running on every worker node).

**3. Service Proxy**
+ Service Proxy is the function or feature of forwarding traffic from a Service to the correct Pod OR Exposes running applications inside the containers/pods to the end users.
+ kube-proxy performs the Service Proxy function.
  + Service Proxy = Job/Function
  + kube-proxy = Worker that does the job

**4. Pods:**
+ Pod is the smallest deployable unit in Kubernetes.
+ A Pod contains one or more containers that share the same network and storage.

**5. CNI (Container Network Interface)**
+ Responsible for establishing the network connection between Control Plane and Worker Nodes.

