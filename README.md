# What is Kubernetes?

1. Kubernetes (also called K8s) is an open-source container orchestration platform.
2. It is used to automate the deployment, Instead of managing containers manually, Kubernetes automatically handled scheduling, scaling, self-healing, and load balancing.

## Key Features:
1. Automated Deployment: Automated Deployment: Deploys applications across cluster nodes.
2. Auto Scaling: Increases or decreases pods based on CPU or memory usage using the Horizontal Pod Autoscaler (HPA).
3. Self-Healing: Restarts failed containers, replaces unhealthy pods, and reschedules workloads if a node fails.
5. Load Balancing: Distributes traffic across multiple pods using Services.
5. Rolling Updates & Rollbacks: Updates applications without downtime and allows quick rollback if an issue occurs.
6. Service Discovery: Pods communicate with each other using Kubernetes Services and DNS.

## Important Kubernetes Objects
1. Pod: Smallest deployable unit containing one or more containers.
2. Deployment: Manages pod creation, updates, and scaling.
3. ReplicaSet: Ensures the desired number of pod replicas are running.
4. Service: Exposes applications internally or externally.
5. ConfigMap: Stores non-sensitive configuration.
6. Secret: Stores sensitive data like passwords and API keys.
7. Ingress: Routes external HTTP/HTTPS traffic to services.
8. Namespace: Logically separates resources within the cluster.


# 𝗞𝘂𝗯𝗲𝗿𝗻𝗲𝘁𝗲𝘀 𝗔𝗿𝗰𝗵𝗶𝘁𝗲𝗰𝘁𝘂𝗿𝗲

![kubernetes](https://github.com/user-attachments/assets/1968bb04-9cff-42b0-aad5-91905069cdb1)

𝟭. 𝗔𝗣𝗜 𝗦𝗲𝗿𝘃𝗲𝗿: Acts like a "Team Lead" by managing all components of the worker nodes.

𝟮. 𝗦𝗰𝗵𝗲𝗱𝘂𝗹𝗲𝗿: Acts Like an "HR Manager," the scheduler places containers on nodes if one is killed or needs rescheduling.

𝟯. 𝗲𝘁𝗰𝗱: Acts Like an "database" of Kubernetes, keeping records of all cluster states, such as created or deleted resources.

𝟰. 𝗖𝗼𝗻𝘁𝗿𝗼𝗹𝗹𝗲𝗿 𝗠𝗮𝗻𝗮𝗴𝗲𝗿: Acts Like a "Project Manager," it ensures everything is running smoothly in both worker and master nodes, using the API Server.

𝟱. 𝗞𝘂𝗯𝗲𝗹𝗲𝘁: Ensures that containers in the worker nodes are functioning correctly. If any crashes, Kubelet reports to the API Server.

𝟲. 𝗦𝗲𝗿𝘃𝗶𝗰𝗲 𝗣𝗿𝗼𝘅𝘆: Exposes running applications inside the containers/pods to the end users.

𝟳. 𝗖𝗡𝗜 (𝗖𝗼𝗻𝘁𝗮𝗶𝗻𝗲𝗿 𝗡𝗲𝘁𝘄𝗼𝗿𝗸 𝗜𝗻𝘁𝗲𝗿𝗳𝗮𝗰𝗲): Responsible for establishing the network connection between master and worker nodes.

𝟴. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹: Act like a "CEO" of the Kubernetes cluster, controlling everything inside the containers. It is used to interact with the Kubernetes cluster.

