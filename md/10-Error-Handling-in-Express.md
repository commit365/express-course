# Lesson 10: Error Handling in Express

In this lesson, we will learn how to handle errors gracefully in an Express.js application. Proper error handling is essential for providing a good user experience and maintaining the stability of your application. We will explore how to implement custom error handling middleware to catch and respond to errors effectively.

## Learning How to Handle Errors Gracefully

### Step 1: Understanding Error Handling in Express

In Express, errors can occur for various reasons, such as:

- Invalid user input
- Database connection issues
- Missing resources
- Internal server errors

Express provides a built-in way to handle errors by using middleware. Error-handling middleware is defined with four parameters: `err`, `req`, `res`, and `next`.

### Step 2: Basic Error Handling

1. **Create or Open Your Server File**: If you haven't already, create a file named `server.js` or open your existing server file.

2. **Set Up Basic Routes**: Add some routes that may generate errors:

   ```javascript
   const express = require('express');
   const app = express();
   const PORT = 3000;

   // Middleware to parse JSON bodies
   app.use(express.json());

   // Example route that may throw an error
   app.get('/api/data', (req, res) => {
       throw new Error('Something went wrong while fetching data!'); // Simulate an error
   });

   app.listen(PORT, () => {
       console.log(`Server is running on http://localhost:${PORT}`);
   });
   ```

3. **Test the Error Route**: Start your server with the command:

   ```bash
   node server.js
   ```

   Navigate to `http://localhost:3000/api/data` in your browser or use Postman. You should see an error message.

### Step 3: Implementing Custom Error Handling Middleware

1. **Define Error Handling Middleware**: Add the following error handling middleware to your `server.js` file:

   ```javascript
   // Custom error handling middleware
   app.use((err, req, res, next) => {
       console.error(err.stack); // Log the error stack for debugging
       res.status(500).json({ error: 'Internal Server Error', message: err.message }); // Send a JSON response
   });
   ```

2. **Test the Error Handling Middleware**: Restart your server and navigate to `http://localhost:3000/api/data` again. You should see a structured JSON response with the error message.

### Step 4: Handling Different Types of Errors

You can customize your error handling middleware to handle different types of errors more effectively. For example, you might want to handle validation errors differently from server errors.

1. **Modify the Error Handling Middleware**:

   ```javascript
   app.use((err, req, res, next) => {
       if (err.name === 'ValidationError') {
           return res.status(400).json({ error: 'Validation Error', message: err.message });
       }
       console.error(err.stack);
       res.status(500).json({ error: 'Internal Server Error', message: err.message });
   });
   ```

2. **Simulate a Validation Error**: You can create a route that simulates a validation error:

   ```javascript
   app.post('/api/user', (req, res) => {
       const { username } = req.body;
       if (!username) {
           const error = new Error('Username is required');
           error.name = 'ValidationError'; // Set the error name for validation
           throw error;
       }
       res.send({ message: 'User created successfully!' });
   });
   ```

3. **Test the Validation Error**: Use Postman to send a POST request to `http://localhost:3000/api/user` without a username. You should receive a 400 status code with a validation error message.

## Implementing Custom Error Handling for Better User Experience

### Step 5: Creating a Custom Error Class

To improve error handling, you can create a custom error class that extends the built-in `Error` class. This allows you to define specific error types and statuses.

1. **Create a Custom Error Class**:

   ```javascript
   class AppError extends Error {
       constructor(message, statusCode) {
           super(message);
           this.statusCode = statusCode;
           Error.captureStackTrace(this, this.constructor);
       }
   }
   ```

2. **Use the Custom Error Class**: Modify your routes to throw an instance of `AppError`:

   ```javascript
   app.get('/api/data', (req, res, next) => {
       next(new AppError('Data not found', 404)); // Throw a custom error
   });
   ```

3. **Update the Error Handling Middleware**:

   ```javascript
   app.use((err, req, res, next) => {
       const statusCode = err.statusCode || 500;
       res.status(statusCode).json({ error: err.message });
   });
   ```

### Step 6: Testing Custom Error Handling

1. **Test the Custom Error**: Start your server and navigate to `http://localhost:3000/api/data`. You should see a JSON response with a 404 status code and the error message "Data not found".

## Conclusion

In this lesson, you learned how to handle errors gracefully in an Express.js application. You implemented custom error handling middleware to catch errors and respond with meaningful messages. Additionally, you explored how to create a custom error class for better error management. Proper error handling is crucial for providing a good user experience and maintaining the stability of your application. In the next lesson, we will explore how to implement logging and monitoring in your Express application to track performance and errors.