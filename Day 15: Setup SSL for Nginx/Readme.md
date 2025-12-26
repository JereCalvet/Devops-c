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

___


## Notas de nginx:
- Sirve para:
    - 🌐 Servidor web (servir HTML, CSS, JS, imágenes)
    - 🔁 Reverse proxy (recibir requests y reenviarlas a otro servicio)
    - ⚖️ Load balancer (distribuir tráfico entre varios backends)
    - 🚪 Gateway (punto de entrada a apps)

### Configuración básica:
    /etc/nginx/       # Directorio principal de configuración de Nginx
    ├── nginx.conf    # Archivo raíz de configuración 
    ├── mime.types    # Tipos MIME para manejar diferentes formatos de archivos
    ├── conf.d/       # Configuraciones adicionales
    ├── sites-available/
    ├── sites-enabled/
    ├── snippets/
    └── modules-enabled/

#### - nginx.conf:
    - Punto de entrada de Nginx
    - Define parametros globales y bloques principales (events, http)
    - Cargar módulos adicionales
    - Incluir configuraciones adicionales desde conf.d/

Ejemplo:
```nginx
user nginx;
worker_processes auto;

events {
    worker_connections 1024;
}

http {
    include mime.types;
    include /etc/nginx/conf.d/*.conf;
}
```

#### - mime.types:
    - Define tipos MIME para diferentes extensiones de archivos
    - Ayuda a Nginx a servir contenido correctamente basado en el tipo de archivo
    - Le dice al navegador que archivos esta recibiendo (HTML, CSS, JS, imágenes, etc.) enviando el header Content-Type 

Ejemplo:
```nginx
types {
    text/html html;
    text/css css;
    application/javascript js;
    image/jpeg jpg jpeg;
    image/png png;
}
```

#### Virtual Hosts (server blocks):
    - Permite alojar múltiples sitios web en un solo servidor Nginx
    - Cada bloque define configuración específica para un dominio o subdominio
    - Ubicación común: /etc/nginx/sites-available/ y /etc/nginx/sites-enabled/

Ejemplo:
```nginx
server {
    listen 80;
    server_name example.com www.example.com;    

    root /var/www/example;
    index index.html;
}
```

##### Convenciones CentOS vs Ubuntu:
- Ubuntu: <br/>
/etc/nginx/ <br/>
├── nginx.conf <br/>
├── sites-available/ <br/>
│   └── example.conf <br/>
├── sites-enabled/ <br/>
│   └── example.conf -> ../sites-available/example.conf <br/>

1. Se define el sitio en sites-available.
2. Se crea un enlace simbólico en sites-enabled para activar el sitio.
3. nginx.conf incluye solo: `include /etc/nginx/sites-enabled/*;`

Permite activar/desactivar sitios fácilmente sin eliminar archivos de configuración. (borrar el enlace simbólico desactiva el sitio)

- CentOS: <br/>
/etc/nginx/ <br/>
├── nginx.conf <br/>
├── conf.d/ <br/>
│   ├── app.conf <br/>
│   ├── api.conf <br/>
│   └── default.conf <br/>
1. Se define cada sitio directamente en conf.d/ como archivos .conf separados.
2. nginx.conf incluye: `include /etc/nginx/conf.d/*.conf;`

#### Modulos de Nginx:
Agregan funcionalidades adicionales a Nginx. Algunos modulos comunes:
- http_ssl_module: Soporte para SSL/TLS
- http_rewrite_module: Reescritura de URLs
- http_gzip_module: Compresión gzip
- http_proxy_module: Soporte para proxy reverso
- http_headers_module: Manejo de headers HTTP
- http_limit_req_module: Rate limit

Se pueden habilitar o deshabilitar modulos al compilar Nginx o mediante configuraciones en nginx.conf.
```nginx
gzip on;
ngx_http_ssl_module on;
```
Revisar en /etc/nginx/modules-enabled/ los modulos habilitados.

#### Logs
- Ubicación común: /var/log/nginx/
- access.log: Registra todas las solicitudes recibidas por el servidor
- error.log: Registra errores y problemas del servidor

#### Comandos útiles:
```bash
# Validar sintaxis de configuración
nginx -t
# Recargar configuración sin reiniciar (mantiene conexiones activas)
systemctl reload nginx
# Reiniciar Nginx (cierra conexiones activas)
systemctl restart nginx
```

#### Ejemplos:
-  **Proxy reverso con SSL:** 
```nginx
server {
    listen 443 ssl;
    server_name example.com;
    ssl_certificate /etc/nginx/ssl/example.crt;
    ssl_certificate_key /etc/nginx/ssl/example.key;
   
    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}   
```

- **Load balancer:**
Transfiere solicitudes entrantes a múltiples servidores backend para distribuir la carga.

Algoritmos comunes:

##### Round Robin:
El algoritmo de balanceo por defecto es round-robin, no considera la carga actual de los servidores.
Ej: <br/>
req1 → app01 <br/>
req2 → app02 <br/>
req3 → app01 <br/>
req4 → app02 <br/>

```nginx
upstream backend {
    server app1.example.com;
    server app2.example.com;
}

server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend;
    }
}
```

##### Least Connections:
El algoritmo distribuye las solicitudes al servidor con la menor cantidad de conexiones activas en ese momento.
```nginx
upstream backend {
    least_conn;
    server app1.example.com;
    server app2.example.com;
}
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend;
    }
}
```
##### Weight (peso):
Asigna diferentes pesos a cada servidor backend. Los servidores con mayor peso reciben más solicitudes.
```nginx
upstream backend {
    server app1.example.com weight=3;
    server app2.example.com weight=1;
}
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend;
    }
}
``` 

##### IP Hash (afinidad de IP):
El algorimot determina que un cliente va siempre a la misma instancia backend basado en su IP. Util para sesiones guardadas en memoria. Sticky sessions.
```nginx
upstream backend {
    ip_hash;
    server app1.example.com;
    server app2.example.com;
}
server {
    listen 80;
    server_name example.com;
    location / {
        proxy_pass http://backend;
    }
}
```

##### Hash por header/cookie:
Distribuye solicitudes basadas en un valor hash de un header o cookie específico, entonces el cliente siempre va al mismo backend basado en ese valor. Sticky sessions.
```nginx
upstream backend {
    hash $http_user_agent consistent;
    server app1.example.com;
    server app2.example.com;
}
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend;
    }
}
```

##### Ejemplo de estructura tipica:
/etc/nginx/ <br/>
├── nginx.conf <br/>
├── conf.d/ <br/>
│   ├── 00-default-http.conf <br/>
│   ├── 00-default-https.conf <br/>
│   ├── app1.conf <br/>
│   ├── app2.conf <br/>
│   └── api.conf <br/>

- conf.d/:
    - 00-default-http.conf: Redirige todo el tráfico HTTP a HTTPS
    - 00-default-https.conf: Configuración SSL global
    - app1.conf: Configuración del sitio web App 1
    - app2.conf: Configuración del sitio web App 2
    - api.conf: Configuración del servicio API
```nginx
# 00-default-http.conf
server {
    listen 80;
    listen [::]:80;

    server_name _;

    return 301 https://$host$request_uri;
}
```
```nginx
# 00-default-https.conf
server {
    listen 443 ssl;
    listen [::]:443 ssl default_server;

    server_name _;

    ssl_certificate /etc/nginx/ssl/default.crt;
    ssl_certificate_key /etc/nginx/ssl/default.key;

    return 444;
}

```
```nginx
# app1.conf
server {
    listen 443 ssl;
    server_name app1.example.com;
    ssl_certificate /etc/nginx/ssl/app1.crt;
    ssl_certificate_key /etc/nginx/ssl/app1.key;
    root /var/www/app1;
    index index.html;
}
```

##### Test
```bash
curl -Ik https://<app-server-ip>/
# Flags:
#   -I: Solo obtener headers HTTP
#   -k: Omitir verificación de certificado SSL (útil para certificados autofirmados)
```