Day 51: Execute Rolling Updates in Kubernetes

Enunciado: Hacer un rolling update

An application currently running on the Kubernetes cluster employs the nginx web server. The Nautilus application development team has introduced some recent changes that need deployment. They've crafted an image nginx:1.19 with the latest updates.

Execute a rolling update for this application, integrating the nginx:1.19 image. The deployment is named nginx-deployment.

Ensure all pods are operational post-update.

Note: The kubectl utility on jump_host is set up to operate with the Kubernetes cluster

Resolución:
```bash
kubectl set image deployment/nginx-deployment nginx-container=nginx:1.19
#sintaxis set image <resource_type/resource_name> <container_name>=<new_image>
kubectl rollout status deployment/nginx-deployment
```