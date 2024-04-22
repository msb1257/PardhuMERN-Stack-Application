## LAB-2: Understanding Dockerization and Deployment with Docker Compose
This lab focuses on Dockerization, where you will learn how to containerize applications using Docker and deploy them using Docker Compose. You'll understand the benefits of containerization and how to manage multi-container applications efficiently.

### Setting up the Frontend

1. Open a New Terminal
```bash
cd frontend
```
2. Install Dependencies
```bash
npm i
```
3. To start your frontend service with EC2 servive host
```bash
npm run dev -- --host
```
4. vim .env.sample
```bash
change localhost:5000 --> public-IP:5000
```
5. Configure Environment Variables
```bash
cp .env.sample .env.local
```
6. Restart your service 
```bash
npm run dev -- --host
```
> Now you can navigate to your frontend --> public-IP:5173
> Frontend / Backend / MongoDB should be connected and deployed on AWS using Virtualization

### Setup Dockerfile for our backend 

> Install Docker -- https://docs.docker.com/engine/install/ubuntu/
> Add user to the docker group - sudo usermod -aG docker $USER [ exit and ssh back to to apply the changes. This step is important as it refreshes your session and grants your user the new group permissions]
 
1. Create a Dockerfile for your Backend [Reference --> README.md file for Backend]
```bash
FROM node:21
WORKDIR /app
COPY . .
RUN npm i
COPY .env.sample .env
EXPOSE 5000
CMD ["npm", "start"]
```
2. Now from the Dockerfile let us build an image
```bash
docker build -t backend .
```
3. We also need mongodb database for our backend service so let's build a mongo container
```bash
docker run -d -p 27017:27017 --name mongo mongo:latest
```
4. Check the status of docker containers
```bash
docker ps
```
5. To validate mongo db is running inside the container
```bash
docker exec -it <container-id> /bin/bash
mongosh
```
6. Run the backend container
```bash
docker run -d -p 5000:5000 --name backend backend:latest
```

### Setup Dockerfile for our Frontend
1. Create a Dockerfile for your Frontend [Reference --> README.md file for Frontend]
```bash
FROM node:21
WORKDIR /app
COPY . .
RUN npm i
COPY .env.sample .env.local
EXPOSE 5173
CMD ["npm", "run", "dev", "--", "--host"]
```
2. vim .env.sample
```bash
change localhost:5000 --> public-IP:5000
```
3. Now from the Dockerfile let us build an image
```bash
docker build -t frontend .
```
4. Run the frontend container
```bash
docker run -d -p 5173:5173 --name frontend frontend:latest
```
5. Check the status of docker containers, all three containers should be up and running 
```bash
docker ps
docker rm -f <all-containers>
```
> But containers are isolated, we have to create a network or we can use docker-compose so that we can have all the three containers in one single stack and communicate with each other.

### docker-compose 

```bash
version: "3.8"
services:
  mongodb:
    container_name: mongo
    image: mongo:latest
    volumes:
      - ./backend/data:/data
    ports:
      - "27017:27017"

  backend:
    container_name: backend
    build: ./backend
    env_file:
      - ./backend/.env.sample
    ports:
      - "5000:5000"
    depends_on:
      - mongodb

  frontend:
    container_name: frontend
    build: ./frontend
    env_file:
      - ./backend/.env.sample
    ports:
      - "5173:5173"

volumes:
  data:
```
Check the mongo container 
```bash
docker ps
docker exec -it <mongo-container-ID> /bin/bash
docker exec -it mongo mongoimport --db wanderlust --collection posts --file ./data/sample_posts.json --jsonArray
```
Add connection string between frontend / backend and mongo 
```bash
cd /backend
vim .env.sample
CORS_ORIGIN="http://<instance-IP>:5173"
```
Run the command to start all the services
```bash
docker compose up -d --build
```

### Debugging Notes
> Check frontend .env.sample file and update the connection URL
> If you are building the image make sure to import the data
> Check developer tools console log to identify the API calls and errors
> Check the backend logs if it is connecting to mongo
