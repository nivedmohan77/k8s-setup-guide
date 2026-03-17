# Kubernetes Bootstrap Labs ☸️

A curated collection of installation guides and configuration patterns for setting up Kubernetes environments. This repository serves as a quick-start reference for local development and bare-metal cluster bootstrapping.

---

## 📂 Repository Structure

Each folder contains a dedicated `README.md` with step-by-step commands and prerequisites.

* **/kind**: Multi-node local clusters using Docker container "nodes".
* **/minikube**: The classic local Kubernetes tool for macOS, Linux, and Windows.
* **/kubeadm**: The standard tool for bootstrapping best-practice, production-ready clusters.
* **/ingress**: Configuration guides for NGINX and HAProxy Ingress Controllers.

---

## 🚀 Getting Started

Choose your preferred installation method based on your resource availability:

| Method | Best For | Complexity |
| :--- | :--- | :--- |
| **Kind** | CI/CD & Local Testing | Low |
| **Minikube** | Learning & Local Dev | Low |
| **Kubeadm** | Production & Hardware | High |

---

## 🛠️ Prerequisites

Before you begin, ensure you have the following CLI tools installed:
* [kubectl](https://kubernetes.io/docs/tasks/tools/)
* [Docker Desktop](https://www.docker.com/products/docker-desktop/) or [Colima](https://github.com/abiosoft/colima)
* [Helm](https://helm.sh/docs/intro/install/) (Optional, for Ingress)

---

## 📖 How to Use This Repo

1.  **Clone the repo:**
    ```bash
    git clone https://github.com/nivedmohan77/k8s-setup-guide.git
    cd k8s-setup-guide
    ```
2.  **Navigate to the tool of your choice:**
    ```bash
    cd kind
    # Follow the instructions in the local README.md
    ```

---

## 🤝 Contributing
Feel free to open a Pull Request if you find a more efficient way to automate these installs or want to add support for new CNI plugins!

---

## 🎖️ Credits & Attribution

* **Original Labs:** Based on the foundational curriculum and installation guides created by **BashOps**.
* **Enhancements:** This repository is an evolved version maintained by **Nived**, featuring optimized configurations, custom scripts, and updated documentation.

---
*Last updated: March 2026*
