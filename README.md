# MERN Stack Project with DevOps Principles

## Introduction

Welcome to our open source MERN stack project! This repository serves as an educational resource for developers who want to learn how to deploy a three-tier application to **PRODUCTION** using DevOps principles. The project leverages a modern technology stack and architecture, consisting of Next.js for the frontend, Node.js for the backend, and MongoDB for the database. 

As an open source project, we encourage anyone to work on and contribute to the project. Whether you are a DevOps engineer, a full-stack developer, or someone looking to learn more about deploying a MERN stack application, you will find opportunities to gain hands-on experience and make meaningful contributions.

## LAB Details

1. **LAB-1** - Virtualization
2. **LAB-2** - Dockerfile and docker-compose
3. **LAB-3** - DevSecOps and CI/CD
4. **LAB-4** - Kubernetes and CI/CD

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
   git clone https://github.com/DevOps-Playbbok/Capstone-Project-LAB.git
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
