# MEAN-Stack-Deployment-to-Ubuntu-in-AWS


## Introduction
This project focuses on deploying a MEAN (MongoDB, Express.js, Angular, Node.js) stack on an Ubuntu server in AWS, enabling a scalable and secure cloud-based web application. The MEAN stack is a widely adopted JavaScript-based framework that provides a seamless development experience by using a single programming language across the entire application. This full-stack architecture is well-suited for building dynamic, data-driven applications, offering high flexibility, real-time capabilities, and efficient performance.
By leveraging AWS cloud infrastructure, this deployment ensures high availability, scalability, and security while optimizing server resources. The combination of MongoDB’s NoSQL database, Express.js for backend development, Angular for frontend interactivity, and Node.js for server-side execution makes the MEAN stack a powerful choice for modern web applications.

## Understanding the MEAN Stack
- **MongoDB**: A NoSQL database for storing data in JSON-like documents.
- **Express.js**: A lightweight backend framework for building web applications and APIs.
- **Angular**: A frontend framework for creating dynamic and interactive web applications.
- **Node.js**: A JavaScript runtime for executing server-side applications efficiently.

## Project Objectives
1. To deploy a Fully Functional MEAN Stack Application on AWS.
2. To install and configure MEAN Stack Components.
3. To develop a RESTful API for CRUD Operations (POST, GET, PUT, and DELETE).
4. To test and deploy the Application for Public Access.

## Project Steps

### Step 1: Spin up an Ubuntu server and SSH into the server
- Go to your AWS Console and search for EC2 instance.
- Click Launch instance -> Give Instance a name -> Select ubuntu as your Application and OS image -> select t2.micro as your instance type -> create a new key pair -> click launch instance.
- SSH into the instance using the command: 
  ```
  ssh -i {your_key.pem} ubuntu@{your_instance_public_ip}
  ```

### Step 2: Install Node.js
1. Update your terminal:
   ```
   sudo apt update
   ```
2. Install curl if not already installed:
   ```
   sudo apt install curl
   ```
3. Install Node.js:
   ```
   curl -sL https://deb.nodesource.com/setup_lts.x | sudo -E bash –
   sudo apt install nodejs
   ```
4. Verify Node.js installation:
   ```
   node -v
   ```

### Step 3: Install MongoDB
1. Install MongoDB:
   ```
   sudo apt-get install -y mongodb-org
   ```
2. Start, enable, and check the status of MongoDB:
   ```
   sudo systemctl start mongod
   sudo systemctl enable mongod
   sudo systemctl status mongod
   ```

### Step 4: Install Express and Set Up Routes to the Server
1. Install Express:
   ```
   sudo npm install express mongoose
   ```
2. Create necessary folders and files for routes and models.
3. Add the web server code and routes.

### Step 5: Access the Routes with Angular.js
1. Set up the Angular.js frontend to communicate with the server and perform CRUD operations.
2. Access the app from a browser by visiting:
   ```
   http://{ip_address}/3300
   ```

## Conclusion
This project successfully deployed a MEAN stack application on an Ubuntu server in AWS, leveraging MongoDB, Express.js, Angular, and Node.js to create a scalable, dynamic web app. The setup involved installing and configuring the necessary components, developing a RESTful API for CRUD operations, and making the app publicly accessible. Using AWS ensures high availability and security, making this deployment suitable for real-world applications. Overall, the project achieved its goals of deploying a fully functional MEAN stack app in the cloud, providing a strong foundation for future development.
