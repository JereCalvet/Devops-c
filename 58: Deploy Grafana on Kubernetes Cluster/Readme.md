Day 58: Deploy Grafana on Kubernetes Cluster

TLDR: Create a deployment for Grafana with a NodePort service to expose it.

- Create a deployment named grafana-deployment-xfusion using any grafana image for Grafana app. Set other parameters as per your choice.

- Create NodePort type service with nodePort 32000 to expose the app.

You need not to make any configuration changes inside the Grafana app once deployed, just make sure you are able to access the Grafana login page.

Resolución:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana-deployment-xfusion
  labels:
    app: grafana
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template: 
    metadata:
      labels:
        app: grafana
    spec:
      containers:
      - image: grafana/grafana:latest
        name: grafana-container
---
apiVersion: v1
kind: Service
metadata:
  name: grafana-service
spec:
  selector:
    app: grafana
  type: NodePort
  ports:
    - port: 3000
      targetPort: 3000
      nodePort: 32000
``` 

```bash
kubectl apply -f grafana-deployment.yaml
# Verificar
kubectl get deployments
kubectl get pods
kubectl get services
kubectl describe service grafana-service
```