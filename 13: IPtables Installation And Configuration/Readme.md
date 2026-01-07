Day 13: IPtables Installation And Configuration

Enunciado de la instalacion y configuracion de iptables
```
We have one of our websites up and running on our Nautilus infrastructure in Stratos DC. Our security team has raised a concern that right now Apache’s port i.e 3003 is open for all since there is no firewall installed on these hosts. So we have decided to add some security layer for these hosts and after discussions and recommendations we have come up with the following requirements:


1. Install iptables and all its dependencies on each app host.

2. Block incoming port 3003 on all apps for everyone except for LBR host.

3. Make sure the rules remain, even after system reboot.
```

Resolución:
- Instalar iptables
```bash
sudo dnf install iptables -y
sudo dnf install iptables-services -y
```

- Agregar las reglas:
```bash
# Permitir el trafico desde el LBR 
sudo iptables -I INPUT -p tcp --dport 3003 -s <LBR_IP_ADDRESS> -j ACCEPT
# Verificar
sudo iptables -L -n -v --line-numbers
# Bloquear el trafico desde cualquier otra IP
sudo iptables -A INPUT -p tcp --dport 3003 -j REJECT
# Verificar
sudo iptables -L -n -v --line-numbers
```

- Persistir las reglas
```bash
sudo service iptables save
# Verificar el contenido del archivo
sudo vi /etc/sysconfig/iptables
```
