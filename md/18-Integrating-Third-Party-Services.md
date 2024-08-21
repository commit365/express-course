# Lesson 18: Integrating Third-Party Services

In this lesson, we will learn how to integrate third-party services into your Express.js application. This includes using APIs from external services such as payment gateways and social media platforms. We will also discuss best practices for managing API keys and secrets securely.

## Using APIs from External Services

### Step 1: Understanding API Integration

APIs (Application Programming Interfaces) allow your application to communicate with external services. This can be useful for various functionalities, such as processing payments, sending notifications, or accessing social media data.

### Step 2: Example: Integrating a Payment Gateway

For this example, we will integrate the **Stripe** payment gateway, which allows you to accept online payments.

#### Step 2.1: Setting Up Stripe

1. **Create a Stripe Account**: Go to the [Stripe website](https://stripe.com/) and sign up for a free account.

2. **Get Your API Keys**: After creating an account, navigate to the API section of the Stripe dashboard to find your API keys. You will need both the **Publishable Key** and the **Secret Key**.

#### Step 2.2: Installing Stripe

1. **Install the Stripe Package**: In your project directory, run the following command to install the Stripe library:

   ```bash
   npm install stripe
   ```

#### Step 2.3: Setting Up Stripe in Your Application

1. **Create a Payment Route**: In your `server.js` file, set up a route to handle payment processing:

   ```javascript
   const express = require('express');
   const stripe = require('stripe')(process.env.STRIPE_SECRET_KEY); // Load secret key from environment variables
   const app = express();
   const PORT = 3000;

   // Middleware to parse JSON bodies
   app.use(express.json());

   // Payment route
   app.post('/api/payment', async (req, res) => {
       try {
           const { amount, currency, source } = req.body; // Get payment details from the request
           const paymentIntent = await stripe.paymentIntents.create({
               amount,
               currency,
               payment_method: source,
               confirm: true,
           });
           res.status(200).send(paymentIntent); // Send the payment intent as a response
       } catch (error) {
           res.status(500).send({ error: error.message }); // Handle errors
       }
   });

   app.listen(PORT, () => {
       console.log(`Server is running on http://localhost:${PORT}`);
   });
   ```

### Step 3: Testing the Payment Integration

1. **Set Up Environment Variables**: Create a `.env` file in your project root and add your Stripe secret key:

   ```
   STRIPE_SECRET_KEY=your_secret_key_here
   ```

2. **Start Your Server**: Run your server with the command:

   ```bash
   node server.js
   ```

3. **Test the Payment Route**: Use Postman to send a POST request to `http://localhost:3000/api/payment` with the following JSON body:

   ```json
   {
       "amount": 1000, // Amount in cents
       "currency": "usd",
       "source": "tok_visa" // Use a test token provided by Stripe
   }
   ```

   You should receive a successful response indicating that the payment was processed.

## Managing API Keys and Secrets Securely

### Step 4: Best Practices for Managing Secrets

1. **Use Environment Variables**: Store sensitive information like API keys in environment variables instead of hardcoding them in your application. This prevents accidental exposure of keys in your source code.

2. **Use a .env File**: Utilize a `.env` file to manage your environment variables locally. Make sure to add `.env` to your `.gitignore` file to prevent it from being committed to version control.

3. **Limit API Key Permissions**: When creating API keys, limit their permissions to only what is necessary for your application. This minimizes the impact in case a key is compromised.

4. **Rotate API Keys Regularly**: Periodically rotate your API keys to reduce the risk of unauthorized access. Update your application with the new keys as needed.

5. **Monitor API Usage**: Most third-party services provide dashboards to monitor API usage. Regularly check for any unusual activity that could indicate unauthorized access.

### Step 5: Example: Integrating a Social Media API

In addition to payment gateways, you may want to integrate social media APIs, such as Facebook or Twitter, to enable features like user authentication or sharing content.

1. **Choose a Social Media API**: For example, if you want to integrate Facebook Login, you would need to create a Facebook App and obtain your App ID and App Secret.

2. **Use OAuth for Authentication**: Many social media APIs use OAuth for authentication. You can use libraries like `passport` and `passport-facebook` to handle authentication in your Express application.

3. **Install Passport and Passport-Facebook**:

   ```bash
   npm install passport passport-facebook
   ```

4. **Set Up Passport**: Configure Passport in your application to use Facebook authentication:

   ```javascript
   const passport = require('passport');
   const FacebookStrategy = require('passport-facebook').Strategy;

   passport.use(new FacebookStrategy({
       clientID: process.env.FACEBOOK_APP_ID,
       clientSecret: process.env.FACEBOOK_APP_SECRET,
       callbackURL: 'http://localhost:3000/auth/facebook/callback',
       profileFields: ['id', 'displayName', 'photos', 'email']
   }, (accessToken, refreshToken, profile, done) => {
       // Handle user profile and authentication here
       return done(null, profile);
   }));

   app.use(passport.initialize());

   // Facebook authentication routes
   app.get('/auth/facebook', passport.authenticate('facebook'));

   app.get('/auth/facebook/callback', passport.authenticate('facebook', {
       successRedirect: '/',
       failureRedirect: '/login'
   }));
   ```

## Conclusion

In this lesson, you learned how to integrate third-party services into your Express.js application, including payment gateways and social media APIs. You also explored best practices for managing API keys and secrets securely. Understanding how to work with external APIs and handle sensitive information is crucial for building robust and secure applications. In the next lesson, we will explore best practices for testing your Express application to ensure its reliability and performance.