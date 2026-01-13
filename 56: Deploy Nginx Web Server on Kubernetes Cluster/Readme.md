Day 56: Deploy Nginx Web Server on Kubernetes Cluster

TLDR: Crear un deployment de Nginx con 3 réplicas y un servicio NodePort para exponerlo.

- Create a deployment using nginx image with latest tag only and remember to mention the tag i.e nginx:latest. Name it as nginx-deployment. The container should be named as nginx-container, also make sure replica counts are 3.

- Create a NodePort type service named nginx-service. The nodePort should be 30011.

Resolución:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template: 
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx:latest
        name: nginx-container
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  type: NodePort
  ports:
    - port: 80
        targetPort: 80
        nodePort: 30011
```

```bash
kubectl apply -f nginx-deployment.yaml
# Verificar
kubectl get deployments
kubectl get pods
kubectl get services
kubectl describe service nginx-service
curl <Node-IP>:30011
```