# 1. Deployment

**What is a Deployment?**
+ A Deployment is a Kubernetes resource used to deploy, manage, and update applications.
+ It manages ReplicaSets.
+ It ensures that the desired number of Pod replicas are always running.
+ It supports rolling updates, allowing application updates with zero or minimal downtime.
+ It supports rollbacks, so you can quickly revert to the previous version if a deployment fails.
+ It allows you to scale applications by increasing or decreasing the number of replicas.
+ It continuously ensures that the actual state matches the desired state.

**How Deployment Works**
+ You create a Deployment.
+ The Deployment creates a ReplicaSet.
+ The ReplicaSet creates the required number of Pods.
+ If a Pod fails, the ReplicaSet creates a new one.
+ During an application update, the Deployment performs a rolling update by replacing Pods gradually.


<img width="1000" height="700" alt="Deploments" src="https://github.com/user-attachments/assets/77876336-66c5-4b74-919a-369265a86203" />


YAML File for Deployment

```
Yaml file name: deployment.yml


kind: Deployment
apiVersion: apps/v1
metadata:
  name: nginx-deployment
  namespace: magic-vision
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      name: nginx-dep-pod 
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80

```

**Basic Commands**

+ **Create Deployment:** kubectl apply -f deployment.yml
+ **Get Deployments:** kubectl get deployments -n namespace
+ **Describe Deployment:** kubectl describe deployment my-deployment -n namespace
+ **Scale Deployment:** kubectl scale deployment my-deployment -n namespace --replicas=5
+ **Update Image:** kubectl set image deployment/my-deployment -n namespace nginx-container=nginx:1.25
+ **Delete Deployment:** kubectl delete deployment my-deployment


# 2. ReplicaSets
**What is a ReplicaSets?**
+ A ReplicaSet is a Kubernetes resource that ensures a specified number of Pod replicas are running at all times.
+ Its main purpose is to provide high availability by maintaining the desired number of Pods.
+ If a Pod fails or is deleted, the ReplicaSet automatically creates a new Pod.
+ If there are more Pods than required, the ReplicaSet terminates the extra Pods.
+ It uses Labels and Selectors to identify and manage Pods.
+ ReplicaSets are usually not created directly in production; they are managed by a Deployment.
+ ReplicaSet supports scaling by increasing or decreasing the number of Pod replicas.


<img width="1000" height="700" alt="Replicasets" src="https://github.com/user-attachments/assets/ae1cf707-f8ef-4104-8799-7ed444442751" />


YAML File for ReplicaSets

```
Yaml file name: replicaset.yml


kind: ReplicaSet
apiVersion: apps/v1
metadata:
  name: nginx-replicasets
  namespace: magic-vision
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      name: nginx-rep-pod 
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80

```

**Basic Commands**
+ **Create ReplicaSet:** kubectl apply -f replicaset.yml
+ **Get ReplicaSets:** kubectl get replicaset OR kubectl get rs
+ **Describe ReplicaSet:** kubectl describe rs my-replicaset
+ **Delete ReplicaSet:** kubectl delete rs my-replicaset


# 3. DaemonSets

**What is a DaemonSets?**
+ A DaemonSet is a Kubernetes workload resource that ensures one Pod runs on every eligible Worker Node in the cluster.
+ As new nodes are added to the cluster, Kubernetes automatically creates the DaemonSet Pod on those nodes. If a node is removed, the corresponding Pod is automatically removed.

<img width="1000" height="700" alt="DaemonSet" src="https://github.com/user-attachments/assets/5019ef37-7081-4416-9a15-e31d2c2e5533" />


YAML File for DaemonSets

```
Yaml file name: daemonset.yml


kind: DaemonSet
apiVersion: apps/v1
metadata:
  name: nginx-daemonset
  namespace: magic-vision
spec:
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      name: nginx-dmn-pod 
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80

```

**Basic Commands**

+ **Create DaemonSet:** kubectl apply -f daemonset.yml
+ **Get DaemonSets:** kubectl get daemonsets
+ **Describe DaemonSet:** kubectl describe daemonset my-daemonset
+ **Check Pods Created by DaemonSet:** kubectl get pods -o wide
+ **Delete DaemonSet:** kubectl delete daemonset my-daemonset


# 4. StatefulSets?
+ A StatefulSet is a Kubernetes workload resource used to manage stateful applications.
+ It provides stable pod identities, persistent storage, and ordered deployment and scaling, making it ideal for applications like databases.
+ Examples:
  + MySQL
  + MongoDB
  + PostgreSQL 

<img width="1100" height="700" alt="Stateless vs Stateful Kubernetes" src="https://github.com/user-attachments/assets/a3c14ccc-b1b4-49e0-9046-a0d2bd27105e" />


