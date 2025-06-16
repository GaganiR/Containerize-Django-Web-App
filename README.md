
# Containerized Python Web App on AWS EC2 using Docker
Deploying a Django-based web application on an AWS EC2 instance by containerizing it with Docker.
## Prerequisites
> We need an EC2 instance deployed.

> Allow inbound traffic on TCP port 8000.

> Install docker in EC2 and add your ubuntu user to the docker group.

## Dockerfile
Below is the Dockerfile used for containerizing this web app.
```
FROM ubuntu

WORKDIR /app

COPY requirements.txt /app/
COPY devops /app/

RUN apt-get update && apt-get install -y python3 python3-pip python3-venv

SHELL ["/bin/bash", "-c"]

RUN python3 -m venv venv1 && \
source venv1/bin/activate && \
pip install --no-cache-dir -r requirements.txt

EXPOSE 8000

CMD source venv1/bin/activate && python3 manage.py runserver 0.0.0.0:8000
```
> The base used is Ubuntu. This tells Docker to use the latest official Ubuntu image as the starting point for the container.

> WORKDIR is an indication of where our source code is going to be. Sets the working directory inside the container to /app

> If youre containerizing pythons apps, the first thing that you need to copy inside that workdir is the requirements file. Because requirements.txt contains your python dependencies that are required to run your app. In my case, the requirements.txt lists Django and tzdata

> Next you need to copy the source code of the app into the workdir.

> Next Python 3, pip, and virtual environment tools are installed.

> Change the default shell used by RUN commands to bash. Otherwise sh is set by default.

> Then it creates a Python virtual environment named venv1, activates it and installs the packages listed in requirements.txt.

> State Docker that the container will listen on port 8000.

> Activate the virtual environment and launche the server on 0.0.0.0:8000 to make it accessible from outside the container.

## Steps
1. Clone this repo into your instance and move in to the python-web-app folder.
![clone](https://github.com/user-attachments/assets/a973bfb4-c048-4a40-878c-600b86e8951a)
2. Build the docker image from the Dockerfile
```
docker build .
```
3. Verify image was built using:
```
docker images
```
![dimage](https://github.com/user-attachments/assets/ea7d228d-2ac9-4fb2-8c89-4fc9ba446471)
4. Run the container
```
docker run -p 8000:8000 -it dockerimagename
```
We map the port 8000 on the container to host. 
![drun](https://github.com/user-attachments/assets/3c63a166-9c11-4f3a-8157-c0c90d337c96)
5. Now on your browser, access the instance through the port and project name.
```
ip-address:8000/demo/
```
![webapp](https://github.com/user-attachments/assets/b05d37e3-cd32-469c-97d8-4ca5ee70d273)
This should display the web app like so.





