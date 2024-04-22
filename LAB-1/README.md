## MERN Stack Project with DevOps Principles

1. Welcome to our open source **MERN** stack project! This repository serves as an educational resource for developers who want to learn how to deploy a three-tier application to **PRODUCTION** using DevOps principles. The project leverages a modern technology stack and architecture, consisting of Next.js for the frontend, Node.js for the backend, and MongoDB for the database.

2. As an open source project, we encourage anyone to work on and contribute to the project. Whether you are a DevOps engineer, a full-stack developer, or someone looking to learn more about deploying a MERN stack application, you will find opportunities to gain hands-on experience and make meaningful contributions.

3. The project uses a modern DevOps pipeline that leverages the following tools and practices:
Code Repository: The project code is stored in a Git repository, providing a centralized location for version control and collaboration.

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
