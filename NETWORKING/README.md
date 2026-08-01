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

