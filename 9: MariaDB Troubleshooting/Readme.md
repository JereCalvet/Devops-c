Day 9: MariaDB Troubleshooting

El servicio no levantaba. 
Dec 18 14:29:17 stdb01.stratos.xfusioncorp.com mariadb-prepare-db-dir[2546]: Make sure the /var/lib/mysql is empt y before running mariadb-prepare-db-dir.

```bash
sudo rm -R /var/lib/mysqld
cd /var/lib
mkdir mysql
sudo chown mysql:mysql mysqld
sudo systemctl start mariadb
```