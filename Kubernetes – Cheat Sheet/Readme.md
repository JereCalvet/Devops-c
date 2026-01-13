# Kubernetes – Cheat Sheet

## Conceptos base

### 📦 Pod
El objeto más pequeño de Kubernetes. Un Pod ejecuta uno o más **containers** que comparten red y almacenamiento.  
- Kubernetes **no gestiona containers sueltos**, siempre los hace correr dentro de Pods.

**Comandos:**
```bash
kubectl apply -f pod.yml
kubectl get pods
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl exec -it <pod-name> -- /bin/sh
kubectl delete pod <pod-name>
```
### 🏷️ Labels
Metadatos clave/valor para identificar y agrupar recursos (pods, services, deployments).  
- Se usan para selección, filtrado y conexión entre recursos.

**Comandos:**
```bash
kubectl get pods --show-labels
kubectl get pods -l app=httpd_app
kubectl label pod pod-httpd env=dev
```
### 🚀 Deployment
Administra Pods de forma declarativa. Permite escalar (replicas), actualizaciones, alta disponibilidad y revertir aplicaciones sin tiempo de inactividad.  
- En producción siempre se usa Deployment, no Pods sueltos.

**Comandos:**
```bash
kubectl apply -f deployment.yml
kubectl get deployments
kubectl scale deployment <name> --replicas=3
kubectl rollout status deployment <name>
kubectl rollout undo deployment <name>
kubectl delete deployment <name>
```
### 🧰 ReplicaSet
Se encarga de mantener la cantidad correcta de Pods en ejecución.  
- Normalmente no se crea manualmente, lo maneja el Deployment.

**Comandos:**
```bash
kubectl get rs
kubectl describe rs <name>
```
### 🌐 Service
Expone Pods a la red y mantiene una IP estable.  
Conecta clientes con Pods usando labels.

Tipos comunes:
- ClusterIP (default): Acceso interno en el cluster.
- NodePort: Expone el servicio en un puerto fijo de cada nodo.
- LoadBalancer: Proporciona una IP externa (en nubes públicas).

**Comandos:**
```bash
kubectl apply -f service.yml
kubectl get services
kubectl describe service <name>
kubectl delete service <name>
```
### 📂 Namespace
Permite dividir el cluster en entornos lógicos (dev, qa, prod).  
Evita conflictos de nombres y mejora organización.  

**Comandos:**
```bash
kubectl create namespace <name>
kubectl get namespaces
kubectl config set-context --current --namespace=<name>
kubectl delete namespace <name>
kubectl get pods -n kube-system
kubectl apply -f pod.yml -n dev
```
### 📄 ConfigMap
Almacena configuración no sensible (variables, archivos).  
Permite separar configuración del código.  

**Comandos:**
```bash
kubectl create configmap <name> --from-literal=key=value
kubectl create configmap app-config --from-file=config.env
kubectl create configmap <name> --from-file=path/to/file
kubectl get configmaps
kubectl describe configmap <name>
kubectl delete configmap <name>
```
### 🔐 Secret
Almacena información sensible (passwords, tokens, claves).  
Similar a ConfigMap pero con datos codificados.  

**Comandos:**
```bash
kubectl create secret generic <name> --from-literal=key=value
kubectl create secret generic db-secret --from-file=db-password.txt
kubectl get secrets
kubectl describe secret <name>
kubectl create secret generic db-secret \
  --from-literal=user=admin \
  --from-literal=password=1234
kubectl delete secret <name>
```
### 💾 Volumes / PVC
Permiten almacenamiento persistente para Pods.  
Los datos no se pierden cuando el Pod muere.  

**Comandos:**
```bash
kubectl apply -f pvc.yml
kubectl get pvc
kubectl describe pvc <name>
kubectl delete pvc <name>
```
### 🔍 Debugging 
Estados comunes de error:  
- CrashLoopBackOff: El contenedor falla repetidamente al iniciar.
- ImagePullBackOff: No se puede descargar la imagen del contenedor.
- Pending: No hay nodos disponibles para programar el Pod.

**Comandos:**
```bash
kubectl get pods
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl exec -it <pod-name> -- /bin/sh
kubectl top pod <pod-name>
kubectl describe pod <pod>
kubectl get events --sort-by=.metadata.creationTimestamp
```
