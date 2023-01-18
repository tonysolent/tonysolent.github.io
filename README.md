## Summary
https://tonysolent.github.io/
This project is designed to make it easy for users to manage a database of items. It's a full-stack JavaScript application, which means that it uses JavaScript for both frontend and backend. The system comprises of a Node.js server that connects to MongoDB, which is a popular NoSQL database, and serves a simple HTML page with JavaScript frontend. The HTML page displays a list of items from the database and allows the user to add, edit, or delete items by making AJAX requests to the server.
The server is built using the Express framework and Mongoose library, which are both widely used in Node.js development. Express is a minimal web framework for Node.js and Mongoose is a MongoDB object modeling tool that is designed to work in an asynchronous environment. Together they make it easy to handle CRUD operations. One of the key features of Mongoose is its schema-based modeling, which allows developers to define the structure of a MongoDB collection by creating a schema that describes the fields and data types of the documents in the collection. This allows Mongoose to automatically perform type validation, casting, and other data manipulation on the documents before they are saved to the database.
Mongoose also provides a simple API for performing CRUD operations on the data, as well as advanced querying capabilities, middleware, and other features that can make it easier to work with MongoDB in a Node.js environment.
In addition to this, Mongoose also supports MongoDB's built-in functionality like indexing, searching and it also allows for easy data validation and sanitization, And also supports middleware that makes it easy to implement hooks for custom logic on specific events like saving and validating data.
The project stores the items in a single collection called "items." The collection stores each item's name, description, and price. The root route '/' handles an HTTP GET request and returns a list of all items in the database by calling the find method on the Item model with an empty query and a callback. The '/create' route handles an HTTP POST request and creates a new item in the database. And, '/update/:id' route handles an HTTP PUT request and updates an existing item in the database.
Security and scalability are two critical aspects of any application. To ensure the security of the application, the project has implemented measures such as using the MongoDB driver's built-in support for connection pooling, which prevents connection exhaustion attacks, and the use of the body-parser middleware, which parses the request bodies and protects against body parsing attacks. The project has put in place safeguards such leveraging connection pooling, which is integrated into the MongoDB driver and enables the server to manage a lot of concurrent connections, to ensure scalability and making use of the Node.js cluster module to build a network of worker processes, which enables the server to benefit from multi-core computers and enhance performance.
## Prerequisites
To use the application, the user must have Node.js and MongoDB installed. They must set up the project by creating a new directory, initializing the project with npm, and installing the necessary dependencies such as Express, Mongoose and others.
Before starting, make sure you have the following installed:
• Node.js
• MongoDB

### Step 1: Set up the project

Create a new directory for the project and navigate to it:
mkdir crud-app cd crud-app
Next, initialize the project with npm and create the package.json file:
npm init -y

### Step 2: Install the dependencies

Next, install the required dependencies. We is using the following packages:
• express: A web framework for Node.js
• mongoose: A MongoDB object modeling tool designed to work in an asynchronous environment
• body-parser: A middleware to parse request bodies
To install these dependencies, run the following command:
npm install express mongoose body-parser

### Step 3: Set up the database

Next, we need to set up the database. Make sure MongoDB is running on your machine.
In your project directory, create a new file called db.js and add the following code to it:
const mongoose = require('mongoose'); // Connect to the database mongoose.connect('mongodb://localhost/crud-app', { useNewUrlParser: true, useUnifiedTopology: true }); const db = mongoose.connection; // Handle any errors db.on('error', console.error.bind(console, 'connection error:')); // Once the connection is open, we can start performing CRUD operations db.once('open', function() { console.log('Connected to the database!'); });
This code connects to a MongoDB database called crud-app running on the local machine. If the connection is successful, it prints a message to the console.

### Step 4: Set up the server

Next, we set up the server. In the project directory, create a new file called server.js and add the following code to it:
const express = require('express'); const bodyParser = require('body-parser'); const db = require('./db'); // The database connection const app = express(); // Parse request bodies as JSON app.use(bodyParser.json()); // Set up the routes // The root route returns a list of all items in the database app.get('/', (req, res) => { res.send('Hello World!'); }); // Start the server const port = 3000; app.listen(port, () => { console.log(`Server listening on port ${port}`); });
This code sets up a simple Express server that listens on port 3000 and returns "Hello World!" when the root route is accessed.

### Step 5: Define the data model

Next, we need to define a data model for the items we want to store in the database.
Create a new file called item.js and add the following code to it:

const mongoose = require('mongoose');

const itemSchema = new mongoose.Schema({
name: String,
description: String,
price: Number
});

module.exports = mongoose.model('Item', itemSchema);

## This code defines a simple schema for items with a name, description, and price.

### Step 6: Set up the routes

Next, we set up the routes for the CRUD operations.
In server.js, import the Item model and add the following routes:
const express = require('express'); const bodyParser = require('body-parser'); const db = require('./db'); // The database connection const Item = require('./item'); // The item model const app = express(); // Parse request bodies as JSON app.use(bodyParser.json()); // Set up the routes // The root route returns a list of all items in the database app.get('/', (req, res) => { Item.find({}, (err, items) => { if (err) { res.send(err); } else { res.json(items); } }); }); // The create route adds a new item to the database app.post('/create', (req, res) => { const item = new Item({ name: req.body.name, description: req.body.description, price: req.body.price }); item.save((err, item) => { if (err) { res.send(err); } else { res.json(item); } }); }); // The update route updates an existing item in the database app.put('/update/:id', (req, res) => { Item.findByIdAndUpdate( req.params.id, { name: req.body.name, description: req.body.description, price: req.body.price }, (err, item) => { if (err) { res.send(err); } else { res.json(item); } } ); }); // The delete route removes an item from the database app.delete('/delete/:id', (req, res) => { Item.findByIdAndDelete(req.params.id, (err, item) => { if (err) { res.send(err); } else { res.json(item); } }); }); // Start the server const port = 3000; app.listen(port, () => { console.log(`Server listening on port ${port}`); });

