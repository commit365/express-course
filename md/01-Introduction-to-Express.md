# Lesson 1: Introduction to Express.js

In this lesson, we will provide an overview of Node.js and Express.js, a popular web framework built on top of Node.js. We will also guide you through setting up your development environment and creating a basic Express server.

## Overview of Node.js and Express.js

### What is Node.js?

Node.js is an open-source, cross-platform runtime environment that allows you to execute JavaScript code on the server side. It uses the V8 JavaScript engine developed by Google and is designed to build scalable network applications. Here are some key features of Node.js:

- **Event-Driven**: Node.js uses an event-driven, non-blocking I/O model, making it efficient and suitable for handling multiple connections simultaneously.
- **Single-Threaded**: Although it operates on a single thread, Node.js can handle many connections due to its asynchronous nature.
- **NPM (Node Package Manager)**: Node.js comes with NPM, which allows you to install and manage third-party packages easily.

### What is Express.js?

Express.js is a minimal and flexible web application framework for Node.js that provides a robust set of features for building web and mobile applications. It simplifies the process of creating server-side applications by providing a set of tools and middleware. Key features of Express.js include:

- **Routing**: Express allows you to define routes for handling different HTTP methods (GET, POST, PUT, DELETE) and URLs.
- **Middleware**: Express supports middleware functions that can execute during the request-response cycle, allowing for additional processing, such as logging, authentication, and error handling.
- **Template Engines**: Express can integrate with template engines (like EJS or Pug) to render dynamic HTML pages.

## Setting Up Your Development Environment

### Step 1: Install Node.js

Before you can start using Express.js, you need to have Node.js installed on your machine. Follow these steps to install Node.js:

1. **Download Node.js**: Go to the [Node.js official website](https://nodejs.org/) and download the installer for your operating system (Windows, macOS, or Linux).

2. **Run the Installer**: Follow the installation instructions provided by the installer. Make sure to select the option to install NPM alongside Node.js.

3. **Verify Installation**: After installation, open your terminal (Command Prompt, PowerShell, or Terminal) and run the following commands to verify that Node.js and NPM are installed correctly:

   ```bash
   node -v  # This should output the version of Node.js
   npm -v   # This should output the version of NPM
   ```

### Step 2: Create a New Project Directory

1. **Create a Project Directory**: In your terminal, create a new directory for your Express project and navigate into it:

   ```bash
   mkdir my-express-app
   cd my-express-app
   ```

### Step 3: Initialize a New Node.js Project

1. **Initialize NPM**: Run the following command to create a `package.json` file, which will manage your project’s dependencies:

   ```bash
   npm init -y
   ```

   The `-y` flag automatically answers "yes" to all prompts, creating a default `package.json` file.

### Step 4: Install Express.js

1. **Install Express**: Use NPM to install Express.js in your project:

   ```bash
   npm install express
   ```

   This command will add Express as a dependency in your `package.json` file and create a `node_modules` directory containing the Express library.

### Step 5: Create a Basic Express Server

1. **Create the Server File**: In your project directory, create a new file named `server.js`:

   ```bash
   touch server.js
   ```

2. **Write Basic Server Code**: Open `server.js` in your favorite text editor and add the following code:

   ```javascript
   const express = require('express');  // Import the Express module
   const app = express();                // Create an Express application

   const PORT = 3000;                   // Define the port number

   // Define a simple route
   app.get('/', (req, res) => {
       res.send('Hello, World!');       // Respond with a message
   });

   // Start the server
   app.listen(PORT, () => {
       console.log(`Server is running on http://localhost:${PORT}`);
   });
   ```

### Step 6: Run Your Express Server

1. **Start the Server**: In your terminal, run the following command to start your Express server:

   ```bash
   node server.js
   ```

   You should see a message indicating that the server is running.

2. **Access the Server**: Open your web browser and navigate to `http://localhost:3000`. You should see the message "Hello, World!" displayed in your browser.

## Conclusion

In this lesson, you learned about Node.js and Express.js, including their features and benefits. You set up your development environment, created a new Express project, and built a basic Express server that responds with a simple message. This foundation will prepare you for more advanced topics in Express.js as you continue through the course. In the next lesson, we will explore routing in Express and how to handle different HTTP methods.