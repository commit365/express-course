# Lesson 2: Routing in Express

In this lesson, we will explore routing in Express.js, which is a fundamental concept for building web applications. Routing allows you to define how your application responds to client requests for specific endpoints. We will cover how to define routes, handle different HTTP methods, and use route parameters, query strings, and nested routes for dynamic routing.

## Understanding How to Define Routes and Handle Different HTTP Methods

### What is Routing?

Routing refers to the mechanism that maps incoming requests to the appropriate handler functions based on the request URL and HTTP method. In Express, you can define routes using the `app.get()`, `app.post()`, `app.put()`, and `app.delete()` methods, among others.

### Step 1: Defining Basic Routes

1. **Create or Open Your Server File**: If you haven't already, create a file named `server.js` or open your existing server file.

2. **Define Basic Routes**: Add the following code to define routes for handling different HTTP methods:

   ```javascript
   const express = require('express');
   const app = express();

   const PORT = 3000;

   // Define a GET route
   app.get('/', (req, res) => {
       res.send('Welcome to the Home Page!');
   });

   // Define a POST route
   app.post('/submit', (req, res) => {
       res.send('Form submitted successfully!');
   });

   // Define a PUT route
   app.put('/update', (req, res) => {
       res.send('Resource updated!');
   });

   // Define a DELETE route
   app.delete('/delete', (req, res) => {
       res.send('Resource deleted!');
   });

   app.listen(PORT, () => {
       console.log(`Server is running on http://localhost:${PORT}`);
   });
   ```

3. **Test Your Routes**: Start your server by running `node server.js` and test your routes using a tool like Postman or your browser. You can send requests to different endpoints and observe the responses.

## Using Route Parameters, Query Strings, and Nested Routes for Dynamic Routing

### Step 2: Route Parameters

Route parameters allow you to capture values from the URL and use them in your route handlers. This is useful for creating dynamic routes.

1. **Define a Route with Parameters**: Modify your `server.js` file to include a route that captures a user ID from the URL:

   ```javascript
   // Define a route with a route parameter
   app.get('/user/:id', (req, res) => {
       const userId = req.params.id;  // Access the route parameter
       res.send(`User ID is: ${userId}`);
   });
   ```

2. **Test the Route**: Start your server and navigate to `http://localhost:3000/user/123`. You should see the response: "User ID is: 123".

### Step 3: Query Strings

Query strings allow you to send additional parameters in the URL. They are appended to the URL after a question mark (`?`) and separated by ampersands (`&`).

1. **Define a Route with Query Strings**: Add a route that accepts query parameters:

   ```javascript
   // Define a route that uses query strings
   app.get('/search', (req, res) => {
       const query = req.query.q;  // Access the query string parameter
       res.send(`Search query: ${query}`);
   });
   ```

2. **Test the Route**: Start your server and navigate to `http://localhost:3000/search?q=express`. You should see the response: "Search query: express".

### Step 4: Nested Routes

Nested routes allow you to create a hierarchy of routes, which can help organize your application better.

1. **Define Nested Routes**: You can create nested routes by defining routes within other routes. For example:

   ```javascript
   // Define a route for blog posts
   app.get('/blog', (req, res) => {
       res.send('Blog Home Page');
   });

   // Define a nested route for a specific blog post
   app.get('/blog/:postId', (req, res) => {
       const postId = req.params.postId;
       res.send(`Viewing blog post with ID: ${postId}`);
   });
   ```

2. **Test the Nested Routes**: Start your server and navigate to `http://localhost:3000/blog/456`. You should see the response: "Viewing blog post with ID: 456".

## Conclusion

In this lesson, you learned how to define routes in Express.js and handle different HTTP methods. You explored route parameters for capturing dynamic values from the URL, query strings for passing additional parameters, and nested routes for organizing your application structure. Understanding routing is essential for building effective web applications with Express.js. In the next lesson, we will dive deeper into middleware in Express and how it can enhance your application’s functionality.