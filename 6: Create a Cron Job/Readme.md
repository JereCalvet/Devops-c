Day 6: Create a Cron Job

Crear un cron job

a. Install cronie package on all Nautilus app servers and start crond service.
```bash
sudo dnf install cronie -y
sudo systemctl start crond
```

b. Add a cron */5 * * * * echo hello > /tmp/cron_text for root user.
```bash
sudo crontab -e
# Add the line:
*/5 * * * * echo hello > /tmp/cron_text

```bash
sudo crontab -l
# You should see the line you just added:
*/5 * * * * echo hello > /tmp/cron_text
```