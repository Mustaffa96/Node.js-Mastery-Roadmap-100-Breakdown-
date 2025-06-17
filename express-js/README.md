Aight, let’s break down **Express.js** in a *kampung* style—simple, chill, and straight to the point, like explaining to your cousin back in the village. Think of Express.js as the pakcik who runs the local *kedai* runcit—he’s got everything organized, handles customers (requests), and keeps the shop running smoothly. Let’s set up a basic Express.js server, talk about routing, middleware, and handling requests/responses, all in a way that feels like a chat over teh tarik.
### What’s Express.js, Bro?
Express.js is a lightweight framework for Node.js, like a trusty *kapchai* (motorcycle) that helps you zip through building web servers. It makes handling HTTP requests (like GET, POST) and sending responses (like HTML, JSON) super easy. Instead of writing raw Node.js code, Express gives you shortcuts to set up a server, manage routes, and add extra features like middleware.

# 1. Setting Up the Express Server (Bina Kedai)
Imagine you’re opening a **kedai* runcit*. First, you need a shop (the server). Here’s how you set it up:
* **Install Node.js and Express:**
  * Make sure you got Node.js installed (like having electricity in the *kampung*).
  * Create a folder for your project, say `my-kedai`, and run:
```bash
npm init -y
npm install express
```

This sets up your project and grabs Express, like stocking up your *kedai* with goods.

* **Create the Server:**
Create a file, `server.js`, and write this:
```javascript
const express = require('express');
const app = express(); // Create the shop (Express app)
const port = 3000; // The shop’s address (port)

app.listen(port, () => {
  console.log(`*kedai* open at http://localhost:${port}`);
});
```

* `express()` creates your app (the *kedai*).
* `app.listen(port, callback)` opens the shop at `http://localhost:300`0 and logs a message when it’s ready.
* Run it with `node server.js`, and your *kedai* is live!

# 2. Routing (Jalan ke *kedai*)
Routing is like telling customers which aisle in your *kedai* has what they want. In Express, routes define how your server responds to different URLs and HTTP methods (GET, POST, etc.).
Here’s an example:
``javascript
// Home page (like the front counter)
app.get('/', (req, res) => {
  res.send('Selamat datang ke *kedai* kami!'); // Send a welcome message
});

// Aisle for drinks
app.get('/drinks', (req, res) => {
  res.send('We got teh tarik, kopi, and air kelapa!');
});

// Aisle for snacks
app.get('/snacks', (req, res) => {
  res.json({ snacks: ['pisang goreng', 'keropok', 'kuih muih'] }); // Send JSON
});
```

* `app.get(path, callback)` handles GET requests to specific paths (like `/` or `/drinks`).
* `req` (request) is what the customer asks for (URL, query params, etc.).
* `res` (response) is what you send back (text, JSON, etc.).
* Try visiting `http://localhost:3000/drinks` in your browser, and you’ll see the drinks menu!

You can also handle other HTTP methods:

```javascript
app.post('/order', (req, res) => {
  res.send('Order received! We’ll fry your pisang goreng now.');
});
```

This is like accepting a customer’s order via a POST request.

# 3. Middleware (Pekerja *kedai*)
Middleware is like the adik helping out at the *kedai*, checking customers before they reach the counter. It’s a function that runs before or between the request and response. Middleware can do stuff like logging, checking permissions, or parsing data.

Here’s a simple middleware example:
```javascript
// Log every customer (request)
app.use((req, res, next) => {
  console.log(`Customer came to ${req.url} at ${new Date()}`);
  next(); // Pass to the next step (route or another middleware)
});
```
* `app.use(middleware)` applies middleware to all routes.
* `next()` tells Express to move to the next function (like passing the customer to the cashier).
* This logs every request’s URL and time.

**Built-in Middleware:**
Express has some ready-made middleware. For example, to parse JSON data from POST requests:
```javascript
app.use(express.json()); // Parse JSON from customer orders
```

Now, if a customer sends JSON like `{ "item": "teh tarik" }` in a POST request, you can access it via `req.body`.

Example:
```javascript
app.post('/order', (req, res) => {
  const item = req.body.item; // Get item from JSON
  res.send(`Got your order for ${item}!`);
});
```
4. Handling Requests and Responses (Layani Pelanggan)
Requests (`req`) and responses (`res`) are the heart of your *kedai*. Customers ask for stuff (requests), and you reply with goods or info (responses).
Requests (`req`):
`req.query`: Grabs query params (e.g., `?name=Ali` → `req.query.name = 'Ali'`).
`req.params`: Grabs URL params (e.g., `/user/Ali` → `req.params.name = 'Ali'`).
`req.body`: Grabs data from POST requests (needs `express.json()` middleware).
* Example:
```javascript
app.get('/user/:name', (req, res) => {
  res.send(`Hello, ${req.params.name}!`);
});
```

Visit `/user/Ali`, and it says “Hello, Ali!”

* Responses (`res`):
* `res.send(data)`: Send text or HTML.
* `res.json(data)`: Send JSON.
* `res.status(code)`: Set HTTP status (e.g., `res.status(404).send('Item not found')`).
* Example:
```javascript
app.get('/stock', (req, res) => {
  res.status(200).json({ stock: ['teh tarik', 'nasi lemak'] });
});
```

# Putting It All Together (Buka *kedai* Penuh)
Here’s a full *kampung*-style Express app that ties everything together:
```javascript
const express = require('express');
const app = express();
const port = 3000;

// Middleware: Log every request
app.use((req, res, next) => {
  console.log(`Customer at ${req.url} on ${new Date()}`);
  next();
});

// Middleware: Parse JSON
app.use(express.json());

// Route: Home page
app.get('/', (req, res) => {
  res.send('Selamat datang ke *kedai* *kampung*!');
});

// Route: Get drinks menu
app.get('/drinks', (req, res) => {
  res.json({ drinks: ['teh tarik', 'kopi', 'air kelapa'] });
});

// Route: Place an order (POST)
app.post('/order', (req, res) => {
  const item = req.body.item || 'nothing';
  res.send(`Order for ${item} received! Coming right up!`);
});

// Route: Dynamic user greeting
app.get('/user/:name', (req, res) => {
  res.send(`Hi, ${req.params.name}! Welcome to our *kedai*!`);
});

// Start the server
app.listen(port, () => {
  console.log(`*kedai* open at http://localhost:${port}`);
});
```

# How to Test Your Kedai
* Save the code in `server.js`.
* Run `node server.js`.
* Open your browser or use a tool like Postman:
  * Visit `http://localhost:3000/` → See welcome message.
  * Visit `http://localhost:3000/drinks` → See drinks menu in JSON.
  * Visit `http://localhost:3000/user/Ali` → See “Hi, Ali! Welcome to our *kedai*!”.
  * Send a POST request to `/order` with JSON `{ "item": "teh tarik" }` → Get “Order for teh tarik received!”.

# Extra Tips (Macam Bonus Kuih)
* **Error Handling:** Add a catch-all for bad routes:
```javascript
app.use((req, res) => {
  res.status(404).send('Sorry, aisle not found!');
});
```

* **Organize Routes:** Use `express.Router()` for bigger apps to keep routes tidy, like separate shelves in the *kedai*.
* **Static Files:** Serve HTML, CSS, or images with `app.use(express.static('public'))`.
* **Environment Variables:** Use `dotenv` to set the port dynamically (e.g., `process.env.PORT`).
