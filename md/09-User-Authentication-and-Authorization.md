# Lesson 9: User Authentication and Authorization

In this lesson, we will learn how to implement user authentication and authorization in an Express.js application. We will cover user registration and login functionality using JSON Web Tokens (JWT) for stateless authentication. Additionally, we will discuss how to secure routes and manage user sessions effectively.

## Implementing User Registration and Login Functionality Using JWT

### Step 1: Setting Up Dependencies

Before we start implementing authentication, we need to install the necessary packages. We will use `jsonwebtoken` for creating and verifying JWTs, and `bcrypt` for hashing passwords.

1. **Install Required Packages**: Run the following command in your project directory:

   ```bash
   npm install jsonwebtoken bcrypt
   ```

### Step 2: Creating User Model

1. **Create a User Model**: In your `models` folder, create a file named `User.js` if you haven’t already. This model will store user information, including hashed passwords.

   ```javascript
   const mongoose = require('mongoose');

   const userSchema = new mongoose.Schema({
       username: { type: String, required: true, unique: true },
       password: { type: String, required: true },
   });

   const User = mongoose.model('User', userSchema);
   module.exports = User;
   ```

### Step 3: Implementing User Registration

1. **Create a Registration Route**: In your `server.js` file, add a route for user registration:

   ```javascript
   const express = require('express');
   const mongoose = require('mongoose');
   const bcrypt = require('bcrypt');
   const jwt = require('jsonwebtoken');
   const User = require('./models/User'); // Adjust the path as needed
   const app = express();
   const PORT = 3000;

   // Middleware to parse JSON bodies
   app.use(express.json());

   // User registration route
   app.post('/api/register', async (req, res) => {
       const { username, password } = req.body;

       // Hash the password
       const hashedPassword = await bcrypt.hash(password, 10);

       const user = new User({ username, password: hashedPassword });
       try {
           await user.save();
           res.status(201).send({ message: 'User registered successfully!' });
       } catch (error) {
           res.status(400).send({ error: 'Error registering user' });
       }
   });
   ```

### Step 4: Implementing User Login

1. **Create a Login Route**: Add a route for user login in your `server.js` file:

   ```javascript
   // User login route
   app.post('/api/login', async (req, res) => {
       const { username, password } = req.body;

       // Find the user by username
       const user = await User.findOne({ username });
       if (!user) {
           return res.status(400).send({ error: 'Invalid username or password' });
       }

       // Compare the password with the hashed password
       const isMatch = await bcrypt.compare(password, user.password);
       if (!isMatch) {
           return res.status(400).send({ error: 'Invalid username or password' });
       }

       // Generate a JWT token
       const token = jwt.sign({ id: user._id }, 'your_jwt_secret', { expiresIn: '1h' });
       res.send({ token });
   });
   ```

### Step 5: Securing Routes with JWT

1. **Create Middleware to Protect Routes**: Create a middleware function to verify JWT tokens:

   ```javascript
   const authenticateJWT = (req, res, next) => {
       const token = req.headers['authorization']?.split(' ')[1]; // Get token from Authorization header

       if (!token) {
           return res.sendStatus(403); // Forbidden
       }

       jwt.verify(token, 'your_jwt_secret', (err, user) => {
           if (err) {
               return res.sendStatus(403); // Forbidden
           }
           req.user = user; // Save user info for the request
           next();
       });
   };
   ```

2. **Secure a Protected Route**: Add a protected route that requires authentication:

   ```javascript
   app.get('/api/protected', authenticateJWT, (req, res) => {
       res.send(`Hello ${req.user.id}, you have access to this protected route!`);
   });
   ```

### Step 6: Testing User Authentication

1. **Start Your Server**: Run your server with the command:

   ```bash
   node server.js
   ```

2. **Use Postman to Test Registration and Login**:
   - **Register a User**: Send a POST request to `http://localhost:3000/api/register` with a JSON body:

     ```json
     {
         "username": "Alice",
         "password": "password123"
     }
     ```

   - **Login**: Send a POST request to `http://localhost:3000/api/login` with the same credentials:

     ```json
     {
         "username": "Alice",
         "password": "password123"
     }
     ```

     You should receive a JWT token in the response.

   - **Access Protected Route**: Send a GET request to `http://localhost:3000/api/protected` with the Authorization header:

     ```
     Authorization: Bearer <your_jwt_token>
     ```

     You should see a response confirming access to the protected route.

## Managing User Sessions

### Step 7: Using Sessions (Optional)

While JWT is a popular method for stateless authentication, you can also manage user sessions using libraries like `express-session`. Here’s how to set it up:

1. **Install express-session**:

   ```bash
   npm install express-session
   ```

2. **Set Up Session Middleware**:

   ```javascript
   const session = require('express-session');

   app.use(session({
       secret: 'your_session_secret',
       resave: false,
       saveUninitialized: true,
       cookie: { secure: false } // Set to true if using HTTPS
   }));
   ```

3. **Implement Login with Sessions**:

   ```javascript
   app.post('/api/login', async (req, res) => {
       const { username, password } = req.body;
       const user = await User.findOne({ username });

       if (!user || !(await bcrypt.compare(password, user.password))) {
           return res.status(400).send({ error: 'Invalid username or password' });
       }

       // Store user ID in session
       req.session.userId = user._id;
       res.send({ message: 'Logged in successfully' });
   });
   ```

4. **Protect Routes with Sessions**:

   ```javascript
   const authenticateSession = (req, res, next) => {
       if (req.session.userId) {
           next();
       } else {
           res.sendStatus(403); // Forbidden
       }
   };

   app.get('/api/protected', authenticateSession, (req, res) => {
       res.send(`Hello, user with ID: ${req.session.userId}`);
   });
   ```

## Conclusion

In this lesson, you learned how to implement user authentication and authorization in an Express.js application using JWT. You set up user registration and login functionality, secured routes with JWT, and explored managing user sessions using `express-session`. Understanding authentication and authorization is crucial for building secure web applications. In the next lesson, we will explore error handling in Express and how to manage errors effectively in your application.