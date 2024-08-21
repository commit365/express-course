# Lesson 3: Middleware in Express

In this lesson, we will explore the concept of middleware in Express.js and its critical role in processing requests and responses. Middleware functions are essential for adding functionality to your application, such as logging, authentication, and error handling. We will also implement both built-in and custom middleware functions to enhance our Express application.

## Exploring the Concept of Middleware

### What is Middleware?

Middleware is a function that has access to the request object (`req`), the response object (`res`), and the next middleware function in the application’s request-response cycle. It can perform various tasks, such as:

- Executing code
- Modifying the request and response objects
- Ending the request-response cycle
- Calling the next middleware function in the stack

Middleware functions can be used for a variety of purposes, including logging, parsing request bodies, handling authentication, and managing errors.

### Step 1: Using Built-in Middleware

Express comes with several built-in middleware functions that you can use in your applications. Some commonly used built-in middleware include:

1. **express.json()**: Parses incoming requests with JSON payloads.
2. **express.urlencoded()**: Parses incoming requests with URL-encoded payloads.

#### Example: Using Built-in Middleware

1. **Create or Open Your Server File**: If you haven't already, create a file named `server.js` or open your existing server file.

2. **Set Up Built-in Middleware**: Add the following code to use the built-in middleware functions:

   ```javascript
   const express = require('express');
   const app = express();

   const PORT = 3000;

   // Use express.json() to parse JSON bodies
   app.use(express.json());

   // Use express.urlencoded() to parse URL-encoded bodies
   app.use(express.urlencoded({ extended: true }));

   app.post('/submit', (req, res) => {
       const name = req.body.name; // Access the parsed body data
       res.send(`Hello, ${name}!`);
   });

   app.listen(PORT, () => {
       console.log(`Server is running on http://localhost:${PORT}`);
   });
   ```

3. **Test the Middleware**: Start your server and use a tool like Postman to send a POST request to `http://localhost:3000/submit` with a JSON body:

   ```json
   {
       "name": "John"
   }
   ```

   You should see the response: "Hello, John!".

## Implementing Custom Middleware Functions

### Step 2: Creating Custom Middleware

You can create custom middleware functions to add specific functionality to your application. A custom middleware function can be defined as follows:

```javascript
const myMiddleware = (req, res, next) => {
    // Custom logic here
    console.log('Middleware executed!');
    next(); // Call the next middleware or route handler
};
```

### Step 3: Using Custom Middleware

1. **Define a Custom Middleware Function**: Add a custom middleware function to your `server.js` file:

   ```javascript
   // Custom middleware function for logging
   const loggerMiddleware = (req, res, next) => {
       console.log(`${req.method} request for '${req.url}'`);
       next(); // Call the next middleware or route handler
   };

   // Use the custom middleware
   app.use(loggerMiddleware);
   ```

2. **Test the Custom Middleware**: Start your server and make a request to any endpoint (e.g., `http://localhost:3000/`). You should see log messages in the console indicating the HTTP method and URL of the request.

### Step 4: Implementing Middleware for Authentication

You can also create middleware for authentication purposes. Here's an example of a simple authentication middleware:

```javascript
const authMiddleware = (req, res, next) => {
    const token = req.headers['authorization'];
    if (token === 'mysecrettoken') {
        next(); // Token is valid, proceed to the next middleware or route handler
    } else {
        res.status(403).send('Forbidden: Invalid token');
    }
};

// Protect a route with the authentication middleware
app.get('/protected', authMiddleware, (req, res) => {
    res.send('This is a protected route!');
});
```

### Step 5: Implementing Error Handling Middleware

Error handling middleware can catch errors that occur in your application and respond appropriately. An error handling middleware function is defined with four parameters:

```javascript
const errorHandler = (err, req, res, next) => {
    console.error(err.stack); // Log the error stack
    res.status(500).send('Something broke!'); // Send a generic error response
};

// Use the error handling middleware
app.use(errorHandler);
```

### Step 6: Testing Error Handling Middleware

1. **Trigger an Error**: Add a route that deliberately throws an error:

   ```javascript
   app.get('/error', (req, res) => {
       throw new Error('This is a forced error!');
   });
   ```

2. **Test the Error Route**: Start your server and navigate to `http://localhost:3000/error`. You should see the error response: "Something broke!" along with the error logged in the console.

## Conclusion

In this lesson, you learned about middleware in Express.js and its role in processing requests and responses. You explored built-in middleware functions for parsing request bodies, created custom middleware for logging and authentication, and implemented error handling middleware to manage application errors. Understanding middleware is crucial for building robust and maintainable Express applications. In the next lesson, we will dive deeper into handling requests and responses in Express.