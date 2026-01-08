Day 49: Deploy Applications with Kubernetes Deployments

TLDR: Create a Deployment named nginx using the nginx:latest image.

Resolución:
```bash
kubectl create deployment nginx --image=nginx:latest
kubectl get deployments
```