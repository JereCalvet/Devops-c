Day 15: Setup SSL for Nginx

The system admins team of xFusionCorp Industries needs to deploy a new application on App Server 2 in Stratos Datacenter. They have some pre-requites to get ready that server for application deployment. Prepare the server as per requirements shared below:


1. Install and configure nginx on App Server 2.

2. On App Server 2 there is a self signed SSL certificate and key present at location /tmp/nautilus.crt and /tmp/nautilus.key. Move them to some appropriate location and deploy the same in Nginx.

3. Create an index.html file with content Welcome! under Nginx document root.

4. For final testing try to access the App Server 2 link (either hostname or IP) from jump host using curl command. For example curl -Ik https://<app-server-ip>/.

Resolución: 
- Instalar nginx
```bash
sudo dnf install nginx -y
```
- Mover el certificado y la key a una ubicacion apropiada
```bash
sudo mv /tmp/nautilus.crt /etc/nginx/ssl/nautilus.crt
sudo mv /tmp/nautilus.key /etc/nginx/ssl/nautilus.key
```

- Configurar nginx para usar SSL
```bash
sudo vi /etc/nginx/conf.d/default.conf
```
- Agregar el siguiente bloque server:
```nginx
server {
    listen 443 ssl;
    server_name _;  
    ssl_certificate /etc/nginx/ssl/nautilus.crt;
    ssl_certificate_key /etc/nginx/ssl/nautilus.key;
    root /usr/share/nginx/html;
    index index.html;
}
```
- Crear el archivo index.html
```bash
echo "Welcome!" | sudo tee /usr/share/nginx/html/index.html
```
- Iniciar y habilitar nginx
```bash
sudo systemctl start nginx
```
- Probar desde jump host
```bash
curl -Ik https://<app-server-ip>/
```
