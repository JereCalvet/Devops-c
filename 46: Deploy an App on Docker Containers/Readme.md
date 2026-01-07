Day 46: Deploy an App on Docker Containers

Enunciado:
TLDR: Builder docker-compose.yml
The compose should deploy two services (web and DB), and each service should deploy a container as per details below:

For web service: <br>
a. Container name must be php_host. <br>
b. Use image php with any apache tag. Check here for more details. <br>
c. Map php_host container's port 80 with host port 6100 <br>
d. Map php_host container's /var/www/html volume with host volume /var/www/html. <br>

For DB service: <br>
a. Container name must be mysql_host. <br>
b. Use image mariadb with any tag (preferably latest). Check here for more details. <br>
c. Map mysql_host container's port 3306 with host port 3306 <br>
d. Map mysql_host container's /var/lib/mysql volume with host volume /var/lib/mysql. <br>
e. Set MYSQL_DATABASE=database_host and use any custom user ( except root ) with some complex password for DB connections. <br>
After running docker-compose up you can access the app with curl command curl <server-ip or hostname>:6100/

Resolución:
```bash
ssh steve@stapp02
cd /opt/docker
touch docker-compose.yml
vi docker-compose.yml
# Agregar la siguiente configuracion al archivo docker-compose.yml
# services:
#   web:
#     image: php:apache
#     container_name: php_host
#     ports:
#       - "6100:80"
#     volumes:
#       - /var/www/html:/var/www/html
#   database:
#     image: mariadb:latest
#     container_name: mysql_host
#     ports:
#       - "3307:3306"
#     volumes:
#       - /var/lib/mysql:/var/lib/mysql
#     environment:
#       MYSQL_DATABASE: database_host
#       MYSQL_USER: usuario
#       MYSQL_PASSWORD: password
#       MYSQL_ROOT_PASSWORD: root_password
# Guardar y salir del archivo
docker-compose up -d
curl http://localhost:6100/
```

