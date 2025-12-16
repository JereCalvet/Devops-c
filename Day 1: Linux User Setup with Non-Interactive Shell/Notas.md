Day 1: Linux User Setup with Non-Interactive Shell

Crear un usuario con shell no interactivo, es decir un usuario para servicios que no requiere acceso a una terminal (sin login).

Agregar un nuevo usuario con shell no interactivo:
```bash
sudo useradd -s /usr/sbin/nologin servicio_user
```
o 
```bash
sudo useradd -s /sbin/nologin servicio_user
``` 

El usuario existe pero:
* No puede iniciar sesión en el sistema.
* No puede acceder a una terminal.
* Puede ser utilizado para ejecutar servicios o procesos específicos.

Verificar el shell del usuario:
```bash
grep servicio_user /etc/passwd

Salida esperada:
servicio_user:x:1001:1001::/home/servicio_user:/usr/sbin/nologin
```



