Day 66: Deploy MySQL on Kubernetes

A new MySQL server needs to be deployed on Kubernetes cluster. The Nautilus DevOps team was working on to gather the requirements. Recently they were able to finalize the requirements and shared them with the team members to start working on it. Below you can find the details:

1.) Create a PersistentVolume mysql-pv, its capacity should be 250Mi, set other parameters as per your preference.

2.) Create a PersistentVolumeClaim to request this PersistentVolume storage. Name it as mysql-pv-claim and request a 250Mi of storage. Set other parameters as per your preference.

3.) Create a deployment named mysql-deployment, use any mysql image as per your preference. Mount the PersistentVolume at mount path /var/lib/mysql.

4.) Create a NodePort type service named mysql and set nodePort to 30007.

5.) Create a secret named mysql-root-pass having a key pair value, where key is password and its value is YUIidhb667, create another secret named mysql-user-pass having some key pair values, where frist key is username and its value is kodekloud_rin, second key is password and value is ksH85UJjhb, create one more secret named mysql-db-url, key name is database and value is kodekloud_db9

6.) Define some Environment variables within the container:

a) name: MYSQL_ROOT_PASSWORD, should pick value from secretKeyRef name: mysql-root-pass and key: password

b) name: MYSQL_DATABASE, should pick value from secretKeyRef name: mysql-db-url and key: database

c) name: MYSQL_USER, should pick value from secretKeyRef name: mysql-user-pass key key: username

d) name: MYSQL_PASSWORD, should pick value from secretKeyRef name: mysql-user-pass and key: password

Resolución:
```yaml
# pv.yaml
apiVersion: v1
kind: PersistentVolume
metadata:
    name: mysql-pv
spec:
    capacity:
      storage: 250Mi
    accessModes:
      - ReadWriteOnce
    volumeMode: Filesystem
    hostPath:
      path: /mnt/data/mysql
---
# pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: mysql-pv-claim
spec:
    accessModes:
      - ReadWriteOnce
    resources:
      requests:
        storage: 250Mi
---
# sc.yaml
apiVersion: v1
kind: Secret
metadata:
    name: mysql-root-pass
type: Opaque
data:
  password: YjY2Nwo= #echo YUIidhb667 | base64
---
apiVersion: v1
kind: Secret
metadata:
    name: mysql-user-pass
type: Opaque            
data:
  username: a29kZWtsb3VkX3Jpbgo= #echo kodekloud_rin | base64
  password: a3NIMjg1VUpqaGIK #echo ksH85UJjhb | base64
---
apiVersion: v1
kind: Secret
metadata:
    name: mysql-db-url
type: Opaque
data:
  database: a29kZWtsb3VkX2RiOQo= #echo kodekloud_db9 | base64
---
# dp.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql-container
        image: mysql:latest
        ports:
        - containerPort: 3306
        volumeMounts:
        - name: mysql-data-storage
          mountPath: /var/lib/mysql
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-root-pass
              key: password
        - name: MYSQL_DATABASE
          value: mydb
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: username
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: password
      volumes:
      - name: mysql-data-storage
        persistentVolumeClaim:
          claimName: mysql-pv-claim
---
# svc.yaml
apiVersion: v1
kind: Service
metadata:
    name: mysql-service
spec:
    type: NodePort
    selector:
        app: mysql
    ports:
      - protocol: TCP
        port: 3306
        targetPort: 3306
        nodePort: 30007
```
```bash
kubectl apply -f pv.yaml
kubectl apply -f pvc.yaml
kubectl apply -f sc.yaml
kubectl apply -f dp.yaml
kubectl apply -f svc.yaml
kubectl describe pv/mysql-pv
kubectl describe pvc/mysql-pv-claim
kubectl get secrets
kubectl describe services/mysql-service
kubectl describe deployment/mysql-deployment
kubectl describe pods
```
