Day 47: Docker Python App

Enunciado:
    Create a Dockerfile under /python_app directory:
        Use any python image as the base image.
        Install the dependencies using requirements.txt file.
        Expose the port 3002.
        Run the server.py script using CMD.

    Build an image named nautilus/python-app using this Dockerfile.

    Once image is built, create a container named pythonapp_nautilus:
        Map port 3002 of the container to the host port 8094.

    Once deployed, you can test the app using curl command on App Server 1.

Resolución:
```bash
ssh tony@stapp01
cd /python_app
sudo touch Dockerfile
sudo vi Dockerfile
# Contenido
# FROM python:3.9-slim
# WORKDIR /app
# COPY ./src .
# RUN pip install -r requirements.txt
# ENTRYPOINT ["python"]
# CMD ["server.py"]
# EXPOSE 3002
sudo docker build -t nautilus/python-app .
sudo docker run -d --name pythonapp_nautilus -p 8094:3002
curl http://localhost:8094
```