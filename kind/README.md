# Kind (Kubernetes in Docker) Setup Guide 🚀

Kind is a tool for running local Kubernetes clusters using Docker container "nodes". It is primarily designed for testing Kubernetes itself, but is often used for local development or CI/CD.

---

## 🐧 Installation (Linux)

Run the following script to install both **Kind** and **kubectl** on your Linux machine:

```bash
#!/bin/bash

# Download Kind binary
[ $(uname -m) = x86_64 ] && curl -Lo ./kind [https://kind.sigs.k8s.io/dl/v0.26.0/kind-linux-amd64](https://kind.sigs.k8s.io/dl/v0.26.0/kind-linux-amd64)
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

# Download kubectl binary
curl -LO "[https://dl.k8s.io/release/$(curl](https://dl.k8s.io/release/$(curl) -L -s [https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl](https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl)"
chmod +x kubectl
sudo mv ./kubectl /usr/local/bin/kubectl

echo "kind & kubectl Successfully Installed!"
```
---
## 🏗️ Multi-node Cluster Configuration

To create a cluster with one control-plane and three worker nodes (including port mapping for Ingress), save the following as kind-config.yaml:

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  image: kindest/node:v1.32.1
- role: worker
  image: kindest/node:v1.32.1
- role: worker
  image: kindest/node:v1.32.1
- role: worker
  image: kindest/node:v1.32.1
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: tcp
```

Create the Cluster

Once the file is saved, run:

```bash
kind create cluster --config kind-config.yaml

```
---

## 🛠️ Common Lifecycle Commands

| Action | Command |
| :--- | :--- |
| **Create** default cluster | `kind create cluster` |
| **Check** active clusters | `kind get clusters` |
| **Delete** a cluster | `kind delete cluster` |
| **Get Nodes** (Status) | `kubectl get nodes -o wide` |

---

