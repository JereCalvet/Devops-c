Day 60: Persistent Volumes in Kubernetes

The Nautilus DevOps team is working on a Kubernetes template to deploy a web application on the cluster. There are some requirements to create/use persistent volumes to store the application code, and the template needs to be designed accordingly. Please find more details below:

- Create a PersistentVolume named as pv-nautilus. Configure the spec as storage class should be manual, set capacity to 3Gi, set access mode to ReadWriteOnce, volume type should be hostPath and set path to /mnt/itadmin (this directory is already created, you might not be able to access it directly, so you need not to worry about it).

- Create a PersistentVolumeClaim named as pvc-nautilus. Configure the spec as storage class should be manual, request 3Gi of the storage, set access mode to ReadWriteOnce.

- Create a pod named as pod-nautilus, mount the persistent volume you created with claim name pvc-nautilus at document root of the web server, the container within the pod should be named as container-nautilus using image nginx with latest tag only (remember to mention the tag i.e nginx:latest).

- Create a node port type service named web-nautilus using node port 30008 to expose the web server running within the pod.

Resolución:
```yaml
# persistent-volume.yaml
apiVersion: v1
kind: PersistentVolume
metadata:
    name: pv-nautilus
spec:
    storageClassName: manual
    capacity:
      storage: 3Gi
    accessModes:
      - ReadWriteOnce
    volumeMode: Filesystem
    hostPath:
      path: /mnt/itadmin
---
# persistent-volume-claim.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: pvc-nautilus
spec:
    storageClassName: manual
    accessModes:
      - ReadWriteOnce
    resources:
      requests:
        storage: 3Gi
---
# pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-nautilus
  labels:
    app: pod-nautilus
spec:
  containers:
    - name: container-nautilus
        image: nginx:latest
        volumeMounts:
            - mountPath: /usr/share/nginx/html
            name: nautilus-storage
    volumes:
        - name: nautilus-storage
          persistentVolumeClaim:
            claimName: pvc-nautilus
---
# service.yaml
apiVersion: v1
kind: Service
metadata:
    name: web-nautilus
spec:
    type: NodePort
    selector:
        app: pod-nautilus
    ports:
        - protocol: TCP
            port: 80
            targetPort: 80
            nodePort: 30008
```
```bash
kubectl apply -f persistent-volume.yaml
kubectl apply -f persistent-volume-claim.yaml
kubectl apply -f pod.yaml
kubectl apply -f service.yaml
```