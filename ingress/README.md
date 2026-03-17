# NGINX Ingress Controller Guide 🌐

This guide covers the deployment of the NGINX Ingress Controller and demonstrates path-based routing. All configuration files are located in the `./manifests` directory.

---

## 🚀 1. Install NGINX Ingress Controller

Deploy the controller using the official manifest optimized for local/cloud environments:

```bash
kubectl apply -f [https://kind.sigs.k8s.io/examples/ingress/deploy-ingress-nginx.yaml](https://kind.sigs.k8s.io/examples/ingress/deploy-ingress-nginx.yaml)
```
