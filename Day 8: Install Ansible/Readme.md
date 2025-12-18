Day 8: Install Ansible

Install ansible version 4.9.0 on Jump host using pip3 only. Make sure Ansible binary is available globally on this system, i.e all users on this system are able to run Ansible commands.

```bash
sudo dnf install python3-pip -y
sudo pip3 install ansible==4.9.0

# Symlink
sudo ln -s /usr/local/bin/ansible /usr/bin/ansible
```
