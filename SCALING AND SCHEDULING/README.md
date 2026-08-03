
# 1. What is Resource Quotas & Limits?
+ A ResourceQuota is a Kubernetes object that limits the total amount of resources (CPU, memory, storage) that can be used within a Namespace.
+ ResourceQuota is defined per namespace.
+ Your Deployment’s Pods must request resources 

**Why Do We Need a ResourceQuota & Limits?**
+ Imagine a Kubernetes cluster is shared by three teams:
 + Team A
 + Team B
 + Team C

**Without a ResourceQuota & Limits:**
+ Team A creates 200 Pods.
+ Team A uses all CPU and memory.
+ Teams B and C cannot create new Pods because the cluster has no resources left.

**With a ResourceQuota & Limits:**
+ Each team gets a fixed amount of resources.
+ No team can consume more than its assigned quota.
+ Resources are shared fairly.
  
<img width="1100" height="700" alt="resousequota" src="https://github.com/user-attachments/assets/b7835cb6-8485-468e-b7bc-0558cb9e35f2" />


YAML File for ResourceQuota & Limits

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

**Example Deployment with Resource Requests & Limits**

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
        resources:              # ResourceQuota & Limits applied in the container inside the deployment YAML file.
          requests:
            cpu: "500m"           # Minimum Required
            memory: "512Mi"
          limits:
            cpu: "1"
            memory: "1Gi"         # Maximum Uses 
```

# 2. What is Probes? 
+ Probes are health checks in Kubernetes.
+ Kubernetes uses probes to monitor the health of an application running inside a Pod.
+ Based on the probe results, Kubernetes decides whether to wait for the application, send traffic to it, or restart it.
+ There are three types of probes, and each has a different purpose.
  
+ **1. Startup Probe**
    + The Startup Probe tells Kubernetes, "My application is still starting. Please wait and don't restart me yet.
  + **Example:**
    + A Spring Boot application takes 2 minutes to start. The Startup Probe prevents Kubernetes from restarting it before it has finished starting.

+ **2. Readiness Probe**
    + The Readiness Probe tells Kubernetes, "Don't send users' requests to me until I'm fully ready.
    + Kubernetes sends an HTTP GET request to the specified path and port. If the check succeeds, the Pod is marked as Ready and starts receiving traffic.
    + If it fails, the Pod stays running but is removed from the Service, so no traffic is sent until it becomes ready again.
  + **Example:**
    + Your application has started, but it's still connecting to the database. Kubernetes waits until the connection is ready before sending traffic.
   
+ **3. Liveness Probe**
    + The Liveness Probe tells Kubernetes, "If I stop responding or get stuck, restart me.
  + **Example:**
    + The application hangs because of a bug. The Liveness Probe detects that it is no longer responding, and Kubernetes automatically restarts the container.

**Probes are configured inside the container section of a Kubernetes Deployment YAML.**

**Example: Deployment with all 3 probes**

```
containers:
      - name: my-app
        image: nginx:latest
        ports:
        - containerPort: 80

        startupProbe:
          httpGet:
            path: /          # Kubernetes sends an HTTP request to http://<pod-ip>:80/ If it gets a successful response, the probe passes.
            port: 80
          initialDelaySeconds: 5  # How long Kubernetes waits before the first check 
          periodSeconds: 10       # Kubernetes performs the probe every 10 seconds (Means 1st after 5 second 2nd after 15 second and 3rd after 25 second). 
          failureThreshold: 30    # Allow 30 consecutive failed checks (30 × 10 seconds = 300 seconds (5 minutes). 

        readinessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 5

        livenessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 10
          periodSeconds: 10


```

# 3. What are Taints and Tolerations?
+ Taints and tolerations are used in Kubernetes to control which Pods can run on which Nodes.
+ They work like a "filter system" for scheduling Pods.
   + **Taint:**
     + A taint is applied to a Node (puts a restriction on the Node).
     + It tells Kubernetes "Do not schedule Pods on this Node unless they have permission."
     + Example:
        + ```kubectl taint node tws-cluster-worker prod=true:NoSchedule```
        +  ```kubectl taint node tws-cluster-worker2 prod=true:NoSchedule```
          
     + Remove a taint node from a Kubernetes cluster (just add the - at the end)
        + ```kubectl taint node tws-cluster-worker prod=true:NoSchedule-```
        +  ```kubectl taint node tws-cluster-worker2 prod=true:NoSchedule-```
          + Where ```tws-cluster-worker``` and  ```tws-cluster-worker2``` is the nodes in the cluster.
      
     + How to check taints on a node.
        + ```kubectl describe node tws-cluster-worker```

      
   + **Toleration:**
     + A toleration is added to a Pod (allows the Pod to use that Node)
     + It tells Kubernetes "This Pod is allowed to run on a Node with this taint."

**Example: Pod with a Toleration**

 ```
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
  tolerations:
  - key: "prod"
    operator: "Equal"
    value: "true"
    effect: "NoSchedule"

