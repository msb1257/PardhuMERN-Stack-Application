## LAB-1: Understanding Virtualization and Deploying on AWS
In this lab, we will delve into the concept of virtualization and deploy our three-tier project on Amazon Web Services (AWS). You will gain hands-on experience with setting up virtualized environments and deploying applications in the cloud.

## Virtualization

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