## This code sets up routes for the four CRUD operations:

• GET /: Returns a list

## Step 7: Test the application

To test the application, start the server by running the following command:
node server.js
The server should start and you should see the message "Server listening on port 3000".
You can test the different routes using a tool like Postman.
For example, you can send a POST request to the /create route to create a new item:
{ "name": "Item 1", "description": "This is the first item", "price": 10 }
You should get a response with the newly created item. You can also send a GET request to the root route to get a list of all items in the database.
## Introduction
Our project aims to create a full-stack JavaScript application that performs CRUD (Create, Read, Update, Delete) operations using MongoDB, Express, and Node.js. The main objective of this project was to develop a simple and intuitive way to manage a database of items. application solves the problem of managing a database of items for users. It allows users to easily add, edit, or delete items from the database and provides a simple and intuitive way to manage their data. The application also provides measures to ensure the security and scalability of the system, making it suitable for both development and production environments. The application is therefore a great tool for managing a database of items in a secure and efficient way.
## Problem Definition
As stated above, the application solves the problem of managing a database of items for users. It allows users to easily add, edit, or delete items from the database and provides a simple and intuitive way to manage their data. The application also provides measures to ensure the security and scalability of the system, making it suitable for both development and production environments. The application is built using full-stack JavaScript, meaning that it uses JavaScript for both the frontend and backend. The server is built using the Express framework and Mongoose library, which makes it easy to handle CRUD operations. The HTML page displays the list of items in the database and allows the user to add, edit, or delete items by making AJAX requests to the server. The project also stores the items in a single collection called "items" and each item has name, description, and price fields. To ensure the security of the application, the system has implemented measures such as using the MongoDB driver's built-in support for connection pooling, which prevents connection exhaustion attacks, and the use of the body-parser middleware, which parses the request bodies and protects against body parsing attacks. The project has taken steps to assure scalability, such as employing connection pooling provided by the MongoDB driver, which enables the server to handle numerous concurrent connections. By establishing a cluster of worker processes with the help of the Node.js cluster module, which enables the server to benefit from multi-core systems and enhance performance. The application requires Node.js, npm and MongoDB to be installed and sets up the project.
## System Overview
The system consists of a Node.js server that connects to a MongoDB database and serves a simple HTML page with JavaScript frontend. The server is built using the Express framework and the MongoDB connection is established using the Mongoose library.
The HTML page displays a list of items in the database and allows the user to add, edit, or delete items. The frontend communicates with the server using AJAX requests and the server performs the CRUD operations on the database. 
## The system has the following key components:
• MongoDB database: Stores the items in a collection.
• Node.js server: Connects to the database and serves the HTML page with the frontend.
• HTML page: Displays the list of items and provides the interface for performing CRUD operations.
• JavaScript frontend: Makes AJAX requests to the server to perform the CRUD operations.
Key Design Decisions
1.	##	Frontend: The frontend is a simple HTML page with JavaScript that displays a list of items from the database. Users is able to add, edit, and delete items by making AJAX requests to the server. The page also includes forms for creating and editing items.
2.	##	Backend: The backend is built using Node.js and the Express framework. It will handle all the CRUD operations for the items, such as creating, reading, updating, and deleting items in the MongoDB database. The Mongoose library is used to handle the connection to the MongoDB database and provide a simple API for performing CRUD operations on the data.
3.	##	Database: The project uses MongoDB as the database. MongoDB is a popular NoSQL database that is well-suited for this type of application. The database will store all the items in a single collection called "items". Each item will have the following fields: name, description, and price.
4.	##	Routes: The server will have the following routes:
•	/: This route will handle an HTTP GET request and return a list of all items in the database.
•	/create: This route will handle an HTTP POST request and create a new item in the database.
•	/update/:id: This route will handle an HTTP PUT request and update an existing item in the database.
•	/delete/:id: This route will handle an HTTP DELETE request and delete an existing item in the database.
5.	##	Security and Scalability: To ensure the security of the application, the project uses the MongoDB driver's built-in support for connection pooling, which prevents connection exhaustion attacks, and the use of the body-parser middleware, which parses the request bodies and protects against body parsing attacks. To ensure scalability, the project leverages connection pooling, which is integrated into the MongoDB driver and enables the server to manage a lot of concurrent connections and make use of the Node.js cluster module to build a network of worker processes, which enables the server to benefit from multi-core computers and enhance performance.
        
### Security and Scalability
To ensure the security of the application, we implemented the following measures:
• We used the MongoDB driver's built-in support for connection pooling to prevent connection exhaustion attacks.
• We used the body-parser middleware to parse the request bodies and protect against body parsing attacks.
To ensure the scalability of the application, we implemented the following measures:
• We used the MongoDB driver's built-in support for connection pooling to allow the server to handle a large number of concurrent connections.
• We used the cluster module in Node.js to create a cluster of worker processes, which allows the server to take advantage of multi-core systems and improve performance.
 
## Refererence 
## initial CRUD application open-source-code-template: https://github.com/K45-94/plum-ui-components-prototype
Plum-UI is a social app template.
Optimizing mobile/small-screen view.
With swipe to go back transitions.
Contact through kelvinhiuhu@live.com

Also, check out the initial Plum from here and use mobile view =>https://plum-ui.surge.sh/
