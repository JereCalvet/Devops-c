Day 16: Install and Configure Nginx as an LBR

Enunciado: 
Day by day traffic is increasing on one of the websites managed by the Nautilus production support team. Therefore, the team has observed a degradation in website performance. Following discussions about this issue, the team has decided to deploy this application on a high availability stack i.e on Nautilus infra in Stratos DC. They started the migration last month and it is almost done, as only the LBR server configuration is pending. Configure LBR server as per the information given below:


a. Install nginx on LBR (load balancer) server.

b. Configure load-balancing with the an http context making use of all App Servers. Ensure that you update only the main Nginx configuration file located at /etc/nginx/nginx.conf.

c. Make sure you do not update the apache port that is already defined in the apache configuration on all app servers, also make sure apache service is up and running on all app servers.

d. Once done, you can access the website using StaticApp button on the top bar.

Resolución:
- Instalar nginx
```bash
sudo dnf install nginx -y
```
- Detectar en que puerto estan corriendo los servicios apache en los app servers
```bash
sudo ss -tlpn
```
- Editar el archivo de configuracion principal de nginx
```bash
sudo vi /etc/nginx/nginx.conf
```
- Agregar el siguiente bloque http:
```nginx
http {
    upstream stapps {
        server stapp01:8087;
        server stapp02:8087;
        server stapp03:8087;
    }

    server {
        listen 80;
        location / {
            proxy_pass http://stapps;
        }
    }
}   
```
- Iniciar y habilitar nginx
```bash
sudo systemctl start nginx
```
- Probar desde jump host
```bash
curl -Ik http://<lbr-server-ip>/
```