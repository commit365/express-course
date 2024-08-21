# Lesson 6: Template Engines in Express

In this lesson, we will learn how to integrate a template engine with Express.js to render dynamic HTML views. Template engines allow you to create HTML pages that can include dynamic content, making it easier to build interactive web applications. We will cover how to set up a template engine, pass data from the server to templates, and render dynamic content.

## Integrating a Template Engine (EJS or Pug)

### Step 1: Choosing a Template Engine

For this lesson, we will use **EJS (Embedded JavaScript)** as our template engine. EJS is simple to use and widely adopted in the Express community. However, you can also choose **Pug** or other template engines based on your preference.

### Step 2: Installing EJS

1. **Open Your Project**: Navigate to your Express project directory.

2. **Install EJS**: Use NPM to install EJS:

   ```bash
   npm install ejs
   ```

### Step 3: Setting Up EJS in Your Express Application

1. **Open Your Server File**: Open your `server.js` file or create it if you haven't done so.

2. **Configure Express to Use EJS**: Add the following code to set EJS as the view engine:

   ```javascript
   const express = require('express');
   const path = require('path');
   const app = express();
   const PORT = 3000;

   // Set EJS as the template engine
   app.set('view engine', 'ejs');

   // Set the views directory
   app.set('views', path.join(__dirname, 'views'));

   app.listen(PORT, () => {
       console.log(`Server is running on http://localhost:${PORT}`);
   });
   ```

3. **Create a Views Directory**: In your project directory, create a folder named `views` to store your EJS templates:

   ```bash
   mkdir views
   ```

### Step 4: Creating an EJS Template

1. **Create an EJS File**: Inside the `views` directory, create a file named `index.ejs`:

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title><%= title %></title>
   </head>
   <body>
       <h1><%= heading %></h1>
       <p><%= message %></p>
   </body>
   </html>
   ```

   In this template, we use `<%= %>` to embed JavaScript expressions that will be evaluated and rendered.

### Step 5: Rendering the EJS Template

1. **Define a Route to Render the Template**: Add a route in your `server.js` file to render the `index.ejs` template:

   ```javascript
   app.get('/', (req, res) => {
       const data = {
           title: 'Home Page',
           heading: 'Welcome to My Express App',
           message: 'This page is rendered using EJS!'
       };
       res.render('index', data); // Render the EJS template with data
   });
   ```

2. **Test Your Application**: Start your server by running `node server.js` and navigate to `http://localhost:3000`. You should see the rendered HTML page with the title, heading, and message dynamically populated from the server.

## Passing Data from the Server to Templates

### Step 6: Dynamic Content Generation

You can pass any data from your server to your EJS templates, allowing for dynamic content generation based on user input, database queries, or other sources.

1. **Create Another Route with Dynamic Data**: Add a new route that demonstrates passing dynamic data:

   ```javascript
   app.get('/user/:name', (req, res) => {
       const userName = req.params.name; // Get the name from the route parameter
       const data = {
           title: 'User Profile',
           heading: `Profile of ${userName}`,
           message: `Hello, ${userName}! Welcome to your profile page.`
       };
       res.render('index', data); // Render the EJS template with user data
   });
   ```

2. **Test the Dynamic Route**: Start your server and navigate to `http://localhost:3000/user/Alice`. You should see a personalized message for the user "Alice".

### Step 7: Using Loops and Conditionals in EJS

EJS allows you to use JavaScript logic within your templates, such as loops and conditionals.

1. **Modify the EJS Template**: Update your `index.ejs` file to include a loop and a conditional statement:

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title><%= title %></title>
   </head>
   <body>
       <h1><%= heading %></h1>
       <p><%= message %></p>

       <% if (users.length > 0) { %>
           <h2>User List:</h2>
           <ul>
               <% users.forEach(user => { %>
                   <li><%= user %></li>
               <% }); %>
           </ul>
       <% } else { %>
           <p>No users found.</p>
       <% } %>
   </body>
   </html>
   ```

2. **Update the Route to Pass an Array**: Modify your route to include an array of users:

   ```javascript
   app.get('/', (req, res) => {
       const data = {
           title: 'Home Page',
           heading: 'Welcome to My Express App',
           message: 'This page is rendered using EJS!',
           users: ['Alice', 'Bob', 'Charlie'] // Sample user data
       };
       res.render('index', data);
   });
   ```

3. **Test the Updated Template**: Restart your server and navigate to `http://localhost:3000`. You should see the list of users displayed on the page.

## Conclusion

In this lesson, you learned how to integrate a template engine (EJS) with your Express.js application to render dynamic HTML views. You explored how to pass data from the server to templates for dynamic content generation, and you learned how to use loops and conditionals within EJS templates. Understanding how to work with template engines is essential for building interactive and user-friendly web applications. In the next lesson, we will explore how to handle form submissions and work with user input in your Express application.