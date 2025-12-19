Day 11: Install and Configure Tomcat Server

The Nautilus application development team recently finished the beta version of one of their Java-based applications, which they are planning to deploy on one of the app servers in Stratos DC. After an internal team meeting, they have decided to use the tomcat application server. Based on the requirements mentioned below complete the task:


a. Install tomcat server on App Server 1.

b. Configure it to run on port 5002.

c. There is a ROOT.war file on Jump host at location /tmp.

Deploy it on this tomcat server and make sure the webpage works directly on base URL i.e curl http://stapp01:5002


Resolución:
- Instalacion de tomcat
```bash
sudo dnf install tomcat -y
```

- Configuracion del puerto 5002
```bash
sudo vi /etc/tomcat/server.xml
```
Cambiar el puerto:
```xml
<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

- Copiar el archivo ROOT.war desde el jump host al app server
Desde el jump host:
```bash
scp /tmp/ROOT.war user@stapp01:/tmp/
```
- Alternativa copiar desde el app server (si la red lo permite)
```bash
scp user@jump_host:/tmp/ROOT.war /var/lib/tomcat/webapps/
```

- Mover el archivo ROOT.war a la carpeta de despliegue de tomcat
```bash
sudo mv /tmp/ROOT.war /var/lib/tomcat/webapps/
```

Tambien se puede dar permisos de escritura al usuario tomcat para copiar directamente el archivo en la carpeta webapps
```bash
sudo chmod o+w /var/lib/tomcat/webapps/
scp user@jump_host:/tmp/ROOT.war /var/lib/tomcat/webapps/
sudo chmod o-w /var/lib/tomcat/webapps/
``` 

---
### Notas sobre tomcat:

Binarios:
- /usr/share/tomcat

Configuracion:
- /etc/tomcat

Apps (war files) desplegadas:
- /var/lib/tomcat/webapps

Logs:
- /var/log/tomcat

Archivos temporales / cache:
- /var/cache/tomcat


### Comparancion con apache:

Binarios:
- /usr/sbin/httpd

Configuracion:
- /etc/httpd

Apps (document root):
- /var/www/html

Logs:
- /var/log/httpd

Archivos temporales / cache:
- /var/cache/httpd
---
