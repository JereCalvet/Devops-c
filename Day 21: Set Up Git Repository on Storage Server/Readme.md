Day 21: Set Up Git Repository on Storage Server

Enunciado:

The Nautilus development team has provided requirements to the DevOps team for a new application development project, specifically requesting the establishment of a Git repository. Follow the instructions below to create the Git repository on the Storage server in the Stratos DC:

Utilize yum to install the git package on the Storage Server.

Create a bare repository named /opt/apps.git (ensure exact name usage).

Resolución:
- Instalar git:
```bash
sudo yum install git -y
```
- Crear el repositorio bare:
```bash
sudo git init --bare /opt/apps.git
```

Notas: 
Un repositorio "bare" es un repositorio de Git que no contiene un árbol de trabajo. Se utiliza principalmente para compartir el repositorio con otros usuarios y para alojar repositorios remotos.