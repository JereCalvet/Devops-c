Day 22: Clone Git Repository on Storage Server

Enunciado - clonar un repo:
The repository to be cloned is located at /opt/official.git

Clone this Git repository to the /usr/src/kodekloudrepos directory. Perform this task using the natasha user, and ensure that no modifications are made to the repository or existing directories, such as changing permissions or making unauthorized alterations.

Resolución:
- Cambiar al usuario natasha:
```bash
ssh natasha@ststor01
# o 
su - natasha 
```
- Clonar el repositorio:
```bash
git clone /opt/official.git /usr/src/kodekloudrepos
```