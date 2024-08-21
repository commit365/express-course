# Lesson 4: Handling Requests and Responses

In this lesson, we will learn how to handle incoming requests and send responses in an Express.js application. We will cover how to work with JSON data, handle form submissions, and manage file uploads. Understanding how to effectively handle requests and responses is crucial for building interactive web applications.

## Learning How to Handle Incoming Requests and Send Responses

### Step 1: Understanding Request and Response Objects

In Express, every incoming request is represented by a `request` object (`req`), and every response is represented by a `response` object (`res`). The request object contains information about the incoming request, such as headers, query parameters, and body data, while the response object is used to send data back to the client.

### Step 2: Handling Incoming Requests

1. **Create or Open Your Server File**: If you haven't already, create a file named `server.js` or open your existing server file.

2. **Define Routes to Handle Requests**: Add the following code to handle different types of requests:

   ```javascript
   const express = require('express');
   const app = express();
   const PORT = 3000;

   // Middleware to parse JSON bodies
   app.use(express.json());

   // Middleware to parse URL-encoded bodies
   app.use(express.urlencoded({ extended: true }));

   // Handle GET request
   app.get('/', (req, res) => {
       res.send('Welcome to the Home Page!');
   });

   // Handle POST request
   app.post('/submit', (req, res) => {
       const name = req.body.name; // Access the parsed body data
       res.send(`Hello, ${name}!`);
   });

   app.listen(PORT, () => {
       console.log(`Server is running on http://localhost:${PORT}`);
   });
   ```

3. **Test Your Routes**: Start your server by running `node server.js`. Use a tool like Postman to test the POST request by sending a JSON body to `http://localhost:3000/submit`:

   ```json
   {
       "name": "Alice"
   }
   ```

   You should see the response: "Hello, Alice!".

## Working with JSON Data, Form Submissions, and File Uploads

### Step 3: Working with JSON Data

Express makes it easy to work with JSON data. When you use the `express.json()` middleware, it automatically parses incoming JSON requests and makes the data available in `req.body`.

1. **Define a Route for JSON Data**: Add the following route to handle JSON data:

   ```javascript
   app.post('/json', (req, res) => {
       const data = req.body; // Access the parsed JSON data
       res.json({
           message: 'Data received successfully!',
           receivedData: data
       });
   });
   ```

2. **Test the JSON Route**: Start your server and send a POST request to `http://localhost:3000/json` with a JSON body:

   ```json
   {
       "key": "value"
   }
   ```

   You should see a response confirming the data received.

### Step 4: Handling Form Submissions

Express can also handle form submissions easily. When forms are submitted with the `application/x-www-form-urlencoded` content type, the data is automatically parsed by the `express.urlencoded()` middleware.

1. **Create an HTML Form**: Create a simple HTML file named `form.html` in your project directory:

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Form Submission</title>
   </head>
   <body>
       <h1>Submit Your Name</h1>
       <form action="/submit" method="POST">
           <input type="text" name="name" placeholder="Enter your name" required>
           <button type="submit">Submit</button>
       </form>
   </body>
   </html>
   ```

2. **Serve the HTML File**: Modify your `server.js` file to serve the HTML form:

   ```javascript
   const path = require('path');

   // Serve the HTML form
   app.get('/form', (req, res) => {
       res.sendFile(path.join(__dirname, 'form.html'));
   });
   ```

3. **Test the Form Submission**: Start your server and navigate to `http://localhost:3000/form`. Fill out the form and submit it. You should see a response with the greeting message.

### Step 5: Handling File Uploads

To handle file uploads, we can use middleware like `multer`, which simplifies the process of handling `multipart/form-data`.

1. **Install Multer**: Install the `multer` package:

   ```bash
   npm install multer
   ```

2. **Set Up Multer in Your Server**: Add the following code to configure `multer` for handling file uploads:

   ```javascript
   const multer = require('multer');
   const upload = multer({ dest: 'uploads/' }); // Specify the upload directory

   // Handle file upload
   app.post('/upload', upload.single('file'), (req, res) => {
       const file = req.file; // Access the uploaded file
       res.send(`File uploaded successfully: ${file.originalname}`);
   });
   ```

3. **Create an HTML Form for File Upload**: Update your `form.html` to include a file upload input:

   ```html
   <h1>Upload a File</h1>
   <form action="/upload" method="POST" enctype="multipart/form-data">
       <input type="file" name="file" required>
       <button type="submit">Upload</button>
   </form>
   ```

4. **Test the File Upload**: Start your server and navigate to `http://localhost:3000/form`. Use the file upload form to select a file and submit it. You should see a response confirming the file upload.

## Conclusion

In this lesson, you learned how to handle incoming requests and send responses in an Express.js application. You explored how to work with JSON data, handle form submissions, and manage file uploads using middleware like `multer`. Understanding these concepts is essential for building interactive web applications with Express.js. In the next lesson, we will dive into serving static files and configuring your application to manage assets effectively.