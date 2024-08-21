# Lesson 5: Serving Static Files

In this lesson, we will learn how to configure Express to serve static files such as CSS, JavaScript, and images. Serving static assets is essential for building a complete web application, as it allows you to include styles, scripts, and images that enhance the user interface. We will also discuss the importance of organizing static files within your project.

## Configuring Express to Serve Static Assets

### Step 1: Understanding Static Files

Static files are files that do not change and are served directly to the client. Common examples include:

- CSS files for styling
- JavaScript files for client-side functionality
- Images (JPEG, PNG, GIF, etc.)
- Fonts and other static assets

### Step 2: Setting Up Your Project Structure

Before we configure Express to serve static files, let's set up a proper project structure. Create a directory for your static files within your project. A common convention is to create a folder named `public` or `assets`.

1. **Create a Directory for Static Files**: In your project directory, create a new folder called `public`:

   ```bash
   mkdir public
   ```

2. **Add Static Files**: Inside the `public` directory, create subdirectories for CSS, JavaScript, and images:

   ```bash
   mkdir public/css
   mkdir public/js
   mkdir public/images
   ```

3. **Create Sample Files**: Create a sample CSS file, JavaScript file, and an image file for testing:

   - **CSS File**: Create a file named `styles.css` in the `public/css` directory:

     ```css
     /* public/css/styles.css */
     body {
         font-family: Arial, sans-serif;
         background-color: #f4f4f4;
         margin: 0;
         padding: 20px;
     }

     h1 {
         color: #333;
     }
     ```

   - **JavaScript File**: Create a file named `script.js` in the `public/js` directory:

     ```javascript
     // public/js/script.js
     document.addEventListener('DOMContentLoaded', () => {
         console.log('JavaScript is loaded and ready to use!');
     });
     ```

   - **Image File**: Place a sample image in the `public/images` directory. You can use any image file you have or download a sample image.

### Step 3: Configuring Express to Serve Static Files

1. **Open Your Server File**: Open your `server.js` file or create it if you haven't done so.

2. **Use the express.static Middleware**: Add the following code to serve static files from the `public` directory:

   ```javascript
   const express = require('express');
   const path = require('path');
   const app = express();
   const PORT = 3000;

   // Serve static files from the "public" directory
   app.use(express.static(path.join(__dirname, 'public')));

   app.get('/', (req, res) => {
       res.sendFile(path.join(__dirname, 'public', 'index.html')); // Serve the main HTML file
   });

   app.listen(PORT, () => {
       console.log(`Server is running on http://localhost:${PORT}`);
   });
   ```

### Step 4: Creating an HTML File to Test Static Assets

1. **Create an HTML File**: In the `public` directory, create a file named `index.html`:

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Static Files Example</title>
       <link rel="stylesheet" href="css/styles.css"> <!-- Link to the CSS file -->
   </head>
   <body>
       <h1>Welcome to the Static Files Example</h1>
       <p>This page demonstrates serving static assets with Express.js.</p>
       <img src="images/sample.jpg" alt="Sample Image"> <!-- Replace with your image file -->
       <script src="js/script.js"></script> <!-- Link to the JavaScript file -->
   </body>
   </html>
   ```

### Step 5: Testing Your Static Files

1. **Start Your Server**: Run your server with the following command:

   ```bash
   node server.js
   ```

2. **Access the Application**: Open your web browser and navigate to `http://localhost:3000`. You should see your HTML page styled with CSS, and the JavaScript should log a message to the console.

## Understanding the Importance of Organizing Static Files

### Step 6: Best Practices for Organizing Static Files

Organizing static files properly is crucial for maintaining a clean and manageable project structure. Here are some best practices:

1. **Use Descriptive Names**: Name your files and directories descriptively to make it easy to understand their purpose. For example, use `styles.css` for styles and `script.js` for JavaScript functionality.

2. **Separate by Type**: Create separate directories for different types of static assets (e.g., CSS, JavaScript, images) to keep your project organized.

3. **Minimize and Optimize**: Minimize your CSS and JavaScript files for production to reduce load times. Use tools like CSSNano or UglifyJS for minification.

4. **Use Version Control**: Keep your static files under version control (e.g., Git) to track changes and collaborate with others effectively.

## Conclusion

In this lesson, you learned how to configure Express to serve static files such as CSS, JavaScript, and images. You set up a proper project structure for organizing static assets and tested your application to ensure everything works as expected. Understanding how to serve static files is essential for building complete web applications with Express.js. In the next lesson, we will explore how to integrate template engines to render dynamic HTML views.