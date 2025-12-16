Day 3: Secure Root SSH Access

Deshabilitar el acceso SSH directo para el usuario root.

Editar el archivo de configuración SSH:
```bash
sudo vi /etc/ssh/sshd_config
```
Buscar la línea y modificar:
PermitRootLogin no

Reiniciar el servicio SSH para aplicar los cambios:
```bash
sudo systemctl restart sshd
```

Para mayor seguridad, también se puede deshabilitar la autenticación por contraseña y usar solo claves SSH:
En el archivo /etc/ssh/sshd_config, buscar y modificar:
PasswordAuthentication no