YAML File for StatefulSets

```
Yaml file name: statefulset.yml


kind: StatefulSet
apiVersion: apps/v1
metadata:
  name: mysql-statefulset
  namespace: mysql
spec:
  serviceName: mysql-service              #Service Name of Headless Service required.
  replicas: 3
  serviceName: mysql-headless-service
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql:8.0
          ports:
            - containerPort: 3306
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: root
            - name: MYSQL_DATABASE
              value: devops               #create devops database
          volumeMounts:
            - name: mysql-data
              mountPath: /var/lib/mysql

  volumeClaimTemplates:
    - metadata:
        name: mysql-data
      spec:
        accessModes: [ "ReadWriteOnce" ]
        resources:
          requests:
            storage: 1Gi
```
## StatefulSet requires a Headless Service
+ StatefulSet requires a Headless Service. Unlike a Deployment, which usually uses a normal ClusterIP Service for load balancing, a StatefulSet uses a Headless Service (clusterIP: None) to provide a stable network identity to each pod.

**YAML File for Headless Service**

```

kind: Service
apiVersion: v1
metadata:
  name: mysql-service
  namespace: mysql
spec:
  clusterIP: None       #ClusterID should be "None" for Headless Service
  selector:
    app: mysql
  ports:
    - name: mysql
      protocol: TCP
      port: 3306
      targetPort: 3306

```


# 5. Jobs
**What is a Jobs?**
+ A Job is a Kubernetes workload resource that is used to run a task to completion.
+ Restarts the task if the Pod fails before completion.
+ Marks the Job as Completed after successful execution.
+ Can run one or multiple Pods to complete the job or task.
+ Supports parallel execution.

**Jobs are commonly used for**

+ Database backups
+ Database migration scripts
+ Sending emails
+ Report generation
+ Data import/export
+ Batch processing
+ Image or video processing
+ Running test scripts
+ CI/CD tasks

<img width="1000" height="650" alt="job-k8s" src="https://github.com/user-attachments/assets/9bf70727-2c16-4ab9-bfbc-128fc495f27a" />

YAML File for Jobs

```
Yaml file name: job.yml


kind: Job
apiVersion: batch/v1
metadata:
  name: nginx-job
  namespace: magic-vision
spec:
  completions: 1
  parallelism: 1 
  template:
    metadata:
      name: demo-job-pod
      labels:
        app: batch-task 
    spec:
      containers:
      - name: batch-container
        image: busybox:latest 
        command: ["sh",  "-c", "echo Hello Dosto! && sleep 10"]
      restartPolicy: Never
  
```

**Basic Commands**
+ **Create Job from YAML:** kubectl apply -f job.yml
+ **List Jobs:** kubectl get jobs
+ **Describe Job:** kubectl describe job my-job
+ **Delete Job:** kubectl delete job my-job
+ **Check Job Status:** kubectl get jobs -o wide


# 6. CronJobs
**What is a CronJobs?**
+ CronJob is a Kubernetes resource that creates Jobs automatically at scheduled times. It is used for tasks that need to run repeatedly based on a schedule, similar to the Linux cron utility.
+ A CronJob does not execute the task itself. Instead, it creates a Job, and the Job creates one or more Pods to perform the task.

<img width="1000" height="700" alt="cron-job" src="https://github.com/user-attachments/assets/350f43a8-bb2a-42b6-a3d9-2bc5074a7a51" />



YAML File for CronJobs

```
Yaml file name: cronjob.yml


apiVersion: batch/v1
kind: CronJob

metadata:
  name: minute-backup
  namespace: magic-vision

spec:
  schedule: "* * * * *"

  jobTemplate:
    spec:
      template:
        metadata:
          labels:
            app: minute-backup

        spec:
          restartPolicy: OnFailure

          containers:
            - name: minute-backup
              image: busybox:latest

              command:
                - sh
                - -c
                - |
                  echo "Backup Started"
                  mkdir -p /backups
                  cp -r /demo-data/* /backups/
                  echo "Backup Completed"

              volumeMounts:
                - name: data-volume
                  mountPath: /demo-data

                - name: backup-volume
                  mountPath: /backups

          volumes:
            - name: data-volume
              hostPath:
                path: /demo-data
                type: DirectoryOrCreate

            - name: backup-volume
              hostPath:
                path: /backups
                type: DirectoryOrCreate       

```

**Basic Commands**

+ **Create CronJob from YAML:** kubectl apply -f cronjob.yml
+ **List CronJobs:** kubectl get cronjobs
+ **Describe CronJob:** kubectl describe cronjob my-cronjob
+ **Delete CronJob:** kubectl delete cronjob my-cronjob

