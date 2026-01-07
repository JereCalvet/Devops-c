Day 41: Write a Docker File

Enunciado:
Create a docker file /opt/docker/Dockerfile (please keep D capital of Dockerfile) on App server 2 in Stratos DC and configure to build an image with the following requirements:

a. Use ubuntu:24.04 as the base image.

b. Install apache2 and configure it to work on 8082 port. (do not update any other Apache configuration settings like document root etc).

Resolución:
```bash
ssh steve@stapp02
cd /opt/docker
touch Dockerfile
nano Dockerfile
# Contenido
FROM ubuntu:24.04       # Imagen base
RUN apt-get update \    # Actualizar repositorios porque sino no encuentra apache2
&& apt-get install -y apache2 \ # Instalar apache2
&& sed -i 's/Listen 80/Listen 8082/' /etc/apache2/ports.conf # Reemplazar el puerto 80 por el 8082
EXPOSE 8082              # Exponer el puerto 8082
ENTRYPOINT ["apache2ctl", "-D", "FOREGROUND"] # Mantener apache2 en primer plano
# Guardar y salir   
```

Notas:
apache necesita correr en primer plano para que el contenedor no se detenga inmediatamente después de iniciarse. Por eso se usa el ENTRYPOINT con "apache2ctl -D FOREGROUND". 

Apache2ctl es un script de control para el servidor Apache que permite iniciar, detener y reiniciar el servidor web Apache de manera sencilla. El flag "-D FOREGROUND" indica que Apache debe ejecutarse en primer plano, lo que es útil en entornos de contenedores como Docker, donde se desea que el proceso principal del contenedor permanezca activo.

Diferencias entre CMD, ENTRYPOINT y RUN en Dockerfile:
- RUN: Se utiliza para ejecutar comandos durante la construcción de la imagen Docker. Cada instrucción RUN crea una nueva capa en la imagen.
- CMD: Proporciona comandos predeterminados para ejecutar cuando se inicia un contenedor a partir de la imagen. Solo puede haber una instrucción CMD por Dockerfile.
- ENTRYPOINT: Configura un contenedor para que se ejecute como un ejecutable. Permite definir un comando fijo que siempre se ejecutará cuando se inicie el contenedor, y puede combinarse con CMD para proporcionar argumentos predeterminados.

Ejemplo: 

ENTRYPOINT ["executable", "param1", "param2"] (los parametros son fijos pero opcionales) <br>
CMD ["param3", "param4"] (parametros por defecto que pueden ser sobrescritos al iniciar el contenedor)

Docker run pisando CMD: <br>
docker run <image_name> param5 param6 <br>
En este caso, el contenedor se ejecutará como:<br>
executable param1 param2 param5 param6 <br>