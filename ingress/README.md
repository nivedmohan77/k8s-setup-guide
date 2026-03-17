# NGINX Ingress Controller & Path-Based Routing 🌐

This lab demonstrates how to deploy an NGINX Ingress Controller and configure it to route traffic to multiple backend services using a single entry point.

---

## 🏗️ Project Structure

For better maintainability, all Kubernetes resource definitions are stored in the `manifests/` directory:

* **`manifests/app1.yaml`**: Deployment and Service for Application 1.
* **`manifests/app2.yaml`**: Deployment and Service for Application 2.
* **`manifests/ingress.yaml`**: Path-based routing rules for the Ingress Controller.

---

## 🚀 1. Install NGINX Ingress Controller

Before deploying our apps, we must install the Ingress Controller (the "engine" that processes routing rules):

```bash
kubectl apply -f [https://kind.sigs.k8s.io/examples/ingress/deploy-ingress-nginx.yaml](https://kind.sigs.k8s.io/examples/ingress/deploy-ingress-nginx.yaml)
```
