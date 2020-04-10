# Docker
## What is Docker?
— Docker is set of tools that help developers, ship their code from their development machine, to a production environment. 
— Docker simplifies the process of deployment by guaranteeing the developer that if it works locally on docker, it will work in production settings also.

Two major parts of the docker ecosystem are - 
		1. Docker image, built using a dockerfile
		2. Docker container 

Sequence lifecycle of a building a docker container
`dockerfile` -> `docker image` -> `docker container`

**Docker image**
A docker image is created from a set of rules / guidelines written in a _dockerfile_ .

Let us take a simple **Flask** application.
```python
# File structure
/ demo_flask_app
	venv/
	app.py # contains code to return Hello World!! 😃
	requirements.txt
	dockerfile # to show that dockerfile is contained in the same directory
```

A simple dockerfile, for the above case would look something like this

```dockerfile
# Fetching a pre-built image
FROM python:3.7-slim-stretch

# Setting a workdir, this a combination of mkdir + cd <dir_name>
WORKDIR /app

# Copy requirements.txt first, to help with caching
COPY requirements.txt .
RUN pip install -r requirements.txt

# Copy the rest of the project
COPY ./ .

# Expose a port where the server is listening
EXPOSE 5000

# Execute the script
CMD [“python”, “app.py”]
```

dockerfiles  are simple and self-explanatory in most cases, in the above example, we are :-

Importing from dockerhub  `python 3.7 pre-built image` -> Copying `demo_flask_app` directory into docker image -> Running `requirements.txt` to install packages in our container -> Exposing `PORT 5000` to be accessed outside of the docker container  -> Running our application.

After this step is complete, we can build a docker image from this dockerfile, one of the benefits of docker is that every step in the process is cached, so that every subsequent build from an existing docker image, is `lightning fast 💨 `.

## Why Docker?
Normal deployment - (of existing code)
— Git push

_manual_
— server ssh
— git pull
 


