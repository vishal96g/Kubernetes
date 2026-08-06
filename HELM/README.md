# What is Helm?
+ Helm is a package manager for Kubernetes that simplifies deploying and managing applications using reusable templates called Charts.

**Install Helm on Linux:**
+ Using the Official Installation Script
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-4
chmod 700 get_helm.sh
./get_helm.sh
```

## To create a Helm Chart for an Apache application

+ Verify Helm is installed:
```
helm version
```

+ Create a New Helm Chart:
```
helm create apache-chart
```

+ This creates the following directory structure: 
<img width="311" height="330" alt="image" src="https://github.com/user-attachments/assets/c26103fe-a5cc-4b99-924a-94cbd3ecdf49" />

+ Understand the Files:
<img width="735" height="396" alt="image" src="https://github.com/user-attachments/assets/8e22efd8-7fd7-41ed-8952-fdea36486680" />

