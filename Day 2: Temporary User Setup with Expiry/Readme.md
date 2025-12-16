Day 2: Temporary User Setup with Expiry

Crear un usuario temporal con fecha de expiración.

Agregar un nuevo usuario con fecha de expiración (por ejemplo, 7 días):

```bash
sudo useradd -e $(date -d "+7 days" +%Y-%m-%d) tempuser
```

Agregar un nuevo usuario con fecha de expiración específica:
```bash
sudo useradd -e 2024-12-31 tempuser
```

Establecer una contraseña para el usuario temporal:
```bash
sudo passwd tempuser
```

Verificar la fecha de expiración del usuario:
```bash
chage -l tempuser
```

La cuenta del usuario "tempuser" expirará en la fecha especificada. Después de esa fecha, el usuario no podrá iniciar sesión.