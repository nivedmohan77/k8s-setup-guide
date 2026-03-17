# Kubeadm Cluster Installation Guide ☸️

This guide provides step-by-step instructions for bootstrapping a best-practice Kubernetes cluster on Ubuntu using `kubeadm`.

---

## 📋 Prerequisites & Infrastructure

Before beginning the installation, ensure your environment meets the following minimum requirements to ensure a stable cluster.

### 💻 Hardware & OS
* **Operating System:** Ubuntu (Xenial 16.04 or later).
* **Compute:** Minimum **2 vCPUs** and **2GB RAM**.
  * *Examples:* AWS `t2.medium`, Azure `Standard_B2s`, or a local Virtual Machine.
* **Privileges:** Full `sudo` access is required on all nodes.
* **Storage:** At least 20GB of available disk space.

### 🌐 Networking & Security
The following ports must be open in your firewall or Network Security Group (NSG):

| Port | Protocol | Purpose |
| :--- | :--- | :--- |
| **6443** | TCP | Kubernetes API Server (Required for Worker nodes to join) |
| **22** | TCP | SSH Management Access |
| **10250** | TCP | Kubelet API (Internal cluster communication) |

> [!IMPORTANT]
> **Swap Memory:** Kubernetes requires Swap to be disabled. The installation script provided in this repo handles this automatically.

---

> [!TIP]
> **Cloud Provider Note (AWS/Azure/GCP):**
> If you are using AWS, ensure all nodes are assigned to the same **Security Group**. You must manually edit the "Inbound Rules" to allow traffic on port **6443** from within the Security Group CIDR or the specific Private IPs of your worker nodes.

---
---

## 🛠️ Step 1: Preparation (Execute on ALL Nodes)

Run these commands on both the **Master** and **Worker** nodes to prepare the environment.

### Disable Swap

Kubernetes requires swap to be disabled to function correctly.

```bash
sudo swapoff -a
# To make it permanent, comment out any swap entry in /etc/fstab
```
---

### Load Kernel Modules & Sysctl

```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter

# Set networking parameters
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

sudo sysctl --system
```
---

### Install Containerd (CRI)

```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL [https://download.docker.com/linux/ubuntu/gpg](https://download.docker.com/linux/ubuntu/gpg) -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] [https://download.docker.com/linux/ubuntu](https://download.docker.com/linux/ubuntu) $(. /etc/os-release && echo \"$VERSION_CODENAME\") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install -y containerd.io

# Configure Containerd to use SystemdCgroup
containerd config default | sed -e 's/SystemdCgroup = false/SystemdCgroup = true/' -e 's/sandbox_image = "registry.k8s.io\/pause:3.6"/sandbox_image = "registry.k8s.io\/pause:3.9"/' | sudo tee /etc/containerd/config.toml

sudo systemctl restart containerd

```
---
### Install Kubelet, Kubeadm & Kubectl

```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl gpg

curl -fsSL [https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key](https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key) | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] [https://pkgs.k8s.io/core:/stable:/v1.29/deb/](https://pkgs.k8s.io/core:/stable:/v1.29/deb/) /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl

```
---

## 👑 Step 2: Master Node Initialization (Execute ONLY on Master)

### Initialize Cluster

```bash
sudo kubeadm init

```
---

### Configure Kubeconfig

To start using the cluster as a regular user, run:

```bash

mkdir -p "$HOME"/.kube
sudo cp -i /etc/kubernetes/admin.conf "$HOME"/.kube/config
sudo chown "$(id -u)":"$(id -g)" "$HOME"/.kube/config
```
---

### Install CNI Plugin (Calico)

```bash
kubectl apply -f [https://raw.githubusercontent.com/projectcalico/calico/v3.26.0/manifests/calico.yaml](https://raw.githubusercontent.com/projectcalico/calico/v3.26.0/manifests/calico.yaml)

```
---

### Get Join Command -- Can be used in case when we lost or have expired the join command.

Generate the token needed for worker nodes:

```bash
kubeadm token create --print-join-command

```
---

## 👷 Step 3: Worker Node Setup (Execute ONLY on Workers)

Perform pre-flight checks and join the cluster using the command generated in the previous step.

```bash
sudo kubeadm reset pre-flight checks

# Replace the placeholders with your actual token and hash
sudo kubeadm join <master-private-ip>:6443 --token <token> \
--discovery-token-ca-cert-hash sha256:<hash> \
--cri-socket "unix:///run/containerd/containerd.sock" --v=5

```
---

## ✅ Step 4: Verification (On Master)

Check the status of your nodes:

```bash
kubectl get nodes

```
---
