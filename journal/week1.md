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
 <p>We created a Dockerfile in `backend-flask/Dockerfile` using the commands below, these commands are instructions on how to build our Docker Image. When we execute the Dockerfile, this file will build our image.</p>
 
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
The following will setup flask and allow us to access our backend on port 4567, we also have to set environmental variables `export FRONTEND_URL = "*"` and `export BACKEND_URL = "*"` so we can access our frontend and backend. You can check if environmental variables are set by running `env | grep BACKEND` or  `env | grep FRONTEND`.

```sh
cd backend-flask
export FRONTEND_URL="*"
export BACKEND_URL="*"
python3 -m flask run --host=0.0.0.0 --port=4567
cd ..
```
### Building a Container
<p>We can build container using the following command below</p>

 ```sh
docker build -t  backend-flask ./backend-flask
```

### Run Container 
```sh
docker run --rm -p 4567:4567 -it backend-flask
FRONTEND_URL="*" BACKEND_URL="*" docker run --rm -p 4567:4567 -it backend-flask
export FRONTEND_URL="*"
export BACKEND_URL="*"
docker run --rm -p 4567:4567 -it -e FRONTEND_URL='*' -e BACKEND_URL='*' backend-flask
docker run --rm -p 4567:4567 -it  -e FRONTEND_URL -e BACKEND_URL backend-flask
unset FRONTEND_URL="*"
unset BACKEND_URL="*"
```
 We want to use `docker run --rm -p 4567:4567 -it -e FRONTEND_URL='*' -e BACKEND_URL='*' backend-flask` this will set the env variables  when the contain is running and allow us to access our frontend and backend 
 
 Another way to run our dockerfile we can right click on the `docker-compose.yml` file then click Compose up
 ![backend](https://user-images.githubusercontent.com/46639580/221342233-76efb02b-465c-4abd-a783-75772532f27d.png)
 
 ### Frontend Container 
 
 Similiarly we do the same above for your Frontend
 
 We create a Docker File

```dockerfile
FROM node:16.18

ENV PORT=3000

COPY . /frontend-react-js
WORKDIR /frontend-react-js
RUN npm install
EXPOSE ${PORT}
CMD ["npm", "start"]
```

Build the Container
```sh
docker build -t frontend-react-js ./frontend-react-js
```
Run the Container 
```sh
docker run -p 3000:3000 -d frontend-react-js
```
![frontend](https://user-images.githubusercontent.com/46639580/221342529-f5af3f29-4eff-4a72-99da-ba3ca8519111.png)

Ultimately we can configure a `docker-compose.yml` that will allow us to run the backend and frontend container at the same time.

```
version: "3.8"
services:
  backend-flask:
    environment:
      FRONTEND_URL: "https://3000-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
      BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./backend-flask
    ports:
      - "4567:4567"
    volumes:
      - ./backend-flask:/backend-flask
  frontend-react-js:
    environment:
      REACT_APP_BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./frontend-react-js
    ports:
      - "3000:3000"
    volumes:
      - ./frontend-react-js:/frontend-react-js
  dynamodb-local:
    # https://stackoverflow.com/questions/67533058/persist-local-dynamodb-data-in-volumes-lack-permission-unable-to-open-databa
    # We needed to add user:root to get this working.
    user: root
    command: "-jar DynamoDBLocal.jar -sharedDb -dbPath ./data"
    image: "amazon/dynamodb-local:latest"
    container_name: dynamodb-local
    ports:
      - "8000:8000"
    volumes:
      - "./docker/dynamodb:/home/dynamodblocal/data"
    working_dir: /home/dynamodblocal
  db:
    image: postgres:13-alpine
    restart: always
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
    ports:
      - '5432:5432'
    volumes: 
      - db:/var/lib/postgresql/data
# the name flag is a hack to change the default prepend folder
# name when outputting the image names
networks: 
  internal-network:
    driver: bridge
    name: cruddur

volumes:
  db:
    driver: local
 ```

## Write a Flask Backend Endpoint for Notifications
![nonbackend](https://user-images.githubusercontent.com/46639580/221342882-a4fd38f2-46c5-4742-bbdf-c51aca74dc0b.png)

We were able to achieve this by: 
1) Creating an OpenAPI path for notifications called `api/activties/notifications:` (we setup the API similiar to the API for our home path)
2) Went into `app.py`and created service object for 'notifcations', next we added a app route for our notifcation API.
3) Created a `notifications_activities.py` and copy and pasted code from `home_activities` into new file.

## Write a React Page for Notifications
![frontendnon](https://user-images.githubusercontent.com/46639580/221343288-2d36197e-e028-40cc-8a23-b58f44dbaaae.png)
We were able to achieve this by: 
1) Created a new page in our `App.js` file called `NotificationsFeed` and created a Path for it
2) Created a new file called file called `NotificationsFeedPage.js` and file called `NotificationsFeedPage.css`, then copied and pasted content in `HomeFeedPage` to `NotificationsFeedPage.js`

 
## Run DynamoDB Local Container and ensure it works
Set up DynamoDB on Local Container using the commands in this repo: https://github.com/100DaysOfCloud/challenge-dynamodb-local

![image](https://user-images.githubusercontent.com/46639580/221343672-7ab2aff3-b684-4f97-afc7-84f3e86921e4.png)
![image](https://user-images.githubusercontent.com/46639580/221343651-c7baade5-0f56-4658-b330-c97df6850dc7.png)


## Run Postgres Container and ensure it works
Got Postgres running we had to add `- cweijan.vscode-postgresql-client2` to `.gitpod.yml`
![image](https://user-images.githubusercontent.com/46639580/221343866-e2596bed-612b-4823-bf06-932c5844639d.png)
