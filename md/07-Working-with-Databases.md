# Lesson 7: Working with Databases

In this lesson, we will learn how to connect an Express.js application to a database using an Object-Relational Mapping (ORM) library. We will cover how to use **Mongoose** to connect to **MongoDB** and **Sequelize** to connect to **PostgreSQL**. Additionally, we will perform CRUD (Create, Read, Update, Delete) operations and manage data persistence.

## Connecting to a Database

### Option 1: Connecting to MongoDB with Mongoose

#### Step 1: Setting Up MongoDB

1. **Install MongoDB**: If you haven't already, install MongoDB on your machine. You can also use a cloud-based service like [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) for easier setup.

2. **Create a Database**: If you are using MongoDB locally, start the MongoDB service and create a database named `mydatabase`.

#### Step 2: Installing Mongoose

1. **Install Mongoose**: In your project directory, run the following command to install Mongoose:

   ```bash
   npm install mongoose
   ```

#### Step 3: Connecting to MongoDB

1. **Open Your Server File**: Open your `server.js` file.

2. **Set Up Mongoose Connection**: Add the following code to connect to your MongoDB database:

   ```javascript
   const express = require('express');
   const mongoose = require('mongoose');
   const app = express();
   const PORT = 3000;

   // Connect to MongoDB
   mongoose.connect('mongodb://localhost:27017/mydatabase', {
       useNewUrlParser: true,
       useUnifiedTopology: true,
   })
   .then(() => console.log('MongoDB connected'))
   .catch(err => console.error('MongoDB connection error:', err));

   app.listen(PORT, () => {
       console.log(`Server is running on http://localhost:${PORT}`);
   });
   ```

### Option 2: Connecting to PostgreSQL with Sequelize

#### Step 1: Setting Up PostgreSQL

1. **Install PostgreSQL**: If you haven't already, install PostgreSQL on your machine. You can also use a cloud service like [Heroku Postgres](https://www.heroku.com/postgres).

2. **Create a Database**: Create a new database named `mydatabase` using a PostgreSQL client or command line.

#### Step 2: Installing Sequelize

1. **Install Sequelize and pg**: In your project directory, run the following command to install Sequelize and the PostgreSQL driver:

   ```bash
   npm install sequelize pg pg-hstore
   ```

#### Step 3: Connecting to PostgreSQL

1. **Open Your Server File**: Open your `server.js` file.

2. **Set Up Sequelize Connection**: Add the following code to connect to your PostgreSQL database:

   ```javascript
   const express = require('express');
   const { Sequelize } = require('sequelize');
   const app = express();
   const PORT = 3000;

   // Connect to PostgreSQL
   const sequelize = new Sequelize('mydatabase', 'username', 'password', {
       host: 'localhost',
       dialect: 'postgres',
   });

   sequelize.authenticate()
       .then(() => console.log('PostgreSQL connected'))
       .catch(err => console.error('PostgreSQL connection error:', err));

   app.listen(PORT, () => {
       console.log(`Server is running on http://localhost:${PORT}`);
   });
   ```

## Performing CRUD Operations

### Step 1: Defining a Model

#### MongoDB with Mongoose

1. **Create a Mongoose Model**: In your project directory, create a new folder named `models` and create a file named `User.js`:

   ```javascript
   const mongoose = require('mongoose');

   const userSchema = new mongoose.Schema({
       name: { type: String, required: true },
       email: { type: String, required: true, unique: true },
   });

   const User = mongoose.model('User', userSchema);
   module.exports = User;
   ```

#### PostgreSQL with Sequelize

1. **Create a Sequelize Model**: In your `models` folder, create a file named `User.js`:

   ```javascript
   const { DataTypes } = require('sequelize');
   const sequelize = require('../server'); // Adjust the path as needed

   const User = sequelize.define('User', {
       name: {
           type: DataTypes.STRING,
           allowNull: false,
       },
       email: {
           type: DataTypes.STRING,
           allowNull: false,
           unique: true,
       },
   });

   module.exports = User;
   ```

### Step 2: Performing CRUD Operations

#### Create Operation

1. **Add a Route to Create a User**: In your `server.js`, add a route to handle user creation:

   ```javascript
   // For MongoDB
   app.post('/users', async (req, res) => {
       const user = new User(req.body);
       try {
           await user.save();
           res.status(201).send(user);
       } catch (error) {
           res.status(400).send(error);
       }
   });

   // For PostgreSQL
   app.post('/users', async (req, res) => {
       try {
           const user = await User.create(req.body);
           res.status(201).send(user);
       } catch (error) {
           res.status(400).send(error);
       }
   });
   ```

#### Read Operation

1. **Add a Route to Retrieve Users**: Add a route to retrieve all users:

   ```javascript
   // For MongoDB
   app.get('/users', async (req, res) => {
       const users = await User.find();
       res.send(users);
   });

   // For PostgreSQL
   app.get('/users', async (req, res) => {
       const users = await User.findAll();
       res.send(users);
   });
   ```

#### Update Operation

1. **Add a Route to Update a User**: Add a route to update a user by ID:

   ```javascript
   // For MongoDB
   app.put('/users/:id', async (req, res) => {
       try {
           const user = await User.findByIdAndUpdate(req.params.id, req.body, { new: true });
           res.send(user);
       } catch (error) {
           res.status(400).send(error);
       }
   });

   // For PostgreSQL
   app.put('/users/:id', async (req, res) => {
       try {
           const user = await User.update(req.body, { where: { id: req.params.id } });
           res.send(user);
       } catch (error) {
           res.status(400).send(error);
       }
   });
   ```

#### Delete Operation

1. **Add a Route to Delete a User**: Add a route to delete a user by ID:

   ```javascript
   // For MongoDB
   app.delete('/users/:id', async (req, res) => {
       try {
           await User.findByIdAndDelete(req.params.id);
           res.send({ message: 'User deleted' });
       } catch (error) {
           res.status(400).send(error);
       }
   });

   // For PostgreSQL
   app.delete('/users/:id', async (req, res) => {
       try {
           await User.destroy({ where: { id: req.params.id } });
           res.send({ message: 'User deleted' });
       } catch (error) {
           res.status(400).send(error);
       }
   });
   ```

### Step 3: Testing CRUD Operations

1. **Start Your Server**: Run your server with the command:

   ```bash
   node server.js
   ```

2. **Use Postman**: Use Postman or another API testing tool to test the CRUD operations by sending requests to the appropriate endpoints.

   - **Create a User**: Send a POST request to `http://localhost:3000/users` with a JSON body:

     ```json
     {
         "name": "Alice",
         "email": "alice@example.com"
     }
     ```

   - **Retrieve Users**: Send a GET request to `http://localhost:3000/users`.

   - **Update a User**: Send a PUT request to `http://localhost:3000/users/{id}` with the updated data.

   - **Delete a User**: Send a DELETE request to `http://localhost:3000/users/{id}`.

## Conclusion

In this lesson, you learned how to connect your Express.js application to a database using Mongoose for MongoDB or Sequelize for PostgreSQL. You performed CRUD operations to create, read, update, and delete data, ensuring data persistence in your application. Understanding how to work with databases is essential for building dynamic web applications that manage user data effectively. In the next lesson, we will explore how to build RESTful APIs using Express.js.