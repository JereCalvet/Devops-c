Day 53: Resolve VolumeMounts Issue in Kubernetes


We encountered an issue with our Nginx and PHP-FPM setup on the Kubernetes cluster this morning, which halted its functionality. Investigate and rectify the issue:

The pod name is `nginx-phpfpm` and configmap name is `nginx-config`. Identify and fix the problem.

Once resolved, copy /home/thor/index.php file from the jump host to the nginx-container within the nginx document root. After this, you should be able to access the website using Website button on the top bar.

Resolucoón:
```bash
touch pods.yml
vi pods.yml
# Contenido
# apiVersion: v1
# kind: Pod
# metadata:
#   name: nginx-phpfpm
#   labels:
#     app: php-app
# spec:
#   containers:
#   - name: php-fpm-container
#     image: php:7.2-fpm-alpine
#     volumeMounts:
#     - name: shared-files
#       mountPath: /var/www/html <-- Corregido, antes era /usr/share/nginx/html
#   - name: nginx-container
#     image: nginx:latest
#     volumeMounts:
#     - name: shared-files
#       mountPath: /var/www/html 
#   volumes:
#   - name: shared-files
#     emptyDir: {}
#   - name: nginx-config-volume
#     configMap:
#       name: nginx-config
# Guardar y salir
kubectl delete pod nginx-phpfpm
kubectl apply -f pods.yml
kubectl get pods nginx-phpfpm -o yaml
kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html/index.php -c nginx-container
```

Salida del comando kubectl describe configmaps nginx-config:
```bash
kubectl describe configmaps nginx-config

# Name:         nginx-config
# Namespace:    default
# Labels:       <none>
# Annotations:  <none>
# 
# Data
# ====
# nginx.conf:
# ----
# events {
# }
# http {
#   server {
#     listen 8099 default_server;
#     listen [::]:8099 default_server;
# 
#     # Set nginx to serve files from the shared volume!
#     root /var/www/html;
#     index  index.html index.htm index.php;
#     server_name _;
#     location / {
#       try_files $uri $uri/ =404;
#     }
#     location ~ \.php$ {
#       include fastcgi_params;
#       fastcgi_param REQUEST_METHOD $request_method;
#       fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
#       fastcgi_pass 127.0.0.1:9000;
#     }
#   }
# }
# 
# 
# BinaryData
# ====
```

Salida del comando kubectl get pods nginx-phpfpm -o yaml:
```bash
kubectl get pods nginx-phpfpm -o yaml
# apiVersion: v1
# kind: Pod
# metadata:
#   annotations:
#     kubectl.kubernetes.io/last-applied-configuration: |
#       {"apiVersion":"v1","kind":"Pod","metadata":{"annotations":{},"labels":{"app":"php-app"},"name":"nginx-phpfpm","namespace":"default"},"spec":{"containers":[{"image":"php:7.2-fpm-alpine","name":"php-fpm-container","volumeMounts":[{"mountPath":"/usr/share/nginx/html","name":"shared-files"}]},{"image":"nginx:latest","name":"nginx-container","volumeMounts":[{"mountPath":"/var/www/html","name":"shared-files"},{"mountPath":"/etc/nginx/nginx.conf","name":"nginx-config-volume","subPath":"nginx.conf"}]}],"volumes":[{"emptyDir":{},"name":"shared-files"},{"configMap":{"name":"nginx-config"},"name":"nginx-config-volume"}]}}
#   creationTimestamp: "2026-01-12T17:50:12Z"
#   labels:
#     app: php-app
#   name: nginx-phpfpm
#   namespace: default
#   resourceVersion: "1863"
#   uid: e8d80ae8-f3fa-4b9c-963c-23f83556c7f6
# spec:
#   containers:
#   - image: php:7.2-fpm-alpine
#     imagePullPolicy: IfNotPresent
#     name: php-fpm-container
#     resources: {}
#     terminationMessagePath: /dev/termination-log
#     terminationMessagePolicy: File
#     volumeMounts:
#     - mountPath: /usr/share/nginx/html
#       name: shared-files
#     - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
#       name: kube-api-access-5bd7n
#       readOnly: true
#   - image: nginx:latest
#     imagePullPolicy: Always
#     name: nginx-container
#     resources: {}
#     terminationMessagePath: /dev/termination-log
#     terminationMessagePolicy: File
#     volumeMounts:
#     - mountPath: /var/www/html
#       name: shared-files
#     - mountPath: /etc/nginx/nginx.conf
#       name: nginx-config-volume
#       subPath: nginx.conf
#     - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
#       name: kube-api-access-5bd7n
#       readOnly: true
#   dnsPolicy: ClusterFirst
#   enableServiceLinks: true
#   nodeName: kodekloud-control-plane
#   preemptionPolicy: PreemptLowerPriority
#   priority: 0
#   restartPolicy: Always
#   schedulerName: default-scheduler
#   securityContext: {}
#   serviceAccount: default
#   serviceAccountName: default
#   terminationGracePeriodSeconds: 30
#   tolerations:
#   - effect: NoExecute
#     key: node.kubernetes.io/not-ready
#     operator: Exists
#     tolerationSeconds: 300
#   - effect: NoExecute
#     key: node.kubernetes.io/unreachable
#     operator: Exists
#     tolerationSeconds: 300
#   volumes:
#   - emptyDir: {}
#     name: shared-files
#   - configMap:
#       defaultMode: 420
#       name: nginx-config
#     name: nginx-config-volume
#   - name: kube-api-access-5bd7n
#     projected:
#       defaultMode: 420
#       sources:
#       - serviceAccountToken:
#           expirationSeconds: 3607
#           path: token
#       - configMap:
#           items:
#           - key: ca.crt
#             path: ca.crt
#           name: kube-root-ca.crt
#       - downwardAPI:
#           items:
#           - fieldRef:
#               apiVersion: v1
#               fieldPath: metadata.namespace
#             path: namespace
# status:
#   conditions:
#   - lastProbeTime: null
#     lastTransitionTime: "2026-01-12T17:50:12Z"
#     status: "True"
#     type: Initialized
#   - lastProbeTime: null
#     lastTransitionTime: "2026-01-12T17:50:23Z"
#     status: "True"
#     type: Ready
#   - lastProbeTime: null
#     lastTransitionTime: "2026-01-12T17:50:23Z"
#     status: "True"
#     type: ContainersReady
#   - lastProbeTime: null
#     lastTransitionTime: "2026-01-12T17:50:12Z"
#     status: "True"
#     type: PodScheduled
#   containerStatuses:
#   - containerID: containerd://c8df163eda280499e038405eb3bb070acedac6eac542bbc95e81d60723b083a4
#     image: docker.io/library/nginx:latest
#     imageID: docker.io/library/nginx@sha256:7272239bd21472f311aa3e86a85fdca0f1ad648995f983ab6e5e7dea665cd233
#     lastState: {}
#     name: nginx-container
#     ready: true
#     restartCount: 0
#     started: true
#     state:
#       running:
#         startedAt: "2026-01-12T17:50:22Z"
#   - containerID: containerd://2704fbe7fce05da002ebb10074f298002103c8b5f4d19c8f4857f3d4414d85ea
#     image: docker.io/library/php:7.2-fpm-alpine
#     imageID: docker.io/library/php@sha256:2e2d92415f3fc552e9a62548d1235f852c864fcdc94bcf2905805d92baefc87f
#     lastState: {}
#     name: php-fpm-container
#     ready: true
#     restartCount: 0
#     started: true
#     state:
#       running:
#         startedAt: "2026-01-12T17:50:16Z"
#   hostIP: 172.17.0.2
#   phase: Running
#   podIP: 10.244.0.5
#   podIPs:
#   - ip: 10.244.0.5
#   qosClass: BestEffort
#   startTime: "2026-01-12T17:50:12Z"
```
