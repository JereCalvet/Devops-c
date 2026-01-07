Day 40: Docker EXEC Operations

Enunciado:
One of the Nautilus DevOps team members was working to configure services on a kkloud container that is running on App Server 2 in Stratos Datacenter. Due to some personal work he is on PTO for the rest of the week, but we need to finish his pending work ASAP. Please complete the remaining work as per details given below:

a. Install apache2 in kkloud container using apt that is running on App Server 2 in Stratos Datacenter.

b. Configure Apache to listen on port 5000 instead of default http port. Do not bind it to listen on specific IP or hostname only, i.e it should listen on localhost, 127.0.0.1, container ip, etc.

c. Make sure Apache service is up and running inside the container. Keep the container in running state at the end.

Resolución:
```bash
ssh steve@stapp02
docker exec -it kkloud bash
apt update
apt install apache2 -y

# Configuracion de Apache para escuchar en el puerto 5000
sed -i 's/Listen 80/Listen 5000/' /etc/apache2/ports.conf
# o 
nano /etc/apache2/ports.conf
# Cambiar la linea "Listen 80" por "Listen 5000"   

service apache2 start
curl http://localhost:5000
exit
docker ls
```