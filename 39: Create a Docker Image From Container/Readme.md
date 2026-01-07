Day 39: Create a Docker Image From Container

Enunciado: 
a. Create an image games:devops on Application Server 2 from a container ubuntu_latest that is running on same server.

Resolución:
```bash
ssh steve@stapp02
docker commit ubuntu_latest games:devops
docker image ls
```

### Docker Commit 
- Casos de usos donde es útil:
  - Crear una imagen a partir de un contenedor modificado. EJ: a partir de un contenedor, se instalan paquetes o se hacen configuraciones y se quiere ir guardando snapshots.
  - Guardar el estado actual de un contenedor para reutilizarlo más tarde. EJ: Debug / forensics, se quiere guardar el estado de un contenedor para analizarlo posteriormente, ver los logs de un determinado momento, binarios, reproducir un error, etc.
  - Crear imágenes personalizadas para entornos específicos.