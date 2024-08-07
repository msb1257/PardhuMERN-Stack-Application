# MERN Stack Project with DevOps Principles
## Introduction
Welcome to our open source MERN stack project! This repository serves as an educational resource for developers who want to learn how to deploy a three-tier application to **PRODUCTION** using DevOps principles. The project leverages a modern technology stack and architecture, consisting of Next.js for the frontend, Node.js for the backend, and MongoDB for the database. 
As an open source project, we encourage anyone to work on and contribute to the project. Whether you are a DevOps engineer, a full-stack developer, or someone looking to learn more about deploying a MERN stack application, you will find opportunities to gain hands-on experience and make meaningful contributions.

### Setting up the Backend
1. **Fork and Clone the Repository**
   ```bash
   git clone https://github.com/amigo-nishant/MERN-Stack-Aplication.git
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
git clone https://github.com/amigo-nishant/Capstone-Project-LAB.git
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
3. vim .env.sample
```bash
change localhost:5000 --> public-IP:5000
```
4. Configure Environment Variables
```bash
cp .env.sample .env.local
```
5. Start your service 
```bash
npm run dev -- --host
```
> Now you can navigate to your frontend --> public-IP:5173
> Frontend / Backend / MongoDB should be connected and deployed on AWS using Virtualization
## LAB-2: Understanding Dockerization and Deployment with Docker Compose on EC2 instance
This lab focuses on Dockerization, where you will learn how to containerize applications using Docker and deploy them using Docker Compose. You'll understand the benefits of containerization and how to manage multi-container applications efficiently.

### Setup Dockerfile for our backend 

1. Install Docker -- https://docs.docker.com/engine/install/ubuntu/
2. Add user to the docker group - sudo usermod -aG docker $USER [ exit and ssh back to to apply the changes. This step is important as it refreshes your session and grants your user the new group permissions]
3. **NOTE** If you are triggering the shell script to provision the cluster then no need to install docker manually
 
4. Create a Dockerfile for your Backend [Reference --> README.md file for Backend]
```bash
FROM node:21
WORKDIR /app
COPY . .
RUN npm i
COPY .env.docker .env
EXPOSE 5000
CMD ["npm", "start"]
```
5. Now from the Dockerfile let us build an image
```bash
docker build -t backend .
```
6. We also need mongodb database for our backend service so let's build a mongo container
```bash
docker run -d -p 27017:27017 --name mongo mongo:latest
```
7. Check the status of docker containers
```bash
docker ps
```
8. To validate mongo db is running inside the container
```bash
docker exec -it <container-id> /bin/bash
mongosh
```
9. Run the backend container
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
COPY .env.docker .env.local
EXPOSE 5173
CMD ["npm", "run", "dev", "--", "--host"]
```
2. vim .env.docker
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

Run the command to start all the services
```bash
docker compose up -d --build
```
If you want to import some data  
```bash
docker ps
docker exec -it <mongo-container-ID> /bin/bash
docker exec -it mongo mongoimport --db wanderlust --collection posts --file ./data/sample_posts.json --jsonArray
```
Stop all the services
```bash
docker compose down
```

### Debugging Notes
> Check frontend .env.sample file and update the connection URL
> If you are building the image make sure to import the data
> Check developer tools console log to identify the API calls and errors
> Check the backend logs if it is connecting to mongo
## Deployment via custom kubernetes manifest files 
1. Check the cluster configs and install minikube cluster using the shell script
```bash
kubectl config get-contexts
```
2. Be on the project root directory
```bash
ls
Jenkinsfile		README.md		docker-compose.yml	helm			package-lock.json
LICENSE			backend			frontend		kubernetes		package.json 
```
3. Deploy the files
```bash
kubectl create -f kubernetes/
```
4. Check the configs
```bash
kubectl get all
```
5. Get the URL for the service running in minikube VM
```bash
minikube service frontend --url
```
6. Test the app locally on minikube VM
```bash
curl <URL>
```
7. Now to test the application outside the cluster via port-forwarding
```bash
kubectl get svc
kubectl port-forward svc/frontend NodePort-port:5173 --address 0.0.0.0 &
```
8. Now to test on browser 
```bash
http:PublicIP:NodePort
```
9. Delete the deployment configs
```bash
kubectl delete -f kubernetes/
```
## PROD Deployment via custom Helm Charts
1. Install minikube cluster on a t2.large instance and navigate to the directoy having helm configs
```bash
cd helm
```
2. Before deployment check the configs using dry-run 
```bash
helm upgrade --install wanderlast . --dry-run --debug
```
3. Deploy the helm chart
```bash
helm upgrade --install wanderlast .
```
4. Check the deployment
```bash
helm ls
```
5. Check the deployment status
```bash
kubectl get all
```
6. Get the URL for the service running in minikube VM
```bash
minikube service wanderlast-helm-frontend --url
```
7. Test the app locally on minikube VM
```bash
curl <URL>
```
8. Now to test the application outside the cluster via port-forwarding
```bash
kubectl get svc
kubectl port-forward svc/wanderlast-helm-frontend NodePort-port:5173 --address 0.0.0.0 &
```
9. Now to test on browser 
```bash
http://PublicIP:NodePort
```
10. Delete the deployment
```bash
helm uninstall wanderlast .
```
## Observability Setup and Monitoring
1. Add the prometheus community maintained helm chart
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```
2. helm repo update
```bash
helm repo update
```
3. Test prometheus using dry-run and validate the chart configs
```bash
helm install prometheus prometheus-community/prometheus --dry-run --debug
```
4. Deploy prometheus after validation
```bash
helm install prometheus prometheus-community/prometheus
```
5. Expose the prometheus service to access prometheus-server using the browser.
```bash
kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-ext
```
6. Test the service for prometheus-server-ext
```bash
kubectl get svc
kubectl port-forward svc/prometheus-server-ext NodePort:80 --address 0.0.0.0 &
```
7. Navigate to browser --> http:PublicIP:NodePort
8. Add the Grafana community maintained helm chart
```bash
helm repo add grafana https://grafana.github.io/helm-charts
```
9. Update the repo
```bash
helm repo update
```
10. Test Grafana using dry-run and validate the chart configs
```bash
helm install grafana grafana/grafana --dry-run --debug
```
11. Deploy Grafana  after validation
```bash
helm install grafana grafana/grafana
```
12. Expose the Grafana service to access Grafana using the browser.
```bash
kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-ext
```
13. Test the service for grafana-ext
```bash
kubectl get svc
kubectl port-forward svc/grafana-ext NodePort:80 --address 0.0.0.0 &
```
14. Navigate to browser --> http://PublicIP:NodePort
15. Get your 'admin' user password by running:
```bash
kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
16. Setup datasorce in Grafana dashboard
    1. Go to Home --> Add Datasource --> Prometheus
    2. In Datasources we need the connection string for our prometheus-server
    ```bash
    kubectl get svc prometheus-server
    NAME                TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
    prometheus-server   ClusterIP   10.98.12.207   <none>        80/TCP    19m
    ```
    3.  Now the Connection URL should be --> http://prometheus-server-cluster-IP:Port --> Eg: http://10.98.12.207:80
    4.  Once Datasorce is connected successfully we can create Dashboard to visualize the charts
    5.  Go to dashboards and import dashboard ID - 3662
    
### To Bring down all the services after the lab
1. Bring down Application pods
```bash
helm uninstall wanderlast .
```
2. Scale down all grafana pods and services
```bash
helm uninstall grafana grafana/grafana
```
3. Lastly scale down all prometheus pods services
```bash
helm uninstall prometheus prometheus-community/prometheus
```
