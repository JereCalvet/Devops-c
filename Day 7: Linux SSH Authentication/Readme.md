Day 7: Linux SSH Authentication

Set up a password-less authentication from user thor on jump host to all app servers through their respective sudo users.

Copiar la llave publica para logear en el otro server sin password.
Habitualmente vi .ssh/id_rsa.pub y pegar en el otro server en el archivo .ssh/authorized_keys.
Descubrimiento el comando ssh-copy-id que hace esto automaticamente. 
```bash
ssh-copy-id usuario@server
Solicita password
# Luego de esto ya se puede logear sin password
ssh usuario@server

# Alternativa para especificar otra llave publica
ssh-copy-id -i ~/.ssh/otra.pub usuario@server
```
