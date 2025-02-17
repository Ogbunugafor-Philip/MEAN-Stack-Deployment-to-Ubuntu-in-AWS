
# MEAN Stack Deployment to Ubuntu in AWS

## Introduction
This project focuses on deploying a MEAN (MongoDB, Express.js, Angular, Node.js) stack on an Ubuntu server in AWS, enabling a scalable and secure cloud-based web application. The MEAN stack is a widely adopted JavaScript-based framework that provides a seamless development experience by using a single programming language across the entire application. This full-stack architecture is well-suited for building dynamic, data-driven applications, offering high flexibility, real-time capabilities, and efficient data management.

By leveraging AWS cloud infrastructure, this deployment ensures high availability, scalability, and security while optimizing server resources. The combination of MongoDB’s NoSQL database, Express.js for backend development, Angular for frontend interactivity, and Node.js for server-side execution makes the MEAN stack a powerful choice for modern web applications.

## Understanding the MEAN Stack
- **MongoDB** – A NoSQL database for storing data in JSON-like documents.
- **Express.js** – A lightweight backend framework for building web applications and APIs.
- **Angular** – A frontend framework for creating dynamic and interactive web applications.
- **Node.js** – A JavaScript runtime for executing server-side applications efficiently.

## Project Objectives
1. To deploy a Fully Functional MEAN Stack Application on AWS.
2. To install and configure MEAN Stack Components.
3. To develop a RESTful API for CRUD Operations (POST, GET, PUT and DELETE).
4. To test and deploy the Application for Public Access.

## Project Steps

### Step 1: Spin up an Ubuntu server and SSH into the server
Ubuntu Server is an open-source Linux operating system based on Debian, designed specifically for server environments. It is widely used for web servers, database servers, cloud services, and more.

1. Go to your AWS Console and search for EC2 instance and click on it
2. Click Launch instance -> Give Instance a name -> Select ubuntu as your Application and OS image -> select t2.micro as your instance type -> create a new key pair -> click launch instance
3. Next, we would need to SSH (Secure Shell) into our newly created instance from our local host. To achieve this:
   - Open your git bash and run this command to go to the directory where you save your key pair:
     ```bash
     cd Downloads
     ```
   - Change mode of your key pair. We always do this to enhance security. We would use 400 which means:
     - 4 (read permission for the owner)
     - 0 (no permissions for the group)
     - 0 (no permissions for others)
     - To change the mode of our keypair, run the command:
       ```bash
       chmod 400 "Devops.pem"
       ```
   - To SSH into our instance, run the command in this format:
     ```bash
     ssh -i {your_key.pem} ubuntu@{your_instance_public_ip}
     ```
     The command would look like this:
     ```bash
     ssh -i Devops.pem ubuntu@54.159.121.163
     ```

### Step 2: Install Node.js
Node.js is a powerful, open-source JavaScript runtime built on Chrome's V8 engine. It's primarily used for building scalable network applications, especially web servers and real-time applications like chat apps, APIs, and more.

1. Update your terminal. Run:
   ```bash
   sudo apt update
   ```
