
TLDR: Configure Jenkins user permissions.

1. Click on the Jenkins button on the top bar to access the Jenkins UI. Login with username admin and password Adm!n321.

2. Create a jenkins user named siva with the passworddCV3szSGNA. Their full name should match Siva.

3. Utilize the Project-based Matrix Authorization Strategy to assign overall read permission to the siva user.

4. Remove all permissions for Anonymous users (if any) ensuring that the admin user retains overall Administer permissions.

5. For the existing job, grant siva user only read permissions, disregarding other permissions such as Agent, SCM etc.

Resolución:
Pasos Web UI:
1. Ingresar a Jenkins UI
2. Login con usuario admin y password Adm!n321
3. Ir a "Manage Jenkins" -> "Manage Users" -> "Create User"
4. Crear usuario siva con password dCV3szSGNA y full name Siva
5. Descargar e instalar el plugin "Project-based Matrix Authorization Strategy"
6. Ir a "Manage Jenkins" -> "Configure Global Security"
7. Seleccionar "Project-based Matrix Authorization Strategy"
8. En la sección "Overall Permissions", asignar "Overall Read" al usuario siva
9. Remover todos los permisos para "Anonymous"
10. Asegurarse que el usuario admin tenga "Overall Administer" permisos
11. Guardar los cambios