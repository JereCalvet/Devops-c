Day 67: Deploy Guest Book App on Kubernetes

TLDR: Deploy app master-slave.

- BACK-END TIER

    Create a deployment named redis-master for Redis master.

    a.) Replicas count should be 1.

    b.) Container name should be master-redis-devops and it should use image redis.

    c.) Request resources as CPU should be 100m and Memory should be 100Mi.

    d.) Container port should be redis default port i.e 6379.

    Create a service named redis-master for Redis master. Port and targetPort should be Redis default port i.e 6379.

    Create another deployment named redis-slave for Redis slave.

    a.) Replicas count should be 2.

    b.) Container name should be slave-redis-devops and it should use gcr.io/google_samples/gb-redisslave:v3 image.

    c.) Requests resources as CPU should be 100m and Memory should be 100Mi.

    d.) Define an environment variable named GET_HOSTS_FROM and its value should be dns.

    e.) Container port should be Redis default port i.e 6379.

    Create another service named redis-slave. It should use Redis default port i.e 6379.

- FRONT END TIER

    Create a deployment named frontend.

    a.) Replicas count should be 3.

    b.) Container name should be php-redis-devops and it should use gcr.io/google-samples/gb-frontend@sha256:a908df8486ff66f2c4daa0d3d8a2fa09846a1fc8efd65649c0109695c7c5cbff image.

    c.) Request resources as CPU should be 100m and Memory should be 100Mi.

    d.) Define an environment variable named as GET_HOSTS_FROM and its value should be dns.

    e.) Container port should be 80.

    Create a service named frontend. Its type should be NodePort, port should be 80 and its nodePort should be 30009.

Resolución:
```yaml
---
# Redis Master Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-master
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
      role: master
      tier: backend
  template:
    metadata:
      labels:
        app: redis
        role: master
        tier: backend
    spec:
      containers:
      - name: master-redis-devops
        image: redis
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        ports:
        - containerPort: 6379

---
# Redis Master Service
apiVersion: v1
kind: Service
metadata:
  name: redis-master
spec:
  selector:
    app: redis
    role: master
    tier: backend
  ports:
  - port: 6379
    targetPort: 6379

---
# Redis Slave Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-slave
spec:
  replicas: 2
  selector:
    matchLabels:
      app: redis
      role: slave
      tier: backend
  template:
    metadata:
      labels:
        app: redis
        role: slave
        tier: backend
    spec:
      containers:
      - name: slave-redis-devops
        image: gcr.io/google_samples/gb-redisslave:v3
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        env:
        - name: GET_HOSTS_FROM
          value: dns
        ports:
        - containerPort: 6379

---
# Redis Slave Service
apiVersion: v1
kind: Service
metadata:
  name: redis-slave
spec:
  selector:
    app: redis
    role: slave
    tier: backend
  ports:
  - port: 6379
    targetPort: 6379

---
# Frontend Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: guestbook
      tier: frontend
  template:
    metadata:
      labels:
        app: guestbook
        tier: frontend
    spec:
      containers:
      - name: php-redis-devops
        image: gcr.io/google-samples/gb-frontend@sha256:a908df8486ff66f2c4daa0d3d8a2fa09846a1fc8efd65649c0109695c7c5cbff
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        env:
        - name: GET_HOSTS_FROM
          value: dns
        ports:
        - containerPort: 80

---
# Frontend Service
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  type: NodePort
  selector:
    app: guestbook
    tier: frontend
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30009
```