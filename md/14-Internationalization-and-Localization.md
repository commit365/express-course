# Lesson 14: Internationalization and Localization

In this lesson, we will learn how to make your Express.js application multilingual and handle locale-specific formatting. Internationalization (i18n) and localization (l10n) are essential for creating applications that can cater to users from different linguistic and cultural backgrounds. We will implement user preferences for language and region settings, allowing users to interact with your application in their preferred language.

## Making Your Application Multilingual

### Step 1: Understanding Internationalization and Localization

- **Internationalization (i18n)**: The process of designing your application so that it can easily be adapted to various languages and regions without requiring engineering changes.
- **Localization (l10n)**: The process of adapting your application for a specific region or language by translating text and adjusting formatting.

### Step 2: Installing i18n Middleware

To implement internationalization in your Express application, we will use the `i18n` package, which simplifies the process of handling translations.

1. **Install i18n**: Run the following command in your project directory:

   ```bash
   npm install i18n
   ```

### Step 3: Setting Up i18n in Your Application

1. **Configure i18n**: In your `server.js` file, set up the `i18n` middleware:

   ```javascript
   const express = require('express');
   const i18n = require('i18n');
   const path = require('path');
   const app = express();
   const PORT = 3000;

   // Configure i18n
   i18n.configure({
       locales: ['en', 'es', 'fr'], // Supported languages
       directory: path.join(__dirname, 'locales'), // Directory for translation files
       defaultLocale: 'en', // Default language
       cookie: 'lang', // Cookie name for storing language preference
   });

   app.use(i18n.init); // Initialize i18n middleware
   ```

2. **Create a Locales Directory**: Create a directory named `locales` in your project root to store translation files:

   ```bash
   mkdir locales
   ```

3. **Create Translation Files**: Create JSON files for each language in the `locales` directory. For example, create `en.json`, `es.json`, and `fr.json`:

   - **en.json** (English):

     ```json
     {
         "welcome": "Welcome to our application!",
         "goodbye": "Goodbye!"
     }
     ```

   - **es.json** (Spanish):

     ```json
     {
         "welcome": "¡Bienvenido a nuestra aplicación!",
         "goodbye": "¡Adiós!"
     }
     ```

   - **fr.json** (French):

     ```json
     {
         "welcome": "Bienvenue dans notre application!",
         "goodbye": "Au revoir!"
     }
     ```

### Step 4: Using Translations in Your Routes

1. **Update Your Routes**: Modify your routes to use the translation keys defined in your JSON files:

   ```javascript
   app.get('/', (req, res) => {
       res.send(req.__('welcome')); // Use i18n to get the translated string
   });

   app.get('/goodbye', (req, res) => {
       res.send(req.__('goodbye')); // Use i18n to get the translated string
   });
   ```

### Step 5: Testing Multilingual Support

1. **Start Your Server**: Run your server with the command:

   ```bash
   node server.js
   ```

2. **Access the Application**: Open your browser and navigate to `http://localhost:3000`. You should see the welcome message in English.

3. **Change Language Using Cookies**: You can change the language by setting a cookie. For example, to switch to Spanish, you can use the following URL:

   ```
   http://localhost:3000/?lang=es
   ```

   The welcome message should now display in Spanish.

## Handling Locale-Specific Formatting

### Step 6: Formatting Dates and Numbers

In addition to translating text, you may need to format dates, numbers, and currencies according to the user's locale. The `i18n` package can help with this, but you may also want to use libraries like `moment` for date formatting and `numeral` for number formatting.

1. **Install Moment and Numeral**:

   ```bash
   npm install moment numeral
   ```

2. **Using Moment for Date Formatting**:

   ```javascript
   const moment = require('moment');

   app.get('/date', (req, res) => {
       const formattedDate = moment().locale(req.getLocale()).format('LLLL'); // Format date based on locale
       res.send(`Current date and time: ${formattedDate}`);
   });
   ```

3. **Using Numeral for Number Formatting**:

   ```javascript
   const numeral = require('numeral');

   app.get('/number', (req, res) => {
       const number = 1234567.89;
       const formattedNumber = numeral(number).format('0,0'); // Format number with commas
       res.send(`Formatted number: ${formattedNumber}`);
   });
   ```

### Step 7: Implementing User Preferences for Language and Region Settings

1. **Allow Users to Set Language Preferences**: Create a route that allows users to set their language preference. You can store this preference in a cookie or in the database.

   ```javascript
   app.post('/set-language', (req, res) => {
       const { lang } = req.body;
       res.cookie('lang', lang); // Set language cookie
       res.send(`Language set to ${lang}`);
   });
   ```

2. **Use Language Preference in Routes**: Modify your routes to respect the user's language preference:

   ```javascript
   app.use((req, res, next) => {
       const lang = req.cookies.lang || 'en'; // Default to English if no cookie
       i18n.setLocale(req, lang); // Set the locale based on cookie
       next();
   });
   ```

## Conclusion

In this lesson, you learned how to make your Express.js application multilingual and handle locale-specific formatting. You implemented user preferences for language and region settings, allowing users to interact with your application in their preferred language. Internationalization and localization are essential for creating inclusive applications that cater to a diverse user base. In the next lesson, we will explore best practices for securing your Express application against common vulnerabilities.