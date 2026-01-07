Day 31: Git Stash

Enunciado:
The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/cluster present on Storage server in Stratos DC. One of the developers stashed some in-progress changes in this repository, but now they want to restore some of the stashed changes. Find below more details to accomplish this task:

Look for the stashed changes under /usr/src/kodekloudrepos/cluster git repository, and restore the stash with stash@{1} identifier. Further, commit and push your changes to the origin.

Resolución:
- Simple Stash pop.
```bash
ssh natasha@ststor01
cd /usr/src/kodekloudrepos/cluster
git stash list
```
```bash
git stash pop 1
# o 
git stash apply stash@{1}
```
```bash
git status
git add .
git commit -m "restored stash changes"
git push origin master
```