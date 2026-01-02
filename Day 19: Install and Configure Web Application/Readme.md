Day 19: Install and Configure Web Application

xFusionCorp Industries is planning to host two static websites on their infra in Stratos Datacenter. The development of these websites is still in-progress, but we want to get the servers ready. Please perform the following steps to accomplish the task:


a. Install httpd package and dependencies on app server 3.

b. Apache should serve on port 3004.

c. There are two website's backups /home/thor/news and /home/thor/games on jump_host. Set them up on Apache in a way that news should work on the link http://localhost:3004/news/ and games should work on link http://localhost:3004/games/ on the mentioned app server.

d. Once configured you should be able to access the website using curl command on the respective app server, i.e curl http://localhost:3004/news/ and curl http://localhost:3004/games/

Resolución:
- Copiar los archivos desde el jump host al app server
Desde el jump host:
```bash
scp -r /home/thor/games user@stapp03:/tmp/
scp -r /home/thor/news user@stapp03:/tmp/
```

- Instalacion de httpd
```bash
sudo dnf install httpd -y
``` 

- Configuracion del puerto 3004
```bash
sudo vi /etc/httpd/conf/httpd.conf
# Cambiar el puerto
Listen 3004
```

- Mover los archivos a la carpeta de despliegue de apache
```bash
sudo mv /tmp/games /var/www/html/
sudo mv /tmp/news /var/www/html/
``` 
