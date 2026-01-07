Day 24: Git Create Branches

Enunciado:
On Storage server in Stratos DC create a new branch xfusioncorp_official from master branch in /usr/src/kodekloudrepos/official git repo.

Resolución:
- Conectarse al servidor Storage:
```bash
ssh natasha@ststor01
```
- Cambiar al directorio del repositorio:
```bash
cd /usr/src/kodekloudrepos/official
```
- Crear la nueva rama desde master:
```bash
git checkout master
git checkout -b xfusioncorp_official
# o
git checkout -b xfusioncorp_official master
```