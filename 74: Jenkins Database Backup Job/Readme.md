Day 74: Jenkins Database Backup Job

There is a requirement to create a Jenkins job to automate the database backup. Below you can find more details to accomplish this task:

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.

-    Create a Jenkins job named database-backup.

-    Configure it to take a database dump of the kodekloud_db01 database present on the Database server in Stratos Datacenter, the database user is kodekloud_roy and password is asdfgdsd.

-    The dump should be named in db_$(date +%F).sql format, where date +%F is the current date.

-    Copy the db_$(date +%F).sql dump to the Backup Server under location /home/clint/db_backups.

-    Further, schedule this job to run periodically at */10 * * * * (please use this exact schedule format).

Resolución:
- Crear un usuario jenkins en el Database Server.
```bash
sudo useradd -m jenkins
```
- Permitir el accesso SSH sin password desde el servidor Jenkins al Database Server.
```bash
#sevidor Jenkins
ssh-keygen
# copiar llave manualmente al Database Server
vi /home/jenkins/.ssh/id_ed25519.pub
ssh peter@stdb01
touch /home/jenkins/.ssh/authorized_keys
# pegar la llave copiada anteriormente
chmod 600 /home/jenkins/.ssh/authorized_keys
```
- Permitir el acceso SSH sin password desde el Database Server al Backup Server.
```bash
ssh-copy-id clint@stbkp01
```
- Crear script para backup de la DB en el Database Server.
```bash
vi /home/jenkins/backup_db.sh
#!/bin/bash
DUMP_FILE="db_$(date +%F).sql"
mysqldump -u kodekloud_roy -pasdfgdsd kodekloud_db01 > /tmp/$DUMP_FILE
scp /tmp/$DUMP_FILE clint@stbkp01:/home/clint/db_backups/
rm /tmp/$DUMP_FILE
```
- Jenkins Web UI:
1. Login con usuario admin y password Adm!n321
2. Crear un nuevo job llamado database-backup
3. Configurar el job para que se ejecute periódicamente cada 10 minutos:
```
*/10 * * * * 
```
4. En "Build", agregar un "Execute shell" con el siguiente script:
```bash
ssh jenkins@stdb01 "sh backup_db.sh"
```