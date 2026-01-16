Day 65: Deploy Redis Deployment on Kubernetes

Create a redis deployment with following parameters:
- Create a config map called my-redis-config having maxmemory 2mb in redis-config.

- Name of the deployment should be redis-deployment, it should use redis:alpine image and container name should be redis-container. Also make sure it has only 1 replica.

- The container should request for 1 CPU.

- Mount 2 volumes:

    a. An Empty directory volume called data at path /redis-master-data.

    b. A configmap volume called redis-config at path /redis-master.

    c. The container should expose the port 6379.

Resolución:
```yaml
# configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
    name: my-redis-config
data:
    redis-config: |
        maxmemory 2mb
---
# redis-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: redis
  name: redis-deployment        
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      name: redis-pod
      labels:
        app: redis
    spec:
      containers:
      - image: redis:alpine
        name: redis-container
        ports:
        - containerPort: 6379
          protocol: TCP
        resources:
          requests:
            cpu: "1"
        volumeMounts:
        - mountPath: /redis-master-data
          name: data
        - mountPath: /redis-master
          name: config
      volumes:
      - emptyDir: {}    
        name: data
      - configMap:
          defaultMode: 420
          name: my-redis-config
        name: config
```
```bash
kubectl apply -f configmap.yaml
kubectl apply -f redis-deployment.yaml
kubectl get deployments
kubectl get pods
kubectl describe pods/<pod-name>
kubectl logs pods/<pod-name>
kubectl exec -it <pod-name> -- redis-cli
```