```

<img width="1000" height="650" alt="HPA_VPA" src="https://github.com/user-attachments/assets/48193d8d-b834-40ad-9327-e36357e35574" />

# 4. What is Autoscaling in Kubernetes?
+ Autoscaling means Kubernetes automatically increases or decreases resources based on the application's load.
+ Instead of manually changing the number of Pods or Nodes, Kubernetes does it automatically.

**Why do we need Autoscaling?**

**Without autoscaling:**
+ **High traffic →** Application becomes slow or crashes.
+ **Low traffic →** Resources are wasted because too many Pods are running.

**With autoscaling:**
+ **High traffic →** Kubernetes adds Pods.
+ **Low traffic →** Kubernetes removes extra Pods.

This helps improve performance and reduce resource usage.

**Types of Autoscaling in Kubernetes**
+ There are three main types.
  + Horizontal Pod Autoscaler (HPA)
  + Vertical Pod Autoscaler (VPA)
  + Cluster Autoscaler (CA)
 
**Note:** 
HPA (Horizontal Pod Autoscaler) and VPA (Vertical Pod Autoscaler) rely on metrics to make scaling decisions. By default, a KIND (Kubernetes IN Docker) cluster does not include the Metrics Server. Therefore, if you want to use HPA (and VPA components that require resource metrics), you need to install the Metrics Server in the kube-system namespace.

**Why is Metrics Server needed?**
+ HPA uses CPU and memory metrics collected by the Metrics Server to automatically increase or decrease the number of pod replicas.
+ VPA uses resource usage metrics (along with its recommender component) to suggest or apply CPU and memory requests/limits.
+ Without the Metrics Server:
  + kubectl top nodes and kubectl top pods will not work.
  + HPA based on CPU/memory metrics will not function.
  + VPA recommendations based on live resource usage will be unavailable or limited.

+ **In a KIND cluster By default, KIND does not install the Metrics Server. You typically install it with:**
```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
+ **For KIND, you often also need to modify the Metrics Server deployment to include:**

```
kubectl -n kube-system edit deployment metrics-server
```
+ **Add the security bypass to deployment under ```container.args```**
```
- --kubelet-insecure-tls
- --kubelet-preferred-address-types=InternalIP,Hostname,ExternalIP
```

+ **Restart the deployment**
```
kubectl -n kube-system rollout restart deployment metrics-server
```
+ **Verify if the metrics server is running**
```
kubectl get pods -n kube-system
kubectl top nodes
```
## 1. Project for HPA (Horizontal Pod Autoscaler):


YAML File for Namespace

```
Yaml file name: namespace.yml


kind: Namespace
apiVersion: v1
metadata:
  name: ns-apche
```

YAML File for Deployment

```
Yaml file name: deployment.yml


kind: Deployment
apiVersion: apps/v1
metadata:
  name: apache-deployment
  namespace: ns-apache
spec:
  replicas: 3
  selector:
    matchLabels:
      app: apache
  template:
    metadata:
      labels:
        app: apache
    spec:
      containers:
        - name: apache
          image: httpd:latest
          ports:
            - containerPort: 80
          resources:
            requests:
              cpu: "100m"
              memory: "128Mi"
            limits:
              cpu: "200m"
              memory: "256Mi" 

```

YAML File for Service

```
Yaml file name: service.yml


apiVersion: v1 
kind: Service
metadata:
  name: apache-service
  namesapce: ns-apache
spec:
  selector:  
      name: apache
  ports:
    - protocol: TCP
      port: 80             # exposed port in the cluster
      targetPort: 80       # container port
  type: ClusterIP  

```

Expose ``` sudo -E kubectl port-forward service/apache-service -n apache 82:80 --address=0.0.0.0```


YAML File for HPA (Horizontal Pod Autoscaler)

```
Yaml file name: hpa.yml



apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: apache-hpa
  namesapce: ns-apache

spec:
  scaleTargetRef:
    kind: Deployment
    apiVersion: apps/v1
    name: apache-deployment   # This is my deployment name which one i want to scale up and scale down

  minReplicas: 1
  maxReplicas: 10

  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50

```

**Generate Load**
+ Start a temporary pod:```kubectl run -i --tty load-generator --image=busybox -n ns-apache /bin/sh```
  
+ Inside the pod, run an infinite loop:  ```while true; do wget -q -O- http://apache-deployment > /dev/null done```

