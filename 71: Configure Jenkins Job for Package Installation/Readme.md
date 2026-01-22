Day 71: Configure Jenkins Job for Package Installation

Some new requirements have come up to install and configure some packages on the Nautilus infrastructure under Stratos Datacenter. The Nautilus DevOps team installed and configured a new Jenkins server so they wanted to create a Jenkins job to automate this task. Find below more details and complete the task accordingly:


1. Access the Jenkins UI by clicking on the Jenkins button in the top bar. Log in using the credentials: username admin and password Adm!n321.

2. Create a new Jenkins job named install-packages and configure it with the following specifications:

    Add a string parameter named PACKAGE.
    Configure the job to install a package specified in the $PACKAGE parameter on the storage server within the Stratos Datacenter.

Resolucción:
- Crear un usuario jenkins en el servidor de storage.
- Darle permisos de sudo sin password para instalar paquetes.
```bash
sudo useradd -m jenkins
visudo
# Agregar la siguiente línea al final del archivo:
jenkins ALL=(ALL) NOPASSWD: ALL
```
- Permitir acceso SSH sin password desde el servidor Jenkins al servidor de storage.
```bash
ssh-keygen
ssh-copy-id jenkins@storage-server
```
Jenkins Web UI:
1. Ingresar a Jenkins UI
2. Login con usuario admin y password Adm!n321
3. Crear un nuevo job llamado install-packages
4. Configurar el job como "This project is parameterized"
5. Agregar un "String Parameter" llamado PACKAGE
6. En la sección "Build", agregar un "Execute shell" con el siguiente script:
```bash
#!/bin/bash
ssh jenkins@storage-server "sudo dnf install -y $PACKAGE"
```