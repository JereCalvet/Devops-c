Day 73: Jenkins Scheduled Jobs

TLDR: Crear un job que copie logs de apache cada 2 minutos desde app server a storage server.

1. Create a Jenkins jobs named copy-logs.

2. Configure it to periodically build every 2 minutes to copy the Apache logs (both access_log and error_logs) from App Server 1 (from default logs location) to location /usr/src/security on Storage Server.

Resolución:
- Crear un usuario jenkins en el App Server 1.
```bash
sudo useradd -m jenkins
```
- Permitir el accesso SSH sin password desde el servidor Jenkins al App Server 1.
```bash
ssh-keygen
ssh-copy-id jenkins@stapp01
```
- Crear el usuario jenkins en el Storage Server.
```bash
sudo useradd -m jenkins
```
- Permitir el acceso SSH sin password desde App Server 1 al Storage Server.
```bash
ssh-keygen
ssh-copy-id jenkins@ststor01
```
Jenkins Web UI:
1. Login con usuario admin y password Adm!n321
2. Crear un nuevo job llamado copy-logs
3. Configurar el job para que se ejecute periódicamente cada 2 minutos:
```
H/2 * * * *
```
4. En "Build", agregar un "Execute shell" con el siguiente script:
```bash
ssh jenkins@stapp01 "scp /var/log/httpd/* jenkins@ststor01:/usr/src/security"
```