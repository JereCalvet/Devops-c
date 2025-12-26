Day 12: Linux Network Services

Enunciado de un servicio de red
```
Our monitoring tool has reported an issue in Stratos Datacenter. One of our app servers has an issue, as its Apache service is not reachable on port 3004 (which is the Apache port).

The service itself could be down, the firewall could be at fault, or something else could be causing the issue.

Use tools like telnet, netstat, etc. to find and fix the issue. Also make sure Apache is reachable from the jump host without compromising any security settings.

Once fixed, you can test the same using command: curl http://stapp01:3004

Note: Please do not try to alter the existing index.html code, as it will lead to task failure.
```

Resolución:
- Verifique si Apache esta corriendo en el server stapp01
```bash
systemctl status httpd
```
- El servicio no estaba corriendo, porque el puerto estaba ocupado por otro servicio
```bash
netstat -tulnp | grep 3004 
# Flags: 
# -t (TCP), -u (UDP), -l (listening), -n (numeric), -p (process)
# -l muestra solo los puertos en escucha
# -n muestra las direcciones y puertos en formato numérico
# -p muestra el PID y nombre del programa al que pertenece el socket
```
- Detuve el otro servicio y reinicie Apache
```bash
systemctl stop sendmail.service
systemctl restart httpd.service
```
- Respondondia curl local pero no desde jump host
```bash
curl http://stapp01:3004 #desde local -> OK
curl http://stapp01:3004 #desde jump host -> no route to host
sudo netstat -tulnp | grep 3004
LISTEN 0 128 *:3004 *:* users:(("httpd",pid=1234,fd=4))
```

- Verifique las reglas de firewall 
```bash
sudo iptables -L -n -v 
sudo iptables -I INPUT -p tcp --dport 3004 -j ACCEPT
```

Alternativa con firewalld
```bash
sudo firewall-cmd --list-ports
sudo firewall-cmd --permanent --add-port=3004/tcp
sudo firewall-cmd --reload
```

Notas sobre iptables:
```bash
-A INPUT: Agrega una regla al final de la cadena INPUT
-I INPUT: Inserta una regla al principio de la cadena INPUT
-D INPUT: Elimina una regla de la cadena INPUT
-F Limpia todas las reglas de la cadena especificada (o de todas si no se especifica ninguna)
-p tcp: Especifica el protocolo TCP
-s 172.16.123.4 # Dirección IP de origen (opcional)
-d # Dirección IP de destino (opcional)
--dport 3004: Especifica el puerto de destino 3004
-j # ACCEPT -> Acepta el paquete
   # DROP -> Descarta el paquete
   # REJECT -> Rechaza el paquete y envía una notificación al remitente

# Para listar
sudo iptables -L -n -v --line-numbers
# Flags
# -L: Lista las reglas
# -n: Muestra las direcciones y puertos en formato numérico
# -v: Muestra información detallada
# --line-numbers: Muestra el número de línea de cada regla

# Para eliminar una regla específica (por número de línea)
sudo iptables -D INPUT <número_de_línea>

# Para guardar las reglas de iptables (dependiendo de la distribución)
sudo dnf install iptables-services 
sudo service iptables save
sudo vi /etc/sysconfig/iptables
```

- Tareas programadas con at:
```bash
sudo iptables -I INPUT -p tcp --dport 3004 -j ACCEPT
echo "sudo iptables -D INPUT $(n)" | at now + 1 minute
x   
# cancelar tarea programada
sudo atrm <job_number>
```