Day 20: Configure Nginx + PHP-FPM Using Unix Sock

Enunciado:
The Nautilus application development team is planning to launch a new PHP-based application, which they want to deploy on Nautilus infra in Stratos DC. The development team had a meeting with the production support team and they have shared some requirements regarding the infrastructure. Below are the requirements they shared:


a. Install nginx on app server 1 , configure it to use port 8092 and its document root should be /var/www/html.

b. Install php-fpm version 8.2 on app server 1, it must use the unix socket /var/run/php-fpm/default.sock (create the parent directories if don't exist).

c. Configure php-fpm and nginx to work together.

d. Once configured correctly, you can test the website using curl http://stapp01:8092/index.php command from jump host.

NOTE: We have copied two files, index.php and info.php, under /var/www/html as part of the PHP-based application setup. Please do not modify these files.

Resolución:
- Instalacion de nginx
```bash
sudo dnf install nginx -y
```

- Instalacion de php-fpm 8.2 (Remi)
```bash
sudo dnf install -y epel-release
sudo dnf install -y https://rpms.remirepo.net/enterprise/remi-release-9.rpm
sudo dnf module reset php -y
sudo dnf module enable php:remi-8.2 -y
sudo dnf install -y php-fpm 
``` 

- Configuracion de php-fpm para usar el socket unix
```bash
sudo mkdir -p /var/run/php-fpm
sudo vi /etc/php-fpm.d/www.conf
# Cambiar las siguientes lineas:
listen = /var/run/php-fpm/default.sock
user = nginx
group = nginx
listen.owner = nginx
listen.group = nginx
listen.mode = 0660
# Si es necesario comentar linea:
;listen.acl_users = apache,nginx
```

- Iniciar y habilitar php-fpm
```bash
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
```

- **Importante:** Verificar que el socket se haya creado y tenga los permisos correctos
```bash
ls -l /var/run/php-fpm/default.sock
# Importante el propietario
srw-rw---- 1 nginx nginx 0 Jan  2 18:51 /var/run/php-fpm/default.sock
```

- Configuracion de nginx para usar el puerto 8092 y trabajar con php-fpm
```bash
sudo vi /etc/nginx/default.d/*.conf 
# creo que se llama php-fpm.conf, el archivo es el siguiente:
# # pass the PHP scripts to FastCGI server
#
# See conf.d/php-fpm.conf for socket configuration
#
# index index.php index.html index.htm;
# 
# location ~ \.php$ {
#     try_files $uri =404;
#     fastcgi_intercept_errors on;
#     fastcgi_index  index.php;
#     include        fastcgi_params;
#     fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
#     fastcgi_pass   php-fpm; -> modificar
# }
#
# **Modificar** fastcgi_pass a unix:/var/run/php-fpm/default.sock
sudo vi /etc/nginx/nginx.conf
# Cambiar el bloque  server
#      server {
#        listen       8092;
#        listen       [::]:8092;
#        server_name  _;
#        root         /var/www/html;
#        index index.php index.html;
#
#        # Load configuration files for the default server block.
#       include /etc/nginx/default.d/*.conf;
#
#        #error_page 404 /404.html;
#        #location = /404.html {
#        #}
#
#        #error_page 500 502 503 504 /50x.html;
#        #location = /50x.html {
#        #}
#    }
``

