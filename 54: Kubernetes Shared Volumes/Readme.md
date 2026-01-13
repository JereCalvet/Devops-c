Day 54: Kubernetes Shared Volumes

Enunciado: Crear un Pod con Volumenes Compartidos

- Create a pod named `volume-share-xfusion`.

- For the first container, use image ubuntu with latest tag only and remember to mention the tag i.e ubuntu:latest, container should be named as volume-container-xfusion-1, and run a sleep command for it so that it remains in running state. Volume volume-share should be mounted at path /tmp/ecommerce.

- For the second container, use image ubuntu with the latest tag only and remember to mention the tag i.e ubuntu:latest, container should be named as volume-container-xfusion-2, and again run a sleep command for it so that it remains in running state. Volume volume-share should be mounted at path /tmp/apps.

- Volume name should be volume-share of type emptyDir.

- After creating the pod, exec into the first container i.e volume-container-xfusion-1, and just for testing create a file ecommerce.txt with any content under the mounted path of first container i.e /tmp/ecommerce.

- The file ecommerce.txt should be present under the mounted path /tmp/apps on the second container volume-container-xfusion-2 as well, since they are using a shared volume.

Resolución:

```yaml 
# pod-volume-share.yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-xfusion    
spec:
  containers:
  - image: ubuntu:latest
    name: volume-container-xfusion-1
    command: ["sleep"]
    args: ["3h"]
    volumeMounts:
    - mountPath: /tmp/ecommerce
      name: volume-share
  
  - image: ubuntu:latest
    name: volume-container-xfusion-2
    command: ["sleep"]
    args: ["3h"]
    volumeMounts:
    - mountPath: /tmp/apps
      name: volume-share

  volumes:
  - name: volume-share
    emptyDir: {}
```

```bash
kubectl apply -f pod-volume-share.yaml
kubectl get pods
kubectl describe pods
kubectl exec -it volume-share-xfusion -c volume-container-xfusion-1 -- bash
# Dentro del contenedor 1
touch /tmp/ecommerce/ecommerce.txt
echo "Archivo de prueba" > /tmp/ecommerce/ecommerce.txt
exit
kubectl exec -it volume-share-xfusion -c volume-container-xfusion-2 -- bash
# Dentro del contenedor 2
vi /tmp/apps/ecommerce.txt
exit
```