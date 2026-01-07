Day 10: Linux Bash Scripts

Enunciado de un script medio complejo
```
The system admins team of xFusionCorp Industries has written a script to take backups of a website from the application servers in Stratos Datacenter. However, the script is not working properly.

Your task is to create a bash script on the given App Server to perform the following actions:

*Create a zip archive named xfusioncorp_blog.zip of the /var/www/html/blog directory.

*Save the created archive in the /backup/ directory on the App Server.
Note: This location is temporary and backups from this location are cleaned on a weekly basis.

*Copy the created archive to the Nautilus Backup Server in the /backup/ directory.

*Ensure that the script does not ask for any password while copying the archive file.

*The respective App Server user (for example, tony on App Server 1) must be able to execute the script.

Do not use sudo inside the script.

Note:
The zip package must be installed on the App Server before executing the script.
Install it manually, outside the script.
```
Resolución:

- Generar la llave ssh:
```bash
ssh-keygen
```
- Copiar la llave publica al server de backup para logear sin password:
```bash
ssh-copy-id clint@stbkp01
```
- Script que va en la carpeta /scripts
backup_blog.sh
```bash
#!/bin/bash
cd /var/www/html # Nos movemos a la carpeta donde esta el blog
zip -r /backup/xfusioncorp_blog.zip blog # Creamos el zip del blog en la carpeta /backup
scp /backup/xfusioncorp_blog.zip clint@stbkp01:/backup/ # Copiamos el zip al server de backup
```
- Dar permisos de ejecucion al script
```bash
chmod o+rx backup_blog.sh
```