Day 17: Install and Configure PostgreSQL

Enunciado:
The Nautilus application development team has shared that they are planning to deploy one newly developed application on Nautilus infra in Stratos DC. The application uses PostgreSQL database, so as a pre-requisite we need to set up PostgreSQL database server as per requirements shared below:

PostgreSQL database server is already installed on the Nautilus database server.

a. Create a database user kodekloud_sam and set its password to YchZHRcLkL.

b. Create a database kodekloud_db5 and grant full permissions to user kodekloud_sam on this database.

Resolución:
- Acceder a la consola de PostgreSQL
```bash
sudo -u postgres psql
```
- Crear el usuario de la base de datos
```sql
CREATE USER kodekloud_sam WITH PASSWORD 'YchZHRcLkL';
```
- Crear la base de datos
```sql
CREATE DATABASE kodekloud_db5 OWNER kodekloud_sam;
```
- Alternativa sin OWNER, para conceder todos los privilegios al usuario sobre la base de datos
```sql
GRANT ALL PRIVILEGES ON DATABASE kodekloud_db5 TO kodekloud_sam;
```
- Comprobar que el usuario y la base de datos han sido creados
```sql
\du
\l
```
____

Notas PostgreSQL:
- Para listar los usuarios: `\du`
- Para listar las bases de datos: `\l`
- Para salir de la consola de PostgreSQL: `\q`
- Para conectarse a una base de datos específica: `\c nombre_basedatos`
- Para listar las tablas de la base de datos actual: `\dt`
- Para ver la configuración actual del servidor PostgreSQL: `SHOW ALL;`


- Para Crear un usuario: `CREATE USER nombre_usuario WITH PASSWORD 'contraseña';`
- Para cambiar la contraseña de un usuario: `ALTER USER nombre_usuario WITH PASSWORD 'nueva_contraseña';`
- Para eliminar un usuario: `DROP USER nombre_usuario;`


- Para Crear una base de datos: `CREATE DATABASE nombre_basedatos OWNER nombre_usuario;`
- Para eliminar una base de datos: `DROP DATABASE nombre_basedatos;`
- Para conceder todos los privilegios a un usuario sobre una base de datos: `GRANT ALL PRIVILEGES ON DATABASE nombre_basedatos TO nombre_usuario;`
- Para revocar todos los privilegios a un usuario sobre una base de datos: `REVOKE ALL PRIVILEGES ON DATABASE nombre_basedatos FROM nombre_usuario;`


- Para Crear un rol: `CREATE ROLE nombre_rol;`
- Para asignar un rol a un usuario: `GRANT nombre_rol TO nombre_usuario;`
- Para eliminar un rol: `DROP ROLE nombre_rol;`

Ejemplo de utilización de los roles:
```sql
-- roles de permisos
CREATE ROLE app_read;
CREATE ROLE app_write;

-- permisos
GRANT SELECT ON ALL TABLES IN SCHEMA public TO app_read;
GRANT INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO app_write;

-- usuario
CREATE ROLE app_user LOGIN PASSWORD 'secret';

-- asignar roles
GRANT app_read, app_write TO app_user;
```
