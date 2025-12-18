Day 5: SElinux Installation and Configuration

*Install the required SELinux packages.
```bash
sudo dnf install selinux-policy selinux-policy-targeted policycoreutils-python-utils -y
```

*Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.
```bash
vi /etc/selinux/config
# Change the line:
SELINUX=enforcing
# To:
SELINUX=disabled
```

No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.

*Disregard the current status of SELinux via the command line; the final status after the reboot should be disabled.
```bash
sestatus
SELinux status:                 disabled
```

