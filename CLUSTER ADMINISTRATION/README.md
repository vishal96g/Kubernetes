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



**YAML File for Role**

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

**YAML File for Service Account**

```
Yaml file name: service-account.yml


kind: ServiceAccount
apiVersion: v1
metadata:
  name: apache-user         # This is the username of user
  namespace: apache

```

**YAML File for Role Binding**

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

