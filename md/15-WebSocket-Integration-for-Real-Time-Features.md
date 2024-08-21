# Lesson 15: WebSocket Integration for Real-Time Features

In this lesson, we will learn how to integrate WebSocket communication into your Express.js application using the Socket.IO library. WebSockets enable real-time, bidirectional communication between the client and server, making them ideal for applications like chat systems, live notifications, and collaborative tools. We will set up a basic chat application as an example of real-time functionality.

## Setting Up WebSocket Communication Using Socket.IO

### Step 1: Installing Socket.IO

1. **Install Socket.IO**: In your project directory, run the following command to install Socket.IO:

   ```bash
   npm install socket.io
   ```

### Step 2: Setting Up the Server

1. **Modify Your Server File**: Open your `server.js` file and set up Socket.IO alongside your Express application:

   ```javascript
   const express = require('express');
   const http = require('http');
   const socketIo = require('socket.io');
   const app = express();
   const PORT = 3000;

   // Create an HTTP server
   const server = http.createServer(app);

   // Set up Socket.IO
   const io = socketIo(server);

   // Serve static files (optional)
   app.use(express.static('public'));

   // Start the server
   server.listen(PORT, () => {
       console.log(`Server is running on http://localhost:${PORT}`);
   });
   ```

### Step 3: Creating a Basic Chat Application

1. **Create a Public Directory**: Create a directory named `public` in your project root to store your front-end files.

   ```bash
   mkdir public
   ```

2. **Create an HTML File**: Inside the `public` directory, create a file named `index.html`:

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Chat Application</title>
       <style>
           body { font-family: Arial, sans-serif; }
           #messages { list-style-type: none; padding: 0; }
           #messages li { padding: 8px; margin: 4px; border: 1px solid #ccc; }
           #messageInput { width: 70%; }
       </style>
   </head>
   <body>
       <h1>Chat Application</h1>
       <ul id="messages"></ul>
       <input id="messageInput" autocomplete="off" /><button id="sendButton">Send</button>

       <script src="/socket.io/socket.io.js"></script>
       <script>
           const socket = io();

           // Listen for incoming messages
           socket.on('chat message', function(msg) {
               const item = document.createElement('li');
               item.textContent = msg;
               document.getElementById('messages').appendChild(item);
           });

           // Send message on button click
           document.getElementById('sendButton').onclick = function() {
               const messageInput = document.getElementById('messageInput');
               socket.emit('chat message', messageInput.value);
               messageInput.value = '';
           };
       </script>
   </body>
   </html>
   ```

### Step 4: Handling WebSocket Events on the Server

1. **Set Up Socket.IO Event Listeners**: In your `server.js` file, add event listeners to handle incoming messages:

   ```javascript
   io.on('connection', (socket) => {
       console.log('A user connected');

       // Listen for chat messages
       socket.on('chat message', (msg) => {
           io.emit('chat message', msg); // Broadcast the message to all connected clients
       });

       // Handle disconnection
       socket.on('disconnect', () => {
           console.log('A user disconnected');
       });
   });
   ```

### Step 5: Testing Your Chat Application

1. **Start Your Server**: Run your server with the command:

   ```bash
   node server.js
   ```

2. **Open Multiple Browser Tabs**: Open your browser and navigate to `http://localhost:3000`. Open multiple tabs to simulate different users.

3. **Send Messages**: Type a message in one tab and click the "Send" button. You should see the message appear in all open tabs in real time.

## Building Real-Time Features

### Step 6: Implementing Live Notifications

You can also use Socket.IO to implement live notifications in your application. Here’s how to set it up.

1. **Create a Notification Route**: Add a new route in your `server.js` file to trigger notifications:

   ```javascript
   app.get('/notify', (req, res) => {
       const notificationMessage = 'New notification received!';
       io.emit('notification', notificationMessage); // Broadcast notification to all clients
       res.send('Notification sent!');
   });
   ```

2. **Update the HTML File**: Modify your `index.html` file to listen for notifications:

   ```html
   <script>
       // Existing code...

       // Listen for notifications
       socket.on('notification', function(msg) {
           alert(msg); // Display notification as an alert
       });
   </script>
   ```

3. **Test Notifications**: Start your server and navigate to `http://localhost:3000`. In another tab, send a GET request to `http://localhost:3000/notify` (you can use Postman or your browser). All connected clients should receive a notification alert.

## Conclusion

In this lesson, you learned how to integrate WebSocket communication into your Express.js application using Socket.IO. You set up a basic chat application and implemented real-time features such as live notifications. WebSockets enable you to create interactive and engaging user experiences by allowing real-time communication between the client and server. In the next lesson, we will explore best practices for securing your Express application against common vulnerabilities.