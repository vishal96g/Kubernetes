
# 1. What is Helm?
+ Helm is a package manager for Kubernetes that simplifies deploying and managing applications using reusable templates called Charts.

**Install Helm on Linux:**
+ Using the Official Installation Script
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-4
chmod 700 get_helm.sh
./get_helm.sh
```

## To create a Helm Chart for an Apache application

**1. Verify Helm is installed:**
```
helm version
```

**2. Create a New Helm Chart:**
```
helm create apache-chart
```

+ This creates the following directory structure: 
<img width="311" height="330" alt="image" src="https://github.com/user-attachments/assets/c26103fe-a5cc-4b99-924a-94cbd3ecdf49" />

+ Understand the Files:
<img width="735" height="396" alt="image" src="https://github.com/user-attachments/assets/8e22efd8-7fd7-41ed-8952-fdea36486680" />

**3. Update Chart.yaml**
```
apiVersion: v2
name: apache-chart
description: Helm chart for Apache Web Server
type: application
version: 0.1.0
appVersion: "2.4"
```

**4. Update values.yaml**
```
replicaCount: 2

image:
  repository: httpd
  tag: "2.4"
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 80

resources:
  requests:
    cpu: 100m
    memory: 128Mi
  limits:
    cpu: 200m
    memory: 256Mi
```

**5. Package the Chart**
```
helm package apache-chart/
```

**5. Install the Chart**
```
helm install dev-apache apache-chart -n dev-apache --create-namespace
```

# 2. What is an Init Container?
+ An Init Container is a special container that runs before the main application container starts.
+ Its job is to perform setup tasks, such as:
   + Download configuration files
   + Wait for a database to become available
   + Create required directories
   + Initialize data

**Important:**
+ It runs only once.
+ It must complete successfully before the main container starts.
+ If it fails, the main container will not start.

# 3. What is a Sidecar Container?
+ A Sidecar Container is a helper container that runs alongside the main application container in the same Pod.
+ It provides supporting services while the application is running.
+ Common uses:
   + Collect logs
   + Monitor the application
   + Proxy network traffic
   + Synchronize files

**Important:**
+ Starts with the main container.
+ Runs as long as the Pod is running.
