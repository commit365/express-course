# Lesson 8: Building RESTful APIs

In this lesson, we will learn how to design and implement RESTful APIs using Express.js. REST (Representational State Transfer) is an architectural style that defines a set of constraints for creating web services. We will cover the principles of RESTful API design, implement a simple API, and discuss best practices for API versioning and documentation using tools like Swagger and Postman.

## Designing and Implementing RESTful APIs Using Express

### Step 1: Understanding RESTful Principles

RESTful APIs are built around the following principles:

1. **Statelessness**: Each request from the client to the server must contain all the information needed to understand and process the request. The server does not store any client context between requests.

2. **Resource-Based**: APIs are designed around resources, which can be represented in various formats (e.g., JSON, XML). Each resource is identified by a unique URL.

3. **HTTP Methods**: RESTful APIs use standard HTTP methods to perform actions on resources:
   - **GET**: Retrieve data from the server.
   - **POST**: Create a new resource.
   - **PUT**: Update an existing resource.
   - **DELETE**: Remove a resource.

### Step 2: Setting Up Your Express API

1. **Create or Open Your Server File**: If you haven't already, create a file named `server.js` or open your existing server file.

2. **Set Up Express**: Add the following code to set up your Express application:

   ```javascript
   const express = require('express');
   const mongoose = require('mongoose');
   const app = express();
   const PORT = 3000;

   // Connect to MongoDB (adjust connection string as needed)
   mongoose.connect('mongodb://localhost:27017/mydatabase', {
       useNewUrlParser: true,
       useUnifiedTopology: true,
   });

   // Middleware to parse JSON bodies
   app.use(express.json());

   app.listen(PORT, () => {
       console.log(`Server is running on http://localhost:${PORT}`);
   });
   ```

### Step 3: Defining a Resource Model

1. **Create a Mongoose Model**: In your `models` folder, create a file named `Post.js` for a blog post resource:

   ```javascript
   const mongoose = require('mongoose');

   const postSchema = new mongoose.Schema({
       title: { type: String, required: true },
       content: { type: String, required: true },
       author: { type: String, required: true },
   });

   const Post = mongoose.model('Post', postSchema);
   module.exports = Post;
   ```

### Step 4: Implementing CRUD Operations for the API

1. **Create Routes for the API**: In your `server.js` file, add the following routes to handle CRUD operations for the `Post` resource:

   ```javascript
   const Post = require('./models/Post'); // Adjust the path as needed

   // Create a new post
   app.post('/api/posts', async (req, res) => {
       try {
           const post = new Post(req.body);
           await post.save();
           res.status(201).send(post);
       } catch (error) {
           res.status(400).send(error);
       }
   });

   // Retrieve all posts
   app.get('/api/posts', async (req, res) => {
       const posts = await Post.find();
       res.send(posts);
   });

   // Retrieve a single post by ID
   app.get('/api/posts/:id', async (req, res) => {
       try {
           const post = await Post.findById(req.params.id);
           if (!post) {
               return res.status(404).send();
           }
           res.send(post);
       } catch (error) {
           res.status(500).send(error);
       }
   });

   // Update a post by ID
   app.put('/api/posts/:id', async (req, res) => {
       try {
           const post = await Post.findByIdAndUpdate(req.params.id, req.body, { new: true, runValidators: true });
           if (!post) {
               return res.status(404).send();
           }
           res.send(post);
       } catch (error) {
           res.status(400).send(error);
       }
   });

   // Delete a post by ID
   app.delete('/api/posts/:id', async (req, res) => {
       try {
           const post = await Post.findByIdAndDelete(req.params.id);
           if (!post) {
               return res.status(404).send();
           }
           res.send({ message: 'Post deleted' });
       } catch (error) {
           res.status(500).send(error);
       }
   });
   ```

### Step 5: Testing Your API

1. **Start Your Server**: Run your server with the command:

   ```bash
   node server.js
   ```

2. **Use Postman**: Use Postman or another API testing tool to test the CRUD operations by sending requests to the appropriate endpoints:

   - **Create a Post**: Send a POST request to `http://localhost:3000/api/posts` with a JSON body:

     ```json
     {
         "title": "My First Post",
         "content": "This is the content of my first post.",
         "author": "Alice"
     }
     ```

   - **Retrieve All Posts**: Send a GET request to `http://localhost:3000/api/posts`.

   - **Retrieve a Single Post**: Send a GET request to `http://localhost:3000/api/posts/{id}`.

   - **Update a Post**: Send a PUT request to `http://localhost:3000/api/posts/{id}` with the updated data.

   - **Delete a Post**: Send a DELETE request to `http://localhost:3000/api/posts/{id}`.

