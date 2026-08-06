# 1. What is RBAC (Role-Based Access Control) in Kubernetes?
+ RBAC (Role-Based Access Control) is a Kubernetes security mechanism used to control who can access and perform actions on Kubernetes resources.
+ For example:
   + A developer can create Pods but cannot delete Nodes.
   + A tester can only view Deployments.
   +  A cluster administrator has full access to the cluster.

**Why RBAC is Important:**
+ Restrict unauthorized access
+ Improve cluster security
+ Follow the principle of least privilege (users get only the permissions they need)
+ Separate responsibilities between developers, testers, and administrators
+ Protect critical cluster resources     

<img width="1000" height="650" alt="rbac_role" src="https://github.com/user-attachments/assets/45d206f2-3f4a-4ce1-ad9c-0a129189dc0a" />


**1. Role:**
+ A Role grants permissions within a single namespace and role does not give permissions to anyone.
+ Namespace-specific
+ Cannot access resources outside that namespace

**2. ClusterRole:**
+ A ClusterRole grants permissions across the entire cluster.

**3. RoleBinding:**
+ A RoleBinding connects a Role to a User, Group, or ServiceAccount.  

**4. ClusterRoleBinding:**
+ A ClusterRoleBinding connects a ClusterRole to a User, Group, or ServiceAccount.

## 1. RBAC Namespace-Level Access in Kubernetes

**1. Create a YAML File for Role**

```
Yaml file name: role.yml

kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: apache-manager          # This is the Role
  namespace: apche
rules:
- apiGroups: ["","apps","rbac.authorization.k8s.io/v1","batch"] # "" indicates the core API group
  resources: ["pod", "deployment", "service"]
  verbs: ["get", "apply", "create", "delete", "watch", "list", "patch"]

```
+ **Apply it:** ``` kubectl apply -f role.yml```

**2. Create a YAML File for Service Account**

```
Yaml file name: service-account.yml


kind: ServiceAccount
apiVersion: v1
metadata:
  name: apache-user         # This is the username of user
  namespace: apache

```
+ **Apply it:** ``` kubectl apply -f service-account.yml```



**3. Create a YAML File for Role Binding**

```
Yaml file name: role-binding.yml


apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: apache-manager-role-binding
  namespace: apache

subjects:
- kind: User
  name: apache-user                           # Name of the Kubernetes user
  apiGroup: rbac.authorization.k8s.io

- kind: ServiceAccount
  name: apache-manager                        # Name of the ServiceAccount
  namespace: apache

roleRef:
  kind: Role
  name: apache-manager
  apiGroup: rbac.authorization.k8s.io

```
+ **Apply it:** ``` kubectl apply -f role-binding.yml```

**Basic Commands**
```
kubectl auth whoami
kubectl auth can-i get pods
kubectl auth can-i get pods -n namespace
kubectl auth can-i get deployment -n namespace
kubectl auth can-i delete deployment -n namespace
Kubectl get role -n namespace
kubectl auth can-i get pods --as=apache-user -n namespace 
```

## 2. RBAC Cluster-Level Access in Kubernetes

**1. Create a YAML File for ClusterRole**

```
Yaml file name: cluster-role.yml

kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: apache-cluster-manager          # This is the Cluster Role

rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch", "create", "update", "delete"]

- apiGroups: ["apps"]
  resources: ["deployments"]
  verbs: ["get", "list", "watch", "create", "update", "delete"]

```
+ **Apply it:** ``` kubectl apply -f cluster-role.yml```

**2. Create a YAML File for Service Account**
 + Although a ClusterRole is cluster-wide, a ServiceAccount is still created in a namespace.

```
Yaml file name: service-account.yml


kind: ServiceAccount
apiVersion: v1
metadata:
  name: apache-admin         # This is the username of user
  namespace: apache

```
+ **Apply it:** ``` kubectl apply -f service-account.yml```

**3. Create a YAML File for ClusterRoleBinding**

```
Yaml file name: cluster-role-binding.yml


apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: apache-cluster-manager-binding

subjects:
- kind: ServiceAccount
  name: apache-admin
  namespace: apache

roleRef:
  kind: ClusterRole
  name: apache-cluster-manager
  apiGroup: rbac.authorization.k8s.io

```

+ **Apply it:** ``` kubectl apply -f cluster-role-binding.yml```

**Quick Rule to Remember:**
+ **ServiceAccount →** Who (the identity used by Pods/applications to access the Kubernetes API).
+ **Role →** What can be done within one namespace.
+ **RoleBinding →** Connects the ServiceAccount (or User/Group) to a Role within one namespace.
+ **ClusterRole →** What can be done across the entire cluster.
+ **ClusterRoleBinding →** Connects the ServiceAccount (or User/Group) to a ClusterRole across the entire cluster.