![Image](https://github.com/user-attachments/assets/32301701-a2cc-4131-a0d3-4c77f7259501)
2. Install curl if not already installed. Curl is a command-line tool used to transfer data to or from a server using various protocols like HTTP, HTTPS, FTP, etc. Run:
   ```bash
   sudo apt install curl
   ```
![Image](https://github.com/user-attachments/assets/1a3ef383-832a-4c74-8e47-525fd1ec6db5)
3. Use the following command to fetch and set up the NodeSource repository. NodeSource repository is a trusted external package repository that allows you to install and update Node.js on your system more easily and ensures you get the latest stable versions of Node.js:
   ```bash
   curl -sL https://deb.nodesource.com/setup_lts.x | sudo -E bash –
   ```
4. Install Node.js:
   ```bash
   sudo apt install nodejs
   ```
5. Verify node.js is installed correctly. Run:
   ```bash
   node -v
   ```
![Image](https://github.com/user-attachments/assets/c4ae2c5c-fd61-43c0-a8a6-160fb2a49dc2)

### Step 3: Install MongoDB
MongoDB is a NoSQL database that stores data in flexible, JSON-like documents, allowing for dynamic schemas and scalable, high-performance data storage and retrieval.

For our example application, we are adding book records to MongoDB that contain book name, ISBN number, author, and number of pages.

1. Add the MongoDB repository:
   ```bash
   sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
![Image](https://github.com/user-attachments/assets/7e6df88f-4955-4d4a-8143-99dafb43d0b0)

   echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
   ```
2. Install MongoDB:
   ```bash
   sudo apt-get install -y mongodb-org
   ```
3. Start, enable and check the status of MongoDB:
   ```bash
   sudo systemctl start mongod
   sudo systemctl enable mongod
   sudo systemctl status mongod
   ```
   ![Image](https://github.com/user-attachments/assets/853a1569-6fb7-49bc-ab06-ddde72294fd6)
4. We need ‘body-parser’ package to help us process JSON files passed in requests to the server:
   ```bash
   sudo npm install body-parser
   ```
![Image](https://github.com/user-attachments/assets/eea4ecee-0f6e-4a5d-b5a0-b408c4950423)
5. Create a folder named ‘Books’ and change directory into the Books folder:
   ```bash
   mkdir Books
   cd Books
   ```
6. Initialize npm project. Run:
   ```bash
   npm init
   ```
7. Add a file to it named server.js:
   ```bash
   touch server.js
   ```
8. Copy and paste the web server code below into the server.js file using sudo vi server.js:
   ```javascript
   var express = require('express');
   var bodyParser = require('body-parser');
   var app = express();
   app.use(express.static(__dirname + '/public'));
   app.use(bodyParser.json());
   require('./apps/routes')(app);
   app.set('port', 3300);
   app.listen(app.get('port'), function() {
       console.log('Server up: http://localhost:' + app.get('port'));
   });
   ```

### Step 4: Install Express and Set Up Routes to the Server
Express.js is a back-end web application framework for Node.js that simplifies building web servers and APIs. It provides a lightweight and flexible way to handle HTTP requests, manage middleware, and define routes.

1. To install express, run:
   ```bash
   sudo npm install express mongoose
   ```
2. In the book folder, create a folder named app. In the app folder, create a file named routes.js:
   ```bash
   mkdir app
   cd app
   touch routes.js
   ```
3. Open the routes.js file so you can paste some items. Run:
   ```bash
   sudo vi routes.js
   ```
4. Paste the below in the routes.js file:
   ```javascript
   var Book = require('./models/book');
   module.exports = function(app) {
     app.get('/book', function(req, res) {
       Book.find({}, function(err, result) {
         if ( err ) throw err;
         res.json(result);
       });
     });
     app.post('/book', function(req, res) {
       var book = new Book( {
         name:req.body.name,
         isbn:req.body.isbn,
         author:req.body.author,
         pages:req.body.pages
       });
       book.save(function(err, result) {
         if ( err ) throw err;
         res.json( {
           message:"Successfully added book",
           book:result
         });
       });
     });
     app.delete("/book/:isbn", function(req, res) {
       Book.findOneAndRemove(req.query, function(err, result) {
         if ( err ) throw err;
         res.json( {
           message: "Successfully deleted the book",
           book: result
         });
       });
     });
     var path = require('path');
     app.get('*', function(req, res) {
       res.sendFile(path.join(__dirname + '/public', 'index.html'));
     });
   };
   ```
5. In the app folder, create a folder named models. In the models folder, create a file named book.js:
   ```bash
   mkdir models
   cd models
   touch book.js
   ```
6. Paste the below in the book.js file:
   ```javascript
   var mongoose = require('mongoose');
   var dbHost = 'mongodb://localhost:27017/test';
   mongoose.connect(dbHost);
   mongoose.connection;
   mongoose.set('debug', true);
   var bookSchema = mongoose.Schema( {
     name: String,
     isbn: {type: String, index: true},
     author: String,
     pages: Number
   });
   var Book = mongoose.model('Book', bookSchema);
   module.exports = mongoose.model('Book', bookSchema);
   ```

### Step 5: Access the routes with Angular.js
1. Go back to the apps Directory. In that directory, create a directory called public. In the public directory, create a file named script.js:
   ```bash
   cd ..
   mkdir public
   cd public
   touch script.js
   ```
2. Paste the below in the script.js file:
   ```javascript
   var app = angular.module('myApp', []);
   app.controller('myCtrl', function($scope, $http) {
     $http( {
       method: 'GET',
       url: '/book'
     }).then(function successCallback(response) {
       $scope.books = response.data;
     }, function errorCallback(response) {
       console.log('Error: ' + response);
     });
     $scope.del_book = function(book) {
       $http( {
         method: 'DELETE',
         url: '/book/:isbn',
         params: {'isbn': book.isbn}
       }).then(function successCallback(response) {
         console.log(response);
       }, function errorCallback(response) {
         console.log('Error: ' + response);
       });
     };
     $scope.add_book = function() {
       var body = '{ "name": "' + $scope.Name + 
       '", "isbn": "' + $scope.Isbn +
       '", "author": "' + $scope.Author + 
       '", "pages": "' + $scope.Pages + '" }';
       $http({
         method: 'POST',
         url: '/book',
         data: body
       }).then(function successCallback(response) {
         console.log(response);
       }, function errorCallback(response) {
         console.log('Error: ' + response);
       });
     };
   });
   ```
![Image](https://github.com/user-attachments/assets/15bbc09f-360b-49c6-ab0b-ba3f726a85fc)
3. In public folder, create a file named index.html and paste the below code:
   ```html
   <!doctype html>
   <html ng-app="myApp" ng-controller="myCtrl">
     <head>
       <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
       <script src="script.js"></script>
     </head>
     <body>
       <div>
         <table>
           <tr>
             <td>Name:</td>
             <td><input type="text" ng-model="Name"></td>
           </tr>
           <tr>
             <td>Isbn:</td>
             <td><input type="text" ng-model="Isbn"></td>
           </tr>
           <tr>
             <td>Author:</td>
             <td><input type="text" ng-model="Author"></td>
           </tr>
           <tr>
             <td>Pages:</td>
             <td><input type="number" ng-model="Pages"></td>
           </tr>
         </table>
         <button ng-click="add_book()">Add</button>
       </div>
       <hr>
       <div>
         <table>
           <tr>
             <th>Name</th>
             <th>Isbn</th>
             <th>Author</th>
             <th>Pages</th>
           </tr>
           <tr ng-repeat="book in books">
             <td>{{book.name}}</td>
             <td>{{book.isbn}}</td>
             <td>{{book.author}}</td>
             <td>{{book.pages}}</td>
             <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
           </tr>
         </table>
       </div>
     </body>
   </html>
   ```
4. Change the directory back up to Books and start the server:
   ```bash
   node server.js
   ```
![Image](https://github.com/user-attachments/assets/e2ebacbe-3d26-4225-8dd2-d505bc6ccd24)

![Image](https://github.com/user-attachments/assets/fd157e90-6d92-487b-a757-555de0b51d2d)

### Conclusion
The project demonstrates a full-stack web application where users can perform CRUD operations on books. Deploying the MEAN stack on AWS offers scalability, availability, and easy access to resources for real-world applications.

