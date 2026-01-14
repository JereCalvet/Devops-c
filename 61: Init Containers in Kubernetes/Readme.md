Day 61: Init Containers in Kubernetes

TLDR: Crear un Deployment con Init Containers

- Create a Deployment named as ic-deploy-xfusion.

- Configure spec as replicas should be 1, labels app should be ic-xfusion, template's metadata lables app should be the same ic-xfusion.

- The initContainers should be named as ic-msg-xfusion, use image debian with latest tag and use command `'/bin/bash', '-c' and 'echo Init Done - Welcome to xFusionCorp Industries > /ic/official'`. The volume mount should be named as ic-volume-xfusion and mount path should be /ic.

- Main container should be named as ic-main-xfusion, use image debian with latest tag and use command `'/bin/bash', '-c' and 'while true; do cat /ic/official; sleep 5; done'`. The volume mount should be named as ic-volume-xfusion and mount path should be /ic.

- Volume to be named as ic-volume-xfusion and it should be an emptyDir type.

Resolución:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: ic-deploy-xfusion
    labels:
        app: ic-xfusion
spec:
    replicas: 1
    selector:
        matchLabels:
            app: ic-xfusion
    template:
        metadata:
            labels:
                app: ic-xfusion
        spec:
            initContainers:
            - name: ic-msg-xfusion
              image: debian:latest
              command: ['/bin/bash', '-c', 'echo Init Done - Welcome to xFusion Industries > /ic/official']
              volumeMounts:
              - name: ic-volume-xfusion
                mountPath: /ic
            containers:
            - name: ic-main-xfusion
              image: debian:latest
              command: ['/bin/bash', '-c', 'while true; do cat /ic/official; sleep 5; done']
              volumeMounts:
              - name: ic-volume-xfusion
                mountPath: /ic
            volumes:
            - name: ic-volume-xfusion
              emptyDir: {}
```
```bash
kubectl apply -f ic-deployment.yaml
kubectl describe deployment ic-deploy-xfusion
```