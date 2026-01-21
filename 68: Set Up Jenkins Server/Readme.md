Day 68: Set Up Jenkins Server

TLDR: Install and configure Jenkins server.

1. Install Jenkins on the jenkins server using the yum utility only, and start its service.

    If you face a timeout issue while starting the Jenkins service, refer to this.

2. Jenkin's admin user name should be theadmin, password should be Adm!n321, full name should be Siva and email should be siva@jenkins.stratos.xfusioncorp.com.

Resolución:
```bash
ssh root@jenkins
sudo yum install wget
# https://www.jenkins.io/doc/book/installing/linux/
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat/jenkins.repo
sudo yum upgrade
sudo yum install fontconfig java-21-openjdk
sudo yum install jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins

# Paso 2: Unlock y configurar usuario admin
vi /var/lib/jenkins/secrets/initialAdminPassword
# Seguir el wizard para crear usuario admin con los datos
```