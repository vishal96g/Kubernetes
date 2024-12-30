# KIND Cluster Setup

# 1. Installation of KIND and kubectl

Install the kind and kubectl with the script:

```bash

#!/bin/bash

[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
chmod +x ./kind
sudo cp ./kind /usr/local/bin/kind

VERSION="v1.30.0"
URL="https://dl.k8s.io/release/${VERSION}/bin/linux/amd64/kubectl"
INSTALL_DIR="/usr/local/bin"

curl -LO "$URL"
chmod +x kubectl
sudo mv kubectl $INSTALL_DIR/
kubectl version --client

rm -f kubectl
rm -rf kind

echo "kind & kubectl installation complete."
```
 Installation Screenshot:
 
![image](https://github.com/user-attachments/assets/5a625ce7-4e92-4283-ac36-56d2ced86929)

Verify KIND & kubectl:

```bash
kind version
kubectl version
```

# 2. Setup the KIND Cluster

Create a kind-cluster-config.yml file:

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4

nodes:
- role: control-plane
  image: kindest/node:v1.31.2
- role: worker
  image: kindest/node:v1.31.2
- role: worker
  image: kindest/node:v1.31.2
```
Create the cluster using the configuration file:

```bash
kind create cluster --config kind-cluster-config.yaml --name my-kind-cluster
OR
kind create cluster --config=kind-cluster-config.yaml --name=my-kind-cluster
```

KIND cluster Installation Screenshot:

![image](https://github.com/user-attachments/assets/5c55eb4d-a367-4e97-b64b-642d95824666)

Verity the cluster:

```bash
kubectl cluster-info
kubectl get nodes
kubectl get namespace OR kubectl get ns
```

# 3. Deleting the Cluster

Delete the KIND cluster:

```bash
kind delete cluster --name my-kind-cluster
OR
kind delete cluster --name=my-kind-cluster
```





