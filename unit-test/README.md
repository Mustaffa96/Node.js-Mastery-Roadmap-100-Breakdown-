Alright, let’s break down Node.js testing basics—unit tests with Jest or Mocha, mocking, and API testing—using simple, kampung-style explanations. Imagine we’re sitting under a tree in the village, chatting about coding over some teh tarik. I’ll keep it clear, practical, and relatable, like explaining to a neighbor who’s curious about how to test their app.

# 1. What is Testing? (Macam Jaga Kualiti di Pasar)
Testing in Node.js is like checking the quality of pisang or durian at the pasar. You don’t just trust that the fruit is good—you poke, smell, and taste to make sure it’s not rotten. Similarly, testing ensures your code works as expected, catches bugs, and doesn’t break when you add new features.
* **Unit Tests:** These are like checking one pisang at a time to see if it’s ripe. You test small pieces of your code (functions or modules) to make sure they do what they’re supposed to.
* **Tools like Jest or Mocha:** Think of these as your trusty timbang (weighing scale) or knife to cut and check the fruit. They’re frameworks that help you write and run tests.
* **Mocking:** This is like pretending you have a durian to test your recipe without actually buying one. You fake parts of your code (like database or API calls) to test other parts in isolation.
* **API Testing:** This is like checking if the `nasi lemak` stall delivers the right food to the right customer. You test if your app’s API endpoints (routes) return the correct responses.

# 2. Unit Tests with Jest or Mocha (Cek Satu-Satu Macam Pisang)
Unit tests focus on small, isolated pieces of code, like a single function. Let’s say you have a function to calculate the price of `ikan bakar` based on weight and type.
**Example Code (Node.js Function)**
```javascript
// ikanBakar.js
function calculateIkanBakarPrice(weight, type) {
  const pricePerKg = type === "kembung" ? 10 : 15; // kembung RM10/kg, others RM15/kg
  return weight * pricePerKg;
}
module.exports = calculateIkanBakarPrice;
```

### Using Jest (Macam Guna Timbang Moden)
Jest is a popular testing framework. It’s like a modern timbang with a digital display—easy to use and comes with everything you need.
* **Setup:**
  * Install Jest: `npm install --save-dev jest`
  * Add to `package.json`: `"test": "jest"`

* **Write a Test:**
Create a file `ikanBakar.test.js`:
```javascript
const calculateIkanBakarPrice = require("./ikanBakar");

test("Harga ikan kembung 2kg mestilah RM20", () => {
  expect(calculateIkanBakarPrice(2, "kembung")).toBe(20);
});

test("Harga ikan siakap 3kg mestilah RM45", () => {
  expect(calculateIkanBakarPrice(3, "siakap")).toBe(45);
});
```

* **Run the Test:**
  * Run `npm test`.
  * Jest checks if the function gives the right price, like making sure your `ikan bakar` isn’t overpriced.

### Using Mocha (Macam Timbang Tradisional)
Mocha is like a classic timbang—it’s flexible but needs extra tools like Chai (for assertions). It’s good if you want more control.

* **Setup:**
Install Mocha and Chai: `npm install --save-dev mocha chai`
Add to `package.json`: `"test": "mocha"`
* **Write a Test:**
Create `ikanBakar.test.js`:
```javascript
const { expect } = require("chai");
const calculateIkanBakarPrice = require("./ikanBakar");

describe("Ikan Bakar Price", () => {
  it("Harga ikan kembung 2kg mestilah RM20", () => {
    expect(calculateIkanBakarPrice(2, "kembung")).to.equal(20);
  });

  it("Harga ikan siakap 3kg mestilah RM45", () => {
    expect(calculateIkanBakarPrice(3, "siakap")).to.equal(45);
  });
});
```

* **Run the Test:**
  * Run `npm test`.
  * Mocha runs each test and tells you if the price calculation is betul or salah.

### Jest vs. Mocha:
* **Jest:** All-in-one, like a pasar malam stall with everything ready. Great for beginners.
* **Mocha:** More customizable, like setting up your own kedai with specific tools. Needs Chai or other libraries for assertions.

# 3. Mocking (Macam Guna Durian Palsu)
Mocking is when you create a fake version of something (like a database or external service) to test your code without using the real thing. It’s like practicing your sambal recipe with a plastic durian instead of the real, smelly one.

