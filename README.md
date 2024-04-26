# MERN Stack Project with DevOps Principles
## Introduction
Welcome to our open source MERN stack project! This repository serves as an educational resource for developers who want to learn how to deploy a three-tier application to **PRODUCTION** using DevOps principles. The project leverages a modern technology stack and architecture, consisting of Next.js for the frontend, Node.js for the backend, and MongoDB for the database. 
As an open source project, we encourage anyone to work on and contribute to the project. Whether you are a DevOps engineer, a full-stack developer, or someone looking to learn more about deploying a MERN stack application, you will find opportunities to gain hands-on experience and make meaningful contributions.
## LAB Details
1. **LAB-1** - Virtualization
2. **LAB-2** - Dockerfile and docker-compose
3. **LAB-3** - DevSecOps and CI/CD
## The project uses a modern DevOps pipeline that leverages the following tools and practices:
1. **Code Repository:** The project code is stored in a Git repository, providing a centralized location for version control and collaboration.
2. **Jenkins CI/CD:** Jenkins is used for continuous integration and continuous deployment (CI/CD). It automates the build, test, and deployment processes based on code changes and triggers.
3. **Docker & Docker Compose:** Docker containerizes the frontend and backend services, providing consistency across environments. Docker Compose simplifies local development by orchestrating Docker containers.
4. **Kubernetes:** Kubernetes manages the deployment, scaling, and maintenance of Docker containers in production environments, ensuring high availability and reliability.
5. **Ansible:** Ansible automates application and infrastructure updates, streamlining configurations and ensuring consistency across the deployment pipeline.
6. **Grafana & Prometheus:** Grafana and Prometheus work together to monitor the application and infrastructure. Prometheus collects metrics, and Grafana visualizes them, providing insights into the health and performance of the application.
### Setting up the Backend
1. **Fork and Clone the Repository**
   ```bash
   git clone https://github.com/amigo-nishant/Capstone-Project-LAB.git
   ```
2. **Navigate to the Backend Directory**
   ```bash
   cd backend
   ```
3. **Install Required Dependencies**
   ```bash
   npm i
   ```
4. **Set up your MongoDB Database**
   - Open MongoDB Compass and connect MongoDB locally at `mongodb://localhost:27017`.
5. **Import sample data**
   > To populate the database with sample posts, you can copy the content from the `backend/data/sample_posts.json` file and insert it as a document in the `wanderlust/posts` collection in your local MongoDB database using either MongoDB Compass or `mongoimport`.
   ```bash
   mongoimport --db wanderlust --collection posts --file ./data/sample_posts.json --jsonArray
   ```
6. **Configure Environment Variables**
   ```bash
   cp .env.sample .env
   ```
7. **Start the Backend Server**
   ```bash
   npm start
   ```
   > You should see the following on your terminal output on successful setup.
   >
   > ```bash
   > [BACKEND] Server is running on port 5000
   > [BACKEND] Database connected: mongodb://127.0.0.1/wanderlust
   > ```
### Setting up the Frontend
1. **Open a New Terminal**
   ```bash
   cd frontend
   ```
2. **Install Dependencies**
   ```bash
   npm i
   ```
3. **Configure Environment Variables**
   ```bash
   cp .env.sample .env.local
   ```
4. **Launch the Development Server**
   ```bash
   npm run dev
   ```

# DevOps Automation and Deployment LABs

## LAB-1: Understanding Virtualization and Deploying on AWS
In this lab, we will delve into the concept of virtualization and deploy our three-tier project on Amazon Web Services (AWS). You will gain hands-on experience with setting up virtualized environments and deploying applications in the cloud.

1. Launch an EC2 instance and connect via ssh
2. Make sure to enable SG frontend '5173' backend '5000'
3. Update the system
```bash
sudo apt-get update -y
```
4. Since it's a private repo, we need a Personal access token to clone the repo and then clone the git repository
```bash
git clone https://github.com/amigo-nishant/Capstone-project-clone-PROD.git
```
5. We need to install npm and nodejs for our project
```bash
https://nodejs.org/en/download/package-manager
```
6. If it says nvm not found, reboot your instance and you've to ssh again
```bash
sudo reboot
```

## Setting up the Backend with DB

7. Install mongodb database
```bash
https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/
```
8. Install dependencies
```bash
cd backend
npm i
```
9. start your mongodb service
```bash
sudo systemctl start mongod
sudo systemctl status mongod
```
10. To validate mongo installation
```bash
mongosh
exit
```
11. Import sample data
```bash
mongoimport --db wanderlust --collection posts --file ./data/sample_posts.json --jsonArray
```
12. Configure Environment Variables
```bash
cp .env.sample .env
```
13. Start the Backend Server
```bash
npm start
```

## Setting up the Frontend

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
## LAB-3: DevOps and DevSecOps tools:

### Tools Covered:
-  Linux
-  Git and GitHub
-  Docker
-  Docker-compose
-  Jenkins CI/CD
-  SonarQube Scan
-  SonarQube Quality Gates
-  Trivy 

## Pre-requisites to implement this project:

-  AWS EC2 instance (Ubuntu) with instance type t2.large and root volume 29GB.

-  Installation of JAVA
   ```bash
   sudo apt update
   sudo apt install fontconfig openjdk-17-jre
   java -version
   ```

-  Installation of Jenkins
   ```bash
   sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
   https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
   echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
   https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
   /etc/apt/sources.list.d/jenkins.list > /dev/null
   sudo apt-get update
   sudo apt-get install jenkins
   ```
-  Setup Jenkins
   ```bash
   Public-IP:8080 (Jenkins running)
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```
-  Docker and docker-compose installled
   ```bash
    sudo apt-get update
    sudo apt-get install docker.io -y
    sudo apt-get install docker-compose -y
    sudo usermod -aG docker $USER
    sudo reboot
   ```

-  Trivy installation
   ```bash
   sudo apt-get install wget apt-transport-https gnupg lsb-release
   wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
   echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
   sudo apt-get update
   sudo apt-get install trivy
   ```

- SonarQube Server installed
   ```bash
    docker run -itd --name sonarqube-server -p 9000:9000 sonarqube:lts-community
   ```
#
## Steps for Jenkins CI/CD:

1)  Access Jenkins UI and setup Jenkins

![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/1eec417e-95ab-4497-ad31-443ecd6b999e)

#

2)  Plugins Installation:

    - Go to <b><i><u>Manage Jenkins</u></i></b>, click on <b><i><u>Plugins</u></i></b> and install all the plugins listed below, we will require for other tools integration:

        - SonarQube Scanner (Version2.16.1)
        - Sonar Quality Gates (Version1.3.1)
        - Docker (Version1.5)
#

3) Go to SonarQube Server and create token

    - Click on <b><i><u> Administration </u></i></b> tab, then <b><i><u> Security </u></i></b>, then <b><i><u> Users </u></i></b> and create Token.
    -  Create a webhook to notify Jenkins that Quality gates scanning is done. (We will need this step later)

        - Go to SonarQube Server, then <b><i><u> Administration </u></i></b>, then <b><i><u> Configuration </u></i></b> and click on <b><i><u> Webhook </u></i></b>, add webhook in below <b>Format</b>:
        > http://<jenkins_url>:8080/sonarqube-webhook/
        
        Example: 

        ```bash
            http://34.207.58.19:8080/sonarqube-webhook/
        ```

        ![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/b9ef2301-b8ff-46f4-a457-6345d5e2dab6)


        ![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/08a33164-f6a6-4c5d-8a34-7091cf8a5745)

#

4) Go to Jenkins UI <b><i><u> Manage Jenkins </u></i></b>, then <b><i><u> Credentials </u></i></b> and add SonarQube Credentials.

![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/f6db72ec-7d8c-4f4c-ae7a-55d99dd20ce9)

#

5) Now, It's time to integrate SonarQube Server with Jenkins, go to <b><i><u> Manage Jenkins </u></i></b>, then <b><i><u> System </u></i></b> and look for <b><i><u> SonarQube Servers </u></i></b> and add SonarQube.

![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/54849cb2-fe56-4acd-972d-3057a0eb3deb)

#

6) Go to <b><i><u> Manage Jenkins </u></i></b>, then <b><i><u> tools </u></i></b>, look for <b><i><u> SonarQube Scanner installations </u></i></b> and add SonarQube Scanner.

> Note: Add name as ```Sonar```
![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/1fe926f6-a844-42d4-bce4-62193dde6640)

7) For trivy, we have already installed it, in pre-reuisites.

![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/0fcd1620-bd64-4286-bc13-f6652d4527c6)

#

8) Now, It's time to create a CI/CD pipeline in Jenkins:
    -  Click on <b><i><u>New Item</u></i></b> and give it a name and select <b><i><u>Pipeline</u></i></b>.
    -  Select <b><i><u>GitHub Project</u></i></b> and paste your GitHub repository link.
    -  Scroll down and in <b><i><u>Pipeline</u></i></b> section select <b><i><u>Pipeline script from SCM</u></i></b>, because our Jenkinsfile is present on GitHub.

    ![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/39af1b22-28aa-4e36-b98c-0e7f120b5fbf)

    ![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/b7153556-f847-40ee-9a98-ff3609930abd)
#

> Since our repository is private we need to create credentials for our repo. Click on Add Credentials --> Jenkins --> username as github usernmae and password as --> Personal access token

9) Make sure to change the .env.docker file for frontend so it can talk to backend container by updating the IP Address 

10) At last run the pipeline and after sometime your code is deployed using DevSecOps.

