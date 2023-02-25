# Week 1 — App Containerization


## Todo list
- [x] Watch weekly videos
- [x] Containerize Application (Dockerfiles, Docker Compose)
- [x] Write a Flask Backend Endpoint for Notifications
- [x] Write a React Page for Notifications
- [x] Run DynamoDB Local Container and ensure it works
- [x] Run Postgres Container and ensure it works

## Containerize Application (Dockerfiles, Docker Compose)
* This week we learned about Docker and how to create Docker file and Build a Docer container.
* Docker lets us containerizes our application so the application can be used across differnt hardware devices.

### Creaing Dockerfile
 <p>We created a Dockerfile in `backend-flask/Dockerfile` using the commands below. This is a list of commands that when we run this file will assemble our image.</p>
 
 ```dockerfile
FROM python:3.10-slim-buster

WORKDIR /backend-flask

COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt

COPY . .

ENV FLASK_ENV=development

EXPOSE ${PORT}
CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]
```




## Write a Flask Backend Endpoint for Notifications


## Write a React Page for Notifications

 
## Run DynamoDB Local Container and ensure it works



## Run Postgres Container and ensure it works