**Why Mock?**
* If your code calls a database or an external API (e.g., to check ikan stock), you don’t want to hit the real database every time you test—it’s slow and might cost money.
* Mocking lets you control the fake data, so you can test edge cases (e.g., what if the database is empty?).

**Mocking with Jest**
Let’s say your function checks ikan stock from a fake “database” (another module).
```javascript
// stockChecker.js
const checkIkanStock = async (type) => {
  // Imagine this calls a real database
  return type === "kembung" ? 100 : 50; // Fake stock data
};
module.exports = checkIkanStock;
```

Your main function uses it:
```javascript
// ikanBakar.js
const checkIkanStock = require("./stockChecker");

async function orderIkanBakar(weight, type) {
  const stock = await checkIkanStock(type);
  if (stock < weight) return "Maaf, stok tak cukup!";
  return `Order ${weight}kg ikan ${type} berjaya!`;
}
module.exports = orderIkanBakar;
```

**Mock the Database Call:**
In Jest, you can fake `checkIkanStock`:
```javascript
// ikanBakar.test.js
const orderIkanBakar = require("./ikanBakar");
jest.mock("./stockChecker"); // Fake the stockChecker module

test("Order gagal jika stok tak cukup", async () => {
  // Set up the mock to return low stock
  require("./stockChecker").mockReturnValue(1);

  const result = await orderIkanBakar(2, "kembung");
  expect(result).toBe("Maaf, stok tak cukup!");
});

test("Order berjaya jika stok cukup", async () => {
  // Set up the mock to return enough stock
  require("./stockChecker").mockReturnValue(10);

  const result = await orderIkanBakar(5, "kembung");
  expect(result).toBe("Order 5kg ikan kembung berjaya!");
});
```
* **What’s Happening?** Jest replaces the real checkIkanStock with a fake version you control. It’s like telling your kedai assistant to pretend there’s only 1kg of ikan to see what happens.

### Mocking with Mocha (Using Sinon)
Mocha needs a library like Sinon for mocking:
* Install: `npm install --save-dev sinon`
* Test example:
```javascript
const { expect } = require("chai");
const sinon = require("sinon");
const orderIkanBakar = require("./ikanBakar");
const checkIkanStock = require("./stockChecker");

describe("Order Ikan Bakar", () => {
  afterEach(() => sinon.restore()); // Clean up mocks

  it("Order gagal jika stok tak cukup", async () => {
    sinon.stub(checkIkanStock).resolves(1); // Fake stock = 1kg
    const result = await orderIkanBakar(2, "kembung");
    expect(result).to.equal("Maaf, stok tak cukup!");
  });
});
```
* **Sinon:** Like Jest’s mocking but more manual. Think of it as crafting your own fake durian from wood instead of using a ready-made plastic one.

# 4. API Testing (Cek Nasi Lemak Delivery)
API testing is like making sure the `nasi lemak` stall sends the right food when someone orders online. You test your app’s endpoints (e.g., /order-ikan) to ensure they return the correct response, status code, and data.

### Tool: Supertest (Macam Posmen untuk API)
Supertest is a library that makes API testing easy. It’s like a posmen who delivers your order and checks if it’s correct.
* **Setup:**
  * Install: `npm install --save-dev supertest`
  * Assume you have an Express app:

```javascript
// app.js
const express = require("express");
const app = express();
app.use(express.json());

app.post("/order-ikan", async (req, res) => {
  const { weight, type } = req.body;
  const stock = await require("./stockChecker")(type);
  if (stock < weight) {
    return res.status(400).json({ message: "Maaf, stok tak cukup!" });
  }
  res.json({ message: `Order ${weight}kg ikan ${type} berjaya!` });
});
module.exports = app;

We were talking about testing an API using Supertest, which is like a posmen checking if the `nasi lemak` order arrives correctly. The last test case in the previous response was incomplete, so let’s finish it and add a bit more context to make sure it’s clear.

### Completing the Supertest Example

```javascript
// app.test.js
const request = require("supertest");
const app = require("./app");
jest.mock("./stockChecker"); // Mock the stockChecker module

