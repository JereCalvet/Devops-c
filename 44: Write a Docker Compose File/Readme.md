Day 44: Write a Docker Compose File

Enunciado:
a. On App Server 3 in Stratos DC create a container named httpd using a docker compose file /opt/docker/docker-compose.yml (please use the exact name for file).

b. Use httpd (preferably latest tag) image for container and make sure container is named as httpd; you can use any name for service.

c. Map 80 number port of container with port 6400 of docker host.

d. Map container's /usr/local/apache2/htdocs volume with /opt/sysops volume of docker host which is already there. (please do not modify any data within these locations).

Resolución:
```bash
ssh banner@stapp03
cd /opt/docker
touch docker-compose.yml
vi docker-compose.yml
# Contenido del archivo docker-compose.yml:
services:
  web:
    image: httpd:latest
    container_name: httpd
    ports: 
      - "6400:80"
    volumes:
      - /opt/sysops:/usr/local/apache2/htdocs
# Guardar y salir del editor
```