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