test("POST /order-ikan gagal jika stok tak cukup", async () => {
  require("./stockChecker").mockReturnValue(1); // Fake stock = 1kg

  const response = await request(app)
    .post("/order-ikan")
    .send({ weight: 2, type: "kembung" });

  expect(response.status).toBe(400);
  expect(response.body.message).toBe("Maaf, stok tak cukup!");
});

test("POST /order-ikan berjaya jika stok cukup", async () => {
  require("./stockChecker").mockReturnValue(10); // Fake stock = 10kg

  const response = await request(app)
    .post("/order-ikan")
    .send({ weight: 5, type: "kembung" });

  expect(response.status).toBe(200);
  expect(response.body.message).toBe("Order 5kg ikan kembung berjaya!");
});
```

* **What’s Happening?**
  * Supertest sends a fake HTTP request to your Express app, like ordering `ikan bakar` through a delivery app.
  * We mock the stockChecker to control the stock amount (1kg or 10kg) without touching a real database.
  * We check the response’s status code (200 for success, 400 for failure) and the message, like making sure the `nasi lemak` comes with the right sambal.

### Extra Tips for API Testing (Macam Posmen yang Cekap)
* **Test Different Scenarios:** Test edge cases, like sending no `weight` or an invalid `type`. It’s like checking if the kedai handles a customer who forgets to specify how much `ikan` they want.
```javascript
test("POST /order-ikan gagal jika tiada weight", async () => {
  const response = await request(app)
    .post("/order-ikan")
    .send({ type: "kembung" }); // No weight

  expect(response.status).toBe(400); // Assume your API validates this
});
```

* **Use a Test Server:** Supertest works with your Express app, but you can also start a temporary server for testing:
```javascript
const request = require("supertest");
const app = require("./app");

describe("API Tests", () => {
  let server;

  beforeAll((done) => {
    server = app.listen(0, done); // Start server on random port
  });

  afterAll((done) => {
    server.close(done); // Close server after tests
  });

  // Your tests here
});
```
This is like setting up a temporary kedai just to test your `ikan bakar` recipe.
* **Mock External APIs:** If your API calls another service (e.g., a weather API to check if it’s good to grill `ikan`), mock it with Jest or Sinon to avoid real calls during tests.

### Summary of Node.js Testing Basics (Macam Balik Kampung, Ringkas dan Jelas)
Let’s wrap this up like packing a bekal for a trip back to the kampung. Testing in Node.js is about making sure your app works as expected, like ensuring your `ikan bakar` is delicious before serving it to customers.
* Unit Tests (Cek Pisang Satu-Satu):
  * Test small pieces of code (e.g., a function to calculate `ikan bakar` price).
  * **Jest:** Easy, all-in-one tool. Just write `test` and `expect`, like using a modern timbang.
  * **Mocha:** Flexible but needs Chai or Sinon, like a traditional timbang where you pick your own tools.
  * Example: Test if `calculateIkanBakarPrice(2, "kembung")` returns `20`.

* **Mocking (Guna Durian Palsu):**
  * Fake parts of your code (e.g., database or API calls) to test in isolation.
  * **Jest:** Built-in mocking, like a ready-made plastic durian.
  * **Mocha with Sinon:** Manual but powerful, like carving a fake durian from wood.
  * Example: Mock checkIkanStock to return low stock and test if the order fails.

* **API Testing (Cek Delivery Nasi Lemak):**
  * Test your API endpoints to ensure they return the right responses.
  * **Supertest:** Acts like a posmen sending fake requests and checking responses.
  * Example: Test if `POST /order-ikan` returns “stok tak cukup” when stock is low.

### Practical Next Steps (Macam Plan Masak untuk Kenduri)
* **Start Small:** Write unit tests for simple functions first, like calculating prices or validating input.
* **Mock Wisely:** Use mocking for external dependencies (databases, APIs) to keep tests fast and reliable.
* **Test APIs Thoroughly:** Check success cases, errors, and edge cases (e.g., missing data, wrong formats).
* **Run Tests Often:** Add `npm test` to your workflow, like checking your ikan stock before opening the kedai.
* **Explore More:** Look into code coverage (e.g., Jest’s `--coverage` flag) to see how much of your code is tested, like counting how many pisang you’ve checked in a bunch.
