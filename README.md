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
The base used is Ubuntu.

WORKDIR is an indication of where our source code is going to be. 

If youre containerizing pythons apps, the first thing that you need to copy inside that workdir is the requirements file. Because requirements.txt contains your python dependencies that are required to run your app. In my case, the requirements.txt lists Django and tzdata

Next you need to copy the source code of the app into the workdir.

Next Python is installed.



