Day 59: Troubleshoot Deployment issues in Kubernetes

TLDR: Fix

The deployment name is redis-deployment. The pods are not in running state right now, so please look into the issue and fix the same.

Resolución:
```yaml
# redis-deployment.yaml con errores
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: redis
  name: redis-deployment
spec:
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - image: redis:alpin # Error: imagen mal escrita
        imagePullPolicy: IfNotPresent
        name: redis-container
        ports:
        - containerPort: 6379
          protocol: TCP
        resources:
          requests:
            cpu: 300m
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
        volumeMounts:
        - mountPath: /redis-master-data
          name: data
        - mountPath: /redis-master
          name: config
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
      volumes:
      - emptyDir: {}
        name: data
      - configMap:
          defaultMode: 420
          name: redis-cofig # Error: nombre mal escrito
        name: config
```
```bash
kubectl describe pods redis-deployment-5bcd4c7d64-kn9kk 
# Error 1
# Events:
#   Type     Reason       Age                 From               Message
#   ----     ------       ----                ----               -------
#   Normal   Scheduled    15m                 default-scheduler  Successfully assigned default/redis-deployment-6fd9d5fcb-l2n76 to kodekloud-control-plane
#   Warning  FailedMount  103s (x6 over 12m)  kubelet            Unable to attach or mount volumes: unmounted volumes=[config], unattached volumes=[], failed to process volumes=[]: timed out waiting for the condition
#   Warning  FailedMount  42s (x15 over 15m)  kubelet            MountVolume.SetUp failed for volume "config" : configmap "redis-cofig" not found

kubectl get configmaps

kubectl describe pods redis-deployment-5bcd4c7d64-kn9kk 
# Error 2
# Events:
#  Type     Reason     Age                From               Message
#  ----     ------     ----               ----               -------
#  Normal   Scheduled  86s                default-scheduler  Successfully assigned default/redis-deployment-5bcd4c7d64-kn9kk to kodekloud-control-plane
#  Normal   BackOff    17s (x4 over 84s)  kubelet            Back-off pulling image "redis:alpin"
#  Warning  Failed     17s (x4 over 84s)  kubelet            Error: ImagePullBackOff
#  Normal   Pulling    2s (x4 over 85s)   kubelet            Pulling image "redis:alpin"
#  Warning  Failed     2s (x4 over 85s)   kubelet            Failed to pull image "redis:alpin": rpc error: code = NotFound desc = failed to pull and unpack image "docker.io/library/redis:alpin": failed to resolve reference "docker.io/library/redis:alpin": docker.io/library/redis:alpin: not found
#  Warning  Failed     2s (x4 over 85s)   kubelet            Error: ErrImagePull

kubectl edit deployment redis-deployment 
# o
kubectl get deployment redis-deployment -o yaml > redis-deployment-fixed.yaml
vi redis-deployment-fixed.yaml
kubectl apply -f redis-deployment-fixed.yaml
# Corregir los errores en el yaml:
# - Cambiar imagen de "redis:alpin" a "redis:alpine"
# - Cambiar nombre del configmap de "redis-cofig" a "redis-config"
```