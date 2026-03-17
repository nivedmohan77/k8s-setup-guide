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
## 📦 2. Deploy Backend Applications

Apply the manifests for both sample applications. These use the ` http-echo ` image to provide a visual confirmation of successful routing:

```bash
kubectl apply -f manifests/app1.yaml
kubectl apply -f manifests/app2.yaml

```
---

## 🛣️ 3. Configure Ingress Routing

Now, we apply the Ingress rule which directs traffic to either ` app1-svc` or ` app2-svc ` based on the URL path (` /app1 ` vs `/app2 `).

```bash
kubectl apply -f manifests/ingress.yaml

```
---

## ✅ 4. Verification & Testing

Get the Ingress Status
Verify that the Ingress resource has been created and assigned an IP/Address:

```bash
kubectl get ingress

```
---

### Test the Routing
Verify that the traffic is being split correctly across your services using `curl`. 



| Resource | Command | Expected Output |
| :--- | :--- | :--- |
| **App 1** | `curl http://localhost/app1` | `"Hello from App 1"` |
| **App 2** | `curl http://localhost/app2` | `"Hello from App 2"` |

---
---

## 🔍 Troubleshooting & Logs

If the routing is not working as expected, use these diagnostic commands to debug the ingress controller, service connectivity, and routing rules.

### 1. Check Controller Logs
View the real-time logs of the NGINX Ingress Controller to identify configuration errors or connection timeouts:
```bash
kubectl logs -n ingress-nginx deploy/ingress-nginx-controller
```
---

### 2. Verify Service Endpoints
Ensure that your services have healthy pods successfully attached to them. If the **ENDPOINTS** column in the output is empty or listed as `<none>`, your pods may be failing to start or the labels may not match.

```bash
kubectl get endpoints app1-svc app2-svc
```
---
### 📋 3. Describe Ingress Resource
Check the detailed configuration and event history of the Ingress rule. This is useful for identifying "backend translation issues" or verifying that the paths are correctly mapped to your services.



```bash
kubectl describe ingress demo-ingress

```
---