## Understanding API Versioning and Best Practices

### Step 6: API Versioning

API versioning is essential for maintaining backward compatibility when you make changes to your API. There are several strategies for versioning your API:

1. **URI Versioning**: Include the version number in the URL path (e.g., `/api/v1/posts`).
2. **Query Parameter Versioning**: Use a query parameter to specify the version (e.g., `/api/posts?version=1`).
3. **Header Versioning**: Specify the version in the request headers.

For example, to implement URI versioning, modify your routes like this:

```javascript
app.post('/api/v1/posts', async (req, res) => { /* ... */ });
app.get('/api/v1/posts', async (req, res) => { /* ... */ });
// Repeat for other routes
```

### Step 7: Best Practices for API Design

1. **Use HTTP Status Codes**: Return appropriate HTTP status codes to indicate the result of the request (e.g., 200 for success, 404 for not found, 400 for bad requests).

2. **Consistent Naming Conventions**: Use consistent naming conventions for your endpoints. Use plural nouns for resource names (e.g., `/posts` instead of `/post`).

3. **Documentation**: Document your API endpoints to help users understand how to interact with your API. You can use tools like Swagger or Postman for API documentation.

### Step 8: Documenting Your API with Swagger

1. **Install Swagger UI**: You can use Swagger to create interactive API documentation. Install the necessary packages:

   ```bash
   npm install swagger-ui-express swagger-jsdoc
   ```

2. **Set Up Swagger in Your Application**: Add the following code to your `server.js` file to configure Swagger:

   ```javascript
   const swaggerJsDoc = require('swagger-jsdoc');
   const swaggerUi = require('swagger-ui-express');

   const swaggerOptions = {
       definition: {
           openapi: '3.0.0',
           info: {
               title: 'My API',
               version: '1.0.0',
               description: 'API documentation for my Express application',
           },
           servers: [
               {
                   url: 'http://localhost:3000',
               },
           ],
       },
       apis: ['./server.js'], // Path to the API docs
   };

   const swaggerDocs = swaggerJsDoc(swaggerOptions);
   app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerDocs));
   ```

3. **Add Swagger Comments**: Document your API endpoints using comments in your `server.js` file. For example:

   ```javascript
   /**
    * @swagger
    * /api/v1/posts:
    *   post:
    *     summary: Create a post
    *     requestBody:
    *       required: true
    *       content:
    *         application/json:
    *           schema:
    *             type: object
    *             properties:
    *               title:
    *                 type: string
    *               content:
    *                 type: string
    *               author:
    *                 type: string
    *     responses:
    *       201:
    *         description: Post created
    *       400:
    *         description: Bad request
    */

   app.post('/api/v1/posts', async (req, res) => { /* ... */ });
   ```

4. **Access the Documentation**: Start your server and navigate to `http://localhost:3000/api-docs` to view your API documentation.

## Conclusion

In this lesson, you learned how to design and implement RESTful APIs using Express.js. You explored the principles of REST, set up CRUD operations for a resource, and discussed best practices for API versioning and documentation. Understanding how to build RESTful APIs is essential for creating scalable and maintainable web applications. In the next lesson, we will explore user authentication and authorization in your Express application.