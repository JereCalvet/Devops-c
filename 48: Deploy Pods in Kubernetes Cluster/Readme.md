Day 48: Deploy Pods in Kubernetes Cluster

Enunciado:
The Nautilus DevOps team is diving into Kubernetes for application management. One team member has a task to create a pod according to the details below:

    Create a pod named pod-httpd using the httpd image with the latest tag. Ensure to specify the tag as httpd:latest.

    Set the app label to httpd_app, and name the container as httpd-container.

Note: The kubectl utility on jump_host is configured to operate with the Kubernetes cluster.

Resolución:
```bash
touch httpd-pod.yml
vi httpd-pod.yml
# Contenido
# apiVersion: v1
# kind: Pod
# metadata:
#   labels:
#     - app: httpd_app
#   name: pod-httpd
# spec:
#   containers:
#   - name: httpd-container
#     image: httpd:latest
# Guardar y salir
kubectl apply -f httpd-pod.yml
kubectl get pods
```
