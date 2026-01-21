Day 69: Install Jenkins Plugins

TLDR: Install Git and GitLab plugins in Jenkins.

1. Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and Adm!n321 password.

2. Once logged in, install the Git and GitLab plugins. Note that you may need to restart Jenkins service to complete the plugins installation, If required, opt to Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre.

Note:

1. After restarting the Jenkins service, wait for the Jenkins login page to reappear before proceeding.

2. For tasks involving web UI changes, capture screenshots to share for review or consider using screen recording software like loom.com for documentation and sharing.


Resolución
Pasos Web UI:
1. Ingresar a Jenkins UI: http://jenkins:8080
2. Login con usuario admin y password Adm!n321
3. Ir a "Manage Jenkins" -> "Manage Plugins"
4. Ir a la pestaña "Available"
5. Buscar "Git Plugin", seleccionar e instalar sin reiniciar
6. Buscar "GitLab Plugin", seleccionar e instalar sin reiniciar
7. Una vez instalados ambos plugins, ir a "Manage Jenkins" -> "Manage Plugins" -> "Advanced"
8. Hacer click en "Restart Jenkins when installation is complete and no jobs are running"
9. Esperar a que Jenkins reinicie y aparezca la pantalla de login
10. Login nuevamente con usuario admin y password Adm!n321