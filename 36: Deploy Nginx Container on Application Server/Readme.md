Day 36: Deploy Nginx Container on Application Server

Enunciado:
TLDR: Deploy a Nginx alpine container on the application server stapp02 using Docker.

Resolución:
- SSH al server
```bash
ssh steve@stapp02
docker run -d --name nginx_2 nginx:alpine
docker ps
```