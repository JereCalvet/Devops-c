Day 30: Git hard reset

Enunciado:
The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/official present on Storage server in Stratos DC. This was just a test repository and one of the developers just pushed a couple of changes for testing, but now they want to clean this repository along with the commit history/work tree, so they want to point back the HEAD and the branch itself to a commit with message add data.txt file. Find below more details:

In /usr/src/kodekloudrepos/official git repository, reset the git commit history so that there are only two commits in the commit history i.e initial commit and add data.txt file.

Also make sure to push your changes.

Resolución:
- Conectarse al servidor Storage en Stratos DC.
```bash
ssh natasha@ststor01
cd /usr/src/kodekloudrepos/official
git log
```
- Hacer un hard reset al commit identificado.
```bash
git reset --hard $hash
# o
git reset HEAD~10 --hard
```
- Forzar el push de los cambios al repositorio remoto.
```bash
git push origin master --force
```