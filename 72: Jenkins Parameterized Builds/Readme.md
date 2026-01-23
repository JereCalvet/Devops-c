Day 72: Jenkins Parameterized Builds

A new DevOps Engineer has joined the team and he will be assigned some Jenkins related tasks. Before that, the team wanted to test a simple parameterized job to understand basic functionality of parameterized builds. He is given a simple parameterized job to build in Jenkins. Please find more details below:


Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.


1. Create a parameterized job which should be named as parameterized-job


2. Add a string parameter named Stage; its default value should be Build.


3. Add a choice parameter named env; its choices should be Development, Staging and Production.


4. Configure job to execute a shell command, which should echo both parameter values (you are passing in the job).


5. Build the Jenkins job at least once with choice parameter value Production to make sure it passes.


Resolucción:
Jenkins Web UI:
1. Login con usuario admin y password Adm!n321
2. Crear un nuevo job llamado parameterized-job
3. Agregar un "String Parameter" llamado Stage con valor por defecto "Build"
4. Agregar un "Choice Parameter" llamado env con las opciones:
   - Development
   - Staging
   - Production
5. En la sección "Build", agregar un "Execute shell" con el siguiente script:
```bash
echo $Stage $env
```