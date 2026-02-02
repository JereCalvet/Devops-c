Day 75: Jenkins Slave Nodes

The Nautilus DevOps team has installed and configured new Jenkins server in Stratos DC which they will use for CI/CD and for some automation tasks. There is a requirement to add all app servers as slave nodes in Jenkins so that they can perform tasks on these servers using Jenkins. Find below more details and accomplish the task accordingly.


1. Add all app servers as SSH build agent/slave nodes in Jenkins. Slave node name for app server 1, app server 2 and app server 3 must be App_server_1, App_server_2, App_server_3 respectively.


2. Add labels as below:


App_server_1 : stapp01

App_server_2 : stapp02

App_server_3 : stapp03


3. Remote root directory for App_server_1 must be /home/tony/jenkins, for App_server_2 must be /home/steve/jenkins and for App_server_3 must be /home/banner/jenkins.


4. Make sure slave nodes are online and working properly.

Resolución:
- Conectarse desde el servidor Jenkins a cada App Server con el usuario correspondiente y realizar los siguientes pasos:
```bash
# jenkins
ssh-keygen
# copiar la llave
vi /home/jenkins/.ssh/id_ed25519.pub
# App Server 1
ssh tony@stapp01
ssh-keygen
# pegar la llave copiada anteriormente
vi .ssh/authorized_keys
sudo dnf install -y java
```
- Jenkins Web UI:
1. Login con usuario admin y password Adm!n321
2. Ir a la configuracion de Jenkins -> Plugins -> Instalar plugin "SSH Build Agents"
3. Ir a la configuracion de Jenkins -> Credenciales -> Sistema -> Agregar Credenciales
   - Tipo: SSH Username with private key
   - Username: tony
   - Private Key: Enter directly -> pegar la llave privada de jenkins (/home/jenkins/.ssh/id_ed25519)
   - ID: stapp01-jenkins
4. Ir a la configuracion de Jenkins -> Manage Jenkins -> Manage Nodes and Clouds -> New Node
   - Node name: App_server_1
   - Tipo: Permanent Agent
   - Remote root directory: /home/tony/jenkins
   - Labels: stapp01
   - Launch method: Launch agents via SSH
   - Host: stapp01
   - Credentials: seleccionar la creada anteriormente (stapp01-jenkins)
   - Launch agent
5. Repetir los pasos 3 y 4 para App Server 2 y App Server 3, cambiando los datos correspondientes (usuario, host, remote root directory, labels, etc.)