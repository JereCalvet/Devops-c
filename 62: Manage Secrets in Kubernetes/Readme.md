Day 62: Manage Secrets in Kubernetes

Create a secret and a pod to consume that secret

- We already have a secret key file official.txt under /opt location on jump host. Create a generic secret named official, it should contain the password/license-number present in official.txt file.

- Also create a pod named secret-nautilus.

- Configure pod's spec as container name should be secret-container-nautilus, image should be fedora with latest tag (remember to mention the tag with image). Use sleep command for container so that it remains in running state. Consume the created secret and mount it under /opt/cluster within the container.

- To verify you can exec into the container secret-container-nautilus, to check the secret key under the mounted path /opt/cluster. Before hitting the Check button please make sure pod/pods are in running state, also validation can take some time to complete so keep patience.

Resolución:
```yaml
apiVersion: v1
kind: Secret
metadata:
    name: official
type: Opaque
data:
    password: NWVjdXIz           # echo -n 'secure3' | base64
    license-number: NWVjdXIz     # echo -n 'secure3' | base64
---
apiVersion: v1
kind: Pod
metadata:
    name: secret-nautilus
spec:
    containers:
    - name: secret-container-nautilus
      image: fedora:latest
      command: ["sh", "-c", "sleep 3h"]
      volumeMounts:
      - name: secret-nautilus
        mountPath: /opt/cluster
    volumes:    
    - name: secret-nautilus
        secret:
            secretName: official
```
```bash
kubectl apply -f secret-and-pod.yaml
kubectl describe pod secret-nautilus
kubectl exec -it secret-container-nautilus -- cat /opt/cluster/password
kubectl exec -it secret-container-nautilus -- cat /opt/cluster/license-number
```

### v2, usando secrets
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: iron-db-secret
  namespace: iron-namespace-devops
type: Opaque
data:
  MYSQL_ROOT_PASSWORD: Q29tcGxleFJvb3RQYXNzMTIzIQ==
  MYSQL_PASSWORD: Q29tcGxleFVzZXJQYXNzMTIzIQ==
