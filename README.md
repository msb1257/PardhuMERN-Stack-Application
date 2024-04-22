# DevOps Bootcamp README
Welcome to our DevOps Bootcamp! We're excited to have you join us on this learning journey. By now, we assume you have a good understanding of Docker and Kubernetes. As the next step, we have outlined the following roadmap to further enhance your skills and knowledge in DevOps practices:

## Roadmap
**LAB-1**: Understanding Virtualization and Deploying on AWS
In this lab, we will delve into the concept of virtualization and deploy our three-tier project on Amazon Web Services (AWS). You will gain hands-on experience with setting up virtualized environments and deploying applications in the cloud.

**LAB-2**: Understanding Dockerization and Deployment with Docker Compose
This lab focuses on Dockerization, where you will learn how to containerize applications using Docker and deploy them using Docker Compose. You'll understand the benefits of containerization and how to manage multi-container applications efficiently.

**LAB-3**: Image Size Reduction and DevSecOps Deployment
In LAB-3, we'll explore techniques to reduce the size of Docker images and deploy them securely using DevSecOps practices. You'll learn about image optimization strategies and best practices for ensuring the security of your containerized applications.

**LAB-4**: Understanding Jenkins and CI/CD Pipeline with Docker Compose and Kubernetes
Jenkins and CI/CD pipelines play a crucial role in automating software delivery processes. In this lab, you will gain insights into Jenkins and set up a CI/CD pipeline for our project using Docker Compose and Kubernetes. You'll experience the benefits of continuous integration and continuous deployment firsthand.

**LAB-5**: Integrating Grafana and Prometheus
Monitoring and observability are essential aspects of DevOps. In LAB-5, we'll focus on integrating Grafana and Prometheus for monitoring our applications. You'll learn how to set up monitoring dashboards, visualize metrics, and gain insights into the performance of your infrastructure and applications.

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
