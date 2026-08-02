
# What is Resource Quotas & Limits?
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

# What is Probes? 
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
