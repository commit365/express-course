# Lesson 19: Final Project: Building a Full-Stack Application

In this lesson, we will combine Express.js with a front-end framework (we will use React for this example) to build a full-stack application. We will also cover how to deploy your full-stack application to a live server and ensure it runs smoothly.

## Combining Express.js with React

### Step 1: Setting Up Your Express Backend

1. **Create a New Directory for Your Project**: Start by creating a new directory for your full-stack application:

   ```bash
   mkdir fullstack-app
   cd fullstack-app
   ```

2. **Initialize a New Express Application**: Create your Express backend:

   ```bash
   mkdir backend
   cd backend
   npm init -y
   npm install express cors mongoose dotenv
   ```

3. **Create Your Express Server**: In the `backend` directory, create a file named `server.js`:

   ```javascript
   const express = require('express');
   const cors = require('cors');
   const mongoose = require('mongoose');
   require('dotenv').config();

   const app = express();
   const PORT = process.env.PORT || 5000;

   // Middleware
   app.use(cors());
   app.use(express.json());

   // Connect to MongoDB
   mongoose.connect(process.env.MONGODB_URI, {
       useNewUrlParser: true,
       useUnifiedTopology: true,
   }).then(() => console.log('MongoDB connected'))
     .catch(err => console.error('MongoDB connection error:', err));

   // Sample route
   app.get('/api/message', (req, res) => {
       res.json({ message: 'Hello from the backend!' });
   });

   app.listen(PORT, () => {
       console.log(`Server is running on http://localhost:${PORT}`);
   });
   ```

4. **Create a .env File**: In the `backend` directory, create a `.env` file to store your MongoDB connection string:

   ```
   MONGODB_URI=mongodb://localhost:27017/mydatabase
   ```

### Step 2: Setting Up Your React Frontend

1. **Create a React Application**: In the root directory of your project, create a new React application using Create React App:

   ```bash
   npx create-react-app frontend
   ```

2. **Navigate to the Frontend Directory**:

   ```bash
   cd frontend
   ```

3. **Install Axios**: We will use Axios to make HTTP requests from the React frontend to the Express backend:

   ```bash
   npm install axios
   ```

4. **Create a Sample Component**: In the `src` directory of your React app, create a new file named `Message.js`:

   ```javascript
   import React, { useEffect, useState } from 'react';
   import axios from 'axios';

   const Message = () => {
       const [message, setMessage] = useState('');

       useEffect(() => {
           const fetchMessage = async () => {
               try {
                   const response = await axios.get('http://localhost:5000/api/message');
                   setMessage(response.data.message);
               } catch (error) {
                   console.error('Error fetching message:', error);
               }
           };

           fetchMessage();
       }, []);

       return (
           <div>
               <h1>{message}</h1>
           </div>
       );
   };

   export default Message;
   ```

5. **Update the App Component**: Modify the `src/App.js` file to include the `Message` component:

   ```javascript
   import React from 'react';
   import Message from './Message';

   const App = () => {
       return (
           <div>
               <Message />
           </div>
       );
   };

   export default App;
   ```

### Step 3: Running Your Full-Stack Application

1. **Start the Express Backend**: Open a terminal, navigate to the `backend` directory, and run:

   ```bash
   node server.js
   ```

2. **Start the React Frontend**: Open another terminal, navigate to the `frontend` directory, and run:

   ```bash
   npm start
   ```

3. **Access Your Application**: Open your browser and navigate to `http://localhost:3000`. You should see the message fetched from the backend displayed on the page.

## Deploying Your Full-Stack Application

### Step 4: Deploying the Express Backend

#### Option 1: Deploying to Heroku

1. **Create a Heroku Account**: If you haven’t already, sign up for a free account at [Heroku](https://www.heroku.com/).

2. **Install the Heroku CLI**: Follow the instructions on the [Heroku CLI documentation](https://devcenter.heroku.com/articles/heroku-cli) to install the CLI.

3. **Log in to Heroku**:

   ```bash
   heroku login
   ```

4. **Create a New Heroku App**:

   ```bash
   cd backend
   heroku create your-app-name
   ```

5. **Set Environment Variables**:

   ```bash
   heroku config:set MONGODB_URI=your_mongodb_uri
   ```

6. **Deploy Your Backend**:

   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git push heroku master
   ```

#### Option 2: Deploying to DigitalOcean

1. **Create a DigitalOcean Account**: Sign up for an account at [DigitalOcean](https://www.digitalocean.com/).

2. **Create a Droplet**: Launch a new droplet with Ubuntu.

3. **SSH into Your Droplet**:

   ```bash
   ssh root@your_droplet_ip
   ```

4. **Install Node.js and MongoDB**: Follow the instructions to install Node.js and MongoDB on your droplet.

5. **Clone Your Repository**: Clone your project repository and navigate to the backend directory.

6. **Install Dependencies and Start Your Server**:

   ```bash
   npm install
   node server.js
   ```

### Step 5: Deploying the React Frontend

1. **Build Your React Application**: In the `frontend` directory, run the following command to create a production build:

   ```bash
   npm run build
   ```

2. **Serve the Static Files**: You can serve the static files from your Express backend. Modify your `server.js` to serve the React build:

   ```javascript
   const path = require('path');

   // Serve static files from the React app
   app.use(express.static(path.join(__dirname, '../frontend/build')));

   // The "catchall" handler: for any request that doesn't
   // match one above, send back React's index.html file.
   app.get('*', (req, res) => {
       res.sendFile(path.join(__dirname, '../frontend/build', 'index.html'));
   });
   ```

3. **Deploy the React App**: If using Heroku, push your changes to deploy the updated backend that serves the React app.

### Step 6: Accessing Your Live Application

1. **Open Your Application**: Once deployed, you can access your full-stack application via the URL provided by Heroku or your DigitalOcean droplet.

## Conclusion

In this lesson, you learned how to build a full-stack application by combining Express.js with a front-end framework (React). You set up a basic chat application and deployed your full-stack application to a live server. Understanding how to integrate front-end and back-end technologies is essential for creating robust web applications. In the next lesson, we will explore best practices for maintaining and scaling your full-stack application.