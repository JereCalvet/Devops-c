Day 35: Install Docker Packages and Start Docker Service

Enunciado:
TLDR: Instalar docker y docker compose

Resolución:
- SSH al server
```bash
ssh steve@stapp02
sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf install docker-ce -y
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker        
```
- Agregar el usuario steve al grupo docker para no usar sudo
```bash
sudo usermod -aG docker steve
```
- Salir y volver a entrar para que los cambios tengan efecto
```bash
exit
ssh steve@stapp02
```
- Verificar que steve pueda correr docker sin sudo
```bash
docker ps
```

Bonus, eliminar un repo en dnf
```bash
sudo dnf repolist -all
# disable repo
sudo dnf config-manager --set-disabled <repo-name>
# remove repo
sudo rm /etc/yum.repos.d/<repo-name>.repo
```