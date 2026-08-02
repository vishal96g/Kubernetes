# What is a Service?
+ Pods in Kubernetes are temporary.
+ Pod can be created, deleted, or replaced and their IP addresses will change change. If applications connect directly to a Pod's IP, the connection can break. 

**Service solves this problem by providing:**
+ A fixed IP address and DNS name. 
+ Automatic load balancing between multiple Pods.

<img width="1000" height="600" alt="service" src="https://github.com/user-attachments/assets/1134f5cb-4e8e-4625-8225-f2b80aa87c0b" />


YAML File for Service

```
Yaml file name: service.yml


kind: Service
apiVersion: v1
metadata:
  name: nginx-service
  namespace: magic-vision
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80          # Outside Port
      targetPort: 80    # Pod Port
  type: ClusterIP

```

**Basic Commands**

+ **Create a Service from YAML:** kubectl apply -f service.yaml
+ **List all Services:** kubectl get svc
+ **Describe a specific Service:** kubectl describe svc <service-name>
+ **Delete a Service:** kubectl delete svc <service-name>

# Types of Kubernetes Services.
Kubernetes provides 4 types of Services to expose applications in different ways. 

## 1. ClusterIP (Default) 
Accessible only within the cluster.  

**Use Case:**
+ Pod-to-Pod communication
+ Backend database
+ Internal databases
+ Backend APIs

## 2. NodePort
A NodePort Service exposes your application outside the Kubernetes cluster by opening a specific port on every worker node.

**Use Case:**
+ Testing application
+ Development environments
+ **For example:** Node-IP:NodePort (192.168.1.100:30080)
  + **where,** 192.168.1.100 = Worker node's IP address
  + 30080 = NodePort opened by Kubernetes


## 3. LoadBalancer
+ A LoadBalancer Service creates an external load balancer (supported by cloud providers) to expose the application to the internet.
   
**Use Case:**
+ Production applications
+ Public websites
+ APIs
+ Production website on AWS/Azure/GCP

## 4. ExternalName
An ExternalName Service maps a Kubernetes Service to an external DNS name.

**Use Case:**
+ Accessing external databases
+ Connecting to third-party APIs

# What is an Ingress?
Ingress is a Kubernetes object which receives incoming requests and forwards them to the correct Kubernetes Service. 
+ Example: Suppose you have two applications
  + app1-service
  + app2-service
  
**Easy Way to Remember**
+ **Service →** Connects users to Pods.
+ **Ingress →** Connects the Internet to the correct Service.

<img width="1100" height="700" alt="ingress_controller" src="https://github.com/user-attachments/assets/be0cdbee-8866-45c0-b9e3-168635be91a9" />


# Configure the NGINX Ingress Controller in a Kind Cluster

**Step 1: Install the NGINX Ingress Controller**
 For a Kind cluster, install the NGINX Ingress Controller by running
```
kubectl apply -f https://kind.sigs.k8s.io/examples/ingress/deploy-ingress-nginx.yaml
```

**Step 2: Verify the Installation**
Check that the ingress-nginx namespace has been created:

```
kubectl get namespaces
```

**You should see an output similar to:**

```
NAME              STATUS   AGE
default           Active   ...
kube-system       Active   ...
ingress-nginx     Active   ...
magic-vision      Active   ...
```

**Step 3: Create the Ingress Resource**
 + **Create a file named ingress.yml**

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx-note-ingress
  namespace: magic-vision
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - http:
        paths:
          - path: /nginx
            pathType: Prefix
            backend:
              service:
                name: nginx-service
                port:
                  number: 80

          - path: /app
            pathType: Prefix
            backend:
              service:
                name: notes-app-service
                port:
                  number: 8000

```

**Step 4: Apply the Ingress Configuration**

```
kubectl apply -f ingress.yml
```

**Step 5: Verify the Ingress Resource**
Check whether the Ingress resource has been created successfully:

```
kubectl get ingress -n magic-vision OR kubectl get ing -n magic-vision
```

**Step 6: Verify the Ingress Controller Service**

List the services running in the ingress-nginx namespace:

```
kubectl get svc -n ingress-nginx
```

You should see the ingress-nginx-controller service.

**Step 7: Expose the Ingress Controller**
Forward port 8080 on your local machine to port 80 of the Ingress Controller:

```
sudo -E kubectl port-forward service/ingress-nginx-controller \
  -n ingress-nginx \
  8080:80 \
  --address=0.0.0.0
  ```
  
**Access the Applications:** 
+ After the port-forward is running, you can access your applications using:
  + **NGINX Application:** ```http://<SERVER-IP>:8080/nginx```
  + **Notes Application:** ```http://<SERVER-IP>:8080/app```

**The NGINX Ingress Controller routes incoming requests based on the URL path:**
+ **/nginx →** nginx-service (Port 80)
+ **/app →** notes-app-service (Port 8000)


