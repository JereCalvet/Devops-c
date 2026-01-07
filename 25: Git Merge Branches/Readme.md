Day 25: Git Merge Branches

Enunciado:
Create a new branch devops in /usr/src/kodekloudrepos/apps repo from master and copy the /tmp/index.html file (present on storage server itself) into the repo. Further, add/commit this file in the new branch and merge back that branch into master branch. Finally, push the changes to the origin for both of the branches.

Solución:
```bash
ssh user@storage-server
cd /usr/src/kodekloudrepos/apps
git checkout -b devops master
cp /tmp/index.html .
git add index.html
git commit -m "Add index.html file"
git checkout master
git merge devops
git push origin
```