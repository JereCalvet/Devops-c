Day 64: Fix Python App Deployed on Kubernetes Cluster

Fix:
- The deployment name is python-deployment-xfusion, its using poroko/flask-demo-appimage. The deployment and service of this app is already deployed.
- nodePort should be 32345 and targetPort should be python flask app's default port.

```bash
kubectl get deployments python-deployment-xfusion -o yaml
kubectl edit deployment python-deployment-xfusion
# arreglar image a poroko/flask-demo-app a poroko/flask-demo-app:latest

kubectl get services python-service-xfusion -o yaml
kubectl edit service python-service-xfusion
# arreglar targetPort a 5000
```