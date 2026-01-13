Day 52: Revert Deployment to Previous Version in Kubernetes

TLDR: Revertir un despliegue a una versión anterior

Resolución:
```bash
kubectl get deployments
# Identificar el nombre del despliegue que se desea revertir
kubectl rollout undo deployment/<deployment_name>
# Revertir el despliegue al estado anterior
kubectl rollout status deployment/<deployment_name>
# Verificar que el despliegue se haya revertido correctamente
```