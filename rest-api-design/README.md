Alright, let’s break down designing a REST API using NodeJS + Express in a way that feels like a chat in the *kampung* (village) – simple, clear, and no fancy jargon. Imagine we’re sitting under a big tree, sipping teh tarik, and I’m explaining how to build a solid API for, say, a *kampung* marketplace app where people buy and sell stuff like ikan bilis or kuih-muih. I’ll cover proper status codes, versioning, pagination, and validation in a way that’s easy to get.

# 1. What’s a REST API? (The Kampung Stall Analogy)
Think of a REST API as a *kedai* runcit (small village shop). Customers (clients, like a mobile app or website) come to the shop, ask for things (data, like a list of kuih), and the shopkeeper (your NodeJS + Express server) gives them what they need or tells them if something’s wrong (like “Sorry, *kuih talam* sold out!”). The shop needs to be organized, so customers know how to ask, what they’ll get, and how to handle problems.
* **NodeJS** is the shop’s foundation – it runs the whole operation.
* **Express** is like the friendly shopkeeper who handles requests quickly and keeps things tidy.

# 2. Proper Status Codes (The Kampung Way of Replying)
When someone comes to your *kedai* (API), you need to reply clearly so they know what’s going on. 
In REST APIs, we use HTTP status codes to say whether things went well or not. It’s like how you’d respond if someone asks for nasi lemak at your stall.

Here’s how it works in *kampung* terms:
* **200 OK:** “Here’s your nasi lemak, all good!”
The request worked, and you’re sending back the data (e.g., a list of products).
*Example:* `res.status(200).json({ message: "Here's your kuih list", data: kuihList })`
* **201 Created:** “Your order for kuih lapis is added to the menu!”
Used when something new is created, like a new user signing up.
*Example:* `res.status(201).json({ message: "New user created", user: newUser })`
* **400 Bad Request:** “Eh, what’s this? You didn’t tell me what kuih you want!”
The customer’s request is wrong or incomplete (e.g., missing required fields like email).
*Example:* `res.status(400).json({ error: "Email is required" })`
* **404 Not Found:** “Sorry, no ikan bilis in stock today!”
The thing they’re asking for doesn’t exist (e.g., a product ID that’s not in the database).
*Example:* `res.status(404).json({ error: "Product not found" })`
* **500 Internal Server Error** “Adoi, the stove broke, cannot cook now!”
Something went wrong on your server’s side (e.g., database crashed).
*Example:* `res.status(500).json({ error: "Something broke, try again later" })`

**Kampung Tip:** Always send a clear message with the status code, like { error: "Cannot find that kuih" }, so the customer knows what’s wrong. In Express, you’d set this with res.status(code).json(response).

# 3. Versioning (Keeping Your Kedai Menu Fresh)
Imagine your *kampung* shop changes how it sells kuih. Last year, you sold *kuih* by weight, but now you sell by pieces. If some customers still expect the old way, they’ll get confused. **Versioning** is like putting a sign saying, “This is Menu V2, we sell by pieces now!”
In a REST API, versioning keeps your API stable when you make changes. You don’t want to break apps that rely on your old setup.
* **How to Version:** Add a version number to your API routes, like /v1/products or /v2/products.

Example in Express:
```javascript
const express = require('express');
const app = express();

// Version 1: Old way of listing kuih
app.get('/v1/kuih', (req, res) => {
  res.status(200).json({ kuih: ["kuih lapis", "kuih talam"], unit: "by weight" });
});

// Version 2: New way of listing kuih
app.get('/v2/kuih', (req, res) => {
  res.status(200).json({ kuih: ["kuih lapis", "kuih talam"], unit: "by piece" });
});
```
* **Kampung Tip:** Use /v1/ for your first version. If you change how the API works (e.g., different data format), make a /v2/. Keep old versions running for a while so customers (apps) can switch slowly. You can also version via headers, but URL versioning (/v1/) is simpler, like a clear signboard at your *kedai*.

# 4. Pagination (Serving Kuih in Small Batches)
If your *kampung* marketplace has 1,000 types of kuih, you can’t give the customer the whole list at once – it’s too much, like piling 1,000 *kuih* on their plate! **Pagination** is like serving *kuih* in small, manageable batches (e.g., 10 at a time).
* **How It Works:** Let customers ask for a specific “page” of data with a limit (e.g., 10 items) and an offset (e.g., start at item 20).
Example: A customer asks for /kuih?page=2&limit=10, meaning “give me the second batch of 10 kuih.”
* **Express Example:**
```javascript
app.get('/kuih', (req, res) => {
  const page = parseInt(req.query.page) || 1; // Default to page 1
  const limit = parseInt(req.query.limit) || 10; // Default to 10 items
  const start = (page - 1) * limit; // Calculate starting point
  const end = start + limit;

  // Fake kuih data (replace with database query)
  const kuihList = Array(100).fill().map((_, i) => `Kuih ${i + 1}`);
  const paginatedKuih = kuihList.slice(start, end);

  res.status(200).json({
    message: "Here’s your kuih batch",
    page,
    limit,
    total: kuihList.length,
    data: paginatedKuih
  });
});
```
* **Kampung Tip:** Always include total (total items), page, and limit in the response so the customer knows how many pages are left. Also, use query params (?page=2&limit=10) – it’s like asking, “Can you pack 10 kuih starting from the 20th one?”

# 5. Validation (Checking the Customer’s Order)
When a customer orders kuih, you check if their order makes sense – like, “Did you specify how many kuih lapis you want?” or “Is your phone number valid for delivery?” Validation in a REST API is checking if the customer’s request is correct before processing it.
* **Why Validate?** To avoid problems, like saving a user with no name or a negative price for ikan bilis.
* **Tools:** Use a library like Joi or express-validator to check inputs.
* **Express Example with Joi:**
```javascript
const express = require('express');
const Joi = require('joi');
const app = express();
app.use(express.json()); // Parse JSON requests

// Define validation rules
const kuihSchema = Joi.object({
  name: Joi.string().min(3).required().messages({
    'string.min': 'Nama kuih kena at least 3 huruf lah!',
    'any.required': 'Nama kuih kena ada!'
  }),
  price: Joi.number().positive().required().messages({
    'number.positive': 'Harga kena lebih dari 0, bro!',
    'any.required': 'Harga kena ada!'
  })
});

// Route to add new kuih
app.post('/kuih', (req, res) => {
  const { error } = kuihSchema.validate(req.body);
  if (error) {
    return res.status(400).json({ error: error.details[0].message });
  }

  // If valid, proceed (e.g., save to database)
  const newKuih = req.body;
  res.status(201).json({ message: "Kuih added!", data: newKuih });
});
```
* **Kampung Tip:** Give clear error messages, like “Nama kuih kena ada!” instead of “Invalid input.” It’s like telling the customer exactly why their order can’t be processed. Validate early in the route to save time and avoid crashes.

### Putting It All Together (The Kampung Marketplace API)
Imagine your *kampung* marketplace API in action:
* **Status Codes:** When someone asks for /v1/kuih/123, you return 404 if that kuih doesn’t exist, or 200 with the kuih details if it does.
* **Versioning:** Your app uses /v1/kuih for now. If you change how prices are calculated later, you create /v2/kuih but keep /v1/ running.
* **Pagination:** For a big list like /v1/kuih?page=3&limit=20, you send 20 kuih starting from the 41st item, with a response like { page: 3, limit: 20, total: 100, data: [...] }.
* **Validation:** When someone posts to /v1/kuih to add a new kuih, you check if name and price are valid. If not, you send 400 with a message like “Harga kena lebih dari 0!”

Here’s a full Express example combining everything:
```javascript
const express = require('express');
const Joi = require('joi');
const app = express();
app.use(express.json());

// Validation schema
const kuihSchema = Joi.object({
  name: Joi.string().min(3).required(),
  price: Joi.number().positive().required()
});

// Fake database
let kuihList = Array(100).fill().map((_, i) => ({
  id: i + 1,
  name: `Kuih ${i + 1}`,
  price: 1.5
}));

// Get paginated kuih list (v1)
app.get('/v1/kuih', (req, res) => {
  const page = parseInt(req.query.page) || 1;
  const limit = parseInt(req.query.limit) || 10;
  const start = (page - 1) * limit;
  const end = start + limit;

  const paginatedKuih = kuihList.slice(start, end);
  res.status(200).json({
    message: "Here’s your kuih batch",
    page,
    limit,
    total: kuihList.length,
    data: paginatedKuih
  });
});

// Add new kuih (v1)
app.post('/v1/kuih', (req, res) => {
  const { error } = kuihSchema.validate(req.body);
  if (error) {
    return res.status(400).json({ error: error.details[0].message });
  }

  const newKuih = { id: kuihList.length + 1, ...req.body };
  kuihList.push(newKuih);
  res.status(201).json({ message: "Kuih added!", data: newKuih });
});

// Start the server
app.listen(3000, () => console.log("Kedai API ready at port 3000!"));
```

### Final Kampung Advice
* **Status Codes: Be clear, like telling a customer exactly why their *kuih* order failed.
* **Versioning:** Keep old and new menus separate so nobody gets confused.
* **Pagination:** Serve data in small, neat portions, like packing *kuih* in small boxes.
* **Validation:** Check orders carefully to avoid serving bad *kuih* or crashing your *kedai*.
This setup makes your API reliable, like a well-run *kampung* shop where customers keep coming back. If you want to dive deeper into any part (like securing the API or adding a database), just let me know, and we’ll talk more over teh tarik!
