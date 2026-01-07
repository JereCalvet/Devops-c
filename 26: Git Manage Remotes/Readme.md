Day 26: Git Manage Remotes

Enunciado:
The xFusionCorp development team added updates to the project that is maintained under /opt/news.git repo and cloned under /usr/src/kodekloudrepos/news. Recently some changes were made on Git server that is hosted on Storage server in Stratos DC. The DevOps team added some new Git remotes, so we need to update remote on /usr/src/kodekloudrepos/news repository as per details mentioned below:


a. In /usr/src/kodekloudrepos/news repo add a new remote dev_news and point it to /opt/xfusioncorp_news.git repository.


b. There is a file /tmp/index.html on same server; copy this file to the repo and add/commit to master branch.


c. Finally push master branch to this new remote origin.
Resolución:
```bash
ssh natasha@ststor01
cd /usr/src/kodekloudrepos/news
git remote add dev_news /opt/xfusioncorp_news.git
cp /tmp/index.html .
git add index.html
git commit -m "Add index.html file"
git push dev_news master
```
