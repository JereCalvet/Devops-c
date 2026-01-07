Day 14: Linux Process Troubleshooting

The production support team of xFusionCorp Industries has deployed some of the latest monitoring tools to keep an eye on every service, application, etc. running on the systems. One of the monitoring systems reported about Apache service unavailability on one of the app servers in Stratos DC.


Identify the faulty app host and fix the issue. Make sure Apache service is up and running on all app hosts. They might not have hosted any code yet on these servers, so you don’t need to worry if Apache isn’t serving any pages. Just make sure the service is up and running. Also, make sure Apache is running on port 3003 on all app servers.

Resolución:
- Identificar el server con el problema
```bash
curl http://stapp01:3003 # OK
curl http://stapp02:3003 # No route to host
curl http://stapp03:3003 # ok
```

- Verificar si Apache esta corriendo en el server stapp02
```bash
systemctl status httpd
```
- El servicio no estaba corriendo, porque el puerto estaba ocupado por otro servicio
```bash
netstat -tulnp | grep 3003 
o 
# ss -tulnp | grep 3003
```
- Detuve el otro servicio y reinicie Apache
```bash
systemctl stop sendmail.service
systemctl restart httpd.service
```