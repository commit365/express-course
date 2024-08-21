# Lesson 12: Deploying Express Applications

In this lesson, we will learn how to prepare your Express.js application for production deployment. We will explore various deployment options, including Heroku, AWS, and DigitalOcean, and provide step-by-step instructions for setting up a live server.

## Preparing Your Application for Production Deployment

### Step 1: Environment Variables

Using environment variables is crucial for managing configuration settings in production. You can use a package like `dotenv` to manage environment variables in your application.

1. **Install dotenv**: Run the following command to install the `dotenv` package:

   ```bash
   npm install dotenv
   ```

2. **Create a .env File**: In your project root, create a file named `.env` to store your environment variables:

   ```
   PORT=3000
   MONGODB_URI=mongodb://localhost:27017/mydatabase
   JWT_SECRET=your_jwt_secret
   ```

3. **Load Environment Variables**: In your `server.js` file, load the environment variables at the top:

   ```javascript
   require('dotenv').config();
   const express = require('express');
   const mongoose = require('mongoose');
   const app = express();
   const PORT = process.env.PORT || 3000;

   // Connect to MongoDB using the URI from environment variables
   mongoose.connect(process.env.MONGODB_URI, {
       useNewUrlParser: true,
       useUnifiedTopology: true,
   });
   ```

### Step 2: Logging and Error Handling

Ensure you have proper logging and error handling in place for production. You can use a logging library like `morgan` for HTTP request logging.

1. **Install Morgan**: Run the following command to install `morgan`:

   ```bash
   npm install morgan
   ```

2. **Set Up Morgan**: In your `server.js` file, add the following code to use `morgan` as middleware:

   ```javascript
   const morgan = require('morgan');

   // Use morgan for logging requests
   app.use(morgan('combined')); // You can use 'dev', 'tiny', or 'combined' formats
   ```

### Step 3: Optimizing Your Application

1. **Minimize Dependencies**: Review your `package.json` file and remove any unnecessary dependencies.

2. **Set Up a Production Build**: If you are using a front-end framework, ensure you create a production build to optimize performance.

3. **Test Your Application**: Before deploying, thoroughly test your application in a production-like environment.

## Exploring Deployment Options

### Option 1: Deploying to Heroku

Heroku is a popular cloud platform that allows you to deploy applications quickly and easily.

#### Step 1: Create a Heroku Account

1. **Sign Up**: Go to the [Heroku website](https://www.heroku.com/) and sign up for a free account.

#### Step 2: Install the Heroku CLI

1. **Download and Install**: Follow the instructions on the [Heroku CLI documentation](https://devcenter.heroku.com/articles/heroku-cli) to install the Heroku Command Line Interface.

#### Step 3: Prepare Your Application for Heroku

1. **Create a Procfile**: In your project root, create a file named `Procfile` (no file extension) and add the following line:

   ```
   web: node server.js
   ```

2. **Add a Start Script**: Ensure your `package.json` has a start script:

   ```json
   "scripts": {
       "start": "node server.js"
   }
   ```

#### Step 4: Deploy to Heroku

1. **Log in to Heroku**: Open your terminal and run:

   ```bash
   heroku login
   ```

2. **Create a New Heroku App**: Run the following command to create a new Heroku app:

   ```bash
   heroku create your-app-name
   ```

3. **Push Your Code to Heroku**: Deploy your application by pushing your code to Heroku:

   ```bash
   git push heroku main
   ```

4. **Set Environment Variables on Heroku**: Use the Heroku CLI to set environment variables:

   ```bash
   heroku config:set MONGODB_URI=your_mongodb_uri
   heroku config:set JWT_SECRET=your_jwt_secret
   ```

5. **Open Your App**: Once deployed, you can open your app in the browser:

   ```bash
   heroku open
   ```

### Option 2: Deploying to AWS

AWS (Amazon Web Services) provides a range of cloud services, including EC2 for hosting applications.

#### Step 1: Create an AWS Account

1. **Sign Up**: Go to the [AWS website](https://aws.amazon.com/) and sign up for an account.

#### Step 2: Launch an EC2 Instance

1. **Log in to AWS Management Console**: Navigate to the EC2 Dashboard.

2. **Launch Instance**: Click on "Launch Instance" and follow the prompts to select an Amazon Machine Image (AMI) and instance type.

3. **Configure Security Group**: Make sure to allow HTTP (port 80) and SSH (port 22) access.

#### Step 3: Connect to Your EC2 Instance

1. **SSH into Your Instance**: Use your terminal to connect to your instance:

   ```bash
   ssh -i /path/to/your-key.pem ec2-user@your-instance-public-dns
   ```

#### Step 4: Set Up Your Application on EC2

1. **Install Node.js**: On your EC2 instance, install Node.js and npm:

   ```bash
   sudo yum update -y
   sudo yum install -y nodejs npm
   ```

2. **Clone Your Repository**: Clone your application repository from GitHub:

   ```bash
   git clone https://github.com/yourusername/your-repo.git
   cd your-repo
   ```

3. **Install Dependencies**: Run the following command to install your application dependencies:

   ```bash
   npm install
   ```

4. **Start Your Application**: Run your application using:

   ```bash
   node server.js
   ```

### Option 3: Deploying to DigitalOcean

DigitalOcean is another cloud provider that offers simple and scalable cloud computing solutions.

#### Step 1: Create a DigitalOcean Account

1. **Sign Up**: Go to the [DigitalOcean website](https://www.digitalocean.com/) and create an account.

#### Step 2: Create a Droplet

1. **Create a Droplet**: Select a plan, choose an OS (Ubuntu is recommended), and create your droplet.

#### Step 3: Connect to Your Droplet

1. **SSH into Your Droplet**: Use your terminal to connect to your droplet:

   ```bash
   ssh root@your_droplet_ip
   ```

#### Step 4: Set Up Your Application on DigitalOcean

1. **Install Node.js**: On your droplet, install Node.js and npm:

   ```bash
   sudo apt update
   sudo apt install -y nodejs npm
   ```

2. **Clone Your Repository**: Clone your application repository from GitHub:

   ```bash
   git clone https://github.com/yourusername/your-repo.git
   cd your-repo
   ```

3. **Install Dependencies**: Run the following command to install your application dependencies:

   ```bash
   npm install
   ```

4. **Start Your Application**: Run your application using:

   ```bash
   node server.js
   ```

## Conclusion

In this lesson, you learned how to prepare your Express.js application for production deployment. We explored various deployment options, including Heroku, AWS, and DigitalOcean, and provided step-by-step instructions for setting up a live server. Understanding how to deploy your application is crucial for making it accessible to users and ensuring it runs smoothly in a production environment. In the next lesson, we will explore logging and monitoring techniques to keep track of your application’s performance and health.