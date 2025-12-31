Day 18: Configure LAMP server

Enunciado:
xFusionCorp Industries is planning to host a WordPress website on their infra in Stratos Datacenter. They have already done infrastructure configuration—for example, on the storage server they already have a shared directory /vaw/www/html that is mounted on each app host under /var/www/html directory. Please perform the following steps to accomplish the task:

a. Install httpd, php and its dependencies on all app hosts.


b. Apache should serve on port 8082 within the apps.


c. Install/Configure MariaDB server on DB Server.


d. Create a database named kodekloud_db1 and create a database user named kodekloud_cap identified as password Rc5C9EyvbU. Further make sure this newly created user is able to perform all operation on the database you created.


e. Finally you should be able to access the website on LBR link, by clicking on the App button on the top bar. You should see a message like App is able to connect to the database using user kodekloud_cap

Resolución:
- Instalar Apache, PHP y sus dependencias en todos los hosts de aplicaciones.
```bash
sudo dnf install httpd php php-mysql mysqlnd -y
```

- Configurar Apache para que sirva en el puerto 8082.
```bash
sudo vi /etc/httpd/conf/httpd.conf

# Cambiar la línea Listen 80 a Listen 8082
```
- Reiniciar el servicio de Apache para aplicar los cambios.
```bash
sudo systemctl restart httpd
```
- Instalar y configurar el servidor MariaDB en el servidor de base de datos.
```bash
sudo dnf install mariadb-server -y
sudo systemctl start mariadb
sudo systemctl enable mariadb
```
- Crear la base de datos y el usuario con los permisos necesarios.
```bash
sudo mysql 
# Crear la base de datos
CREATE DATABASE kodekloud_db1;
# Crear el usuario y otorgar permisos
CREATE USER 'kodekloud_cap'@'%' IDENTIFIED BY 'Rc5C9EyvbU';
GRANT ALL PRIVILEGES ON kodekloud_db1.* TO 'kodekloud_cap'@'%';
FLUSH PRIVILEGES;
EXIT;
```
- Verificar que la configuración es correcta accediendo al sitio web a través del enlace del LBR y comprobando que la aplicación puede conectarse a la base de datos utilizando el usuario kodekloud_cap.
```bash
curl http://stapp01
```