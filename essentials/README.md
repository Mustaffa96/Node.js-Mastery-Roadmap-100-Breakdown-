Oi, bro, let’s break down JavaScript essentials like we’re chilling in a kampung, sipping teh tarik under a big ol’ tree, lah. I’ll explain it simple-simple, like talking to my cousin who just started coding, but with that kampung vibe—relaxed, got some slang, and straight to the point, sia. Here we go!

# 1. Variables: Storing Your Stuff
Variables are like your kampung sundry shop’s containers—got place to keep your stuff, lah. In JavaScript, you got three ways to declare them:

**var:** Old-school, like your uncle’s bicycle. Can be redeclared and scoped to function or global. Bit messy, sia.

```
javascript
---
var nasi = "lemak"; // Can change later
```

**let:** Modern, like your new smartphone. Scoped to block (inside {}), can change value but no redeclare.

```
javascript

let teh = "tarik"; // Change value OK, but no repeat 'let teh'
```

**const:** Like your grandma’s secret rendang recipe, cannot change after setting. Also block-scoped.


```
javascript

const kampung = "best"; // Fixed, cannot change
```

Use let or const nowadays, lah. var is like using a typewriter—nobody does that anymore.

# 2. Data Types: What’s in Your Container?

JavaScript got different kinds of stuff you can store, like items in your pasar malam stall:

## Primitive Types (simple stuff):

**string**: Words or text, like "nasi goreng" or 'ayam penyet'.

**number:** Any number, like 42 or 3.14. No separate integer or float, all same lah.

**boolean:** True or false, like “shop open meh?” (true) or “closed lah” (false).

**undefined:** Variable declared but no value yet, like “eh, where my kuih go?”

**null**: Empty on purpose, like “I got no plans, bro.”

**symbol:** Unique ID thing, rare in kampung coding but good for fancy stuff.

**bigint:** For super big numbers, like your angpow collection after 10 years.

## Non-Primitive (complex stuff):

**object:** Like a basket holding many things—key-value pairs. Can be arrays, functions, or custom objects.

```
javascript

let stall = { food: "roti canai", price: 2 };
```

Check type with typeof, like asking “this what lah?”

```
javascript

typeof "nasi" // "string"
typeof 42 // "number"
```

# 3. Functions: Your Kampung Chef

Functions are like your makcik cooking curry—give her ingredients, she gives you food. In JavaScript, functions do work when you call them.

Declare a function:
```
javascript

function cookNasi(type) {
  return `Nasi ${type} siap, bro!`;
}
cookNasi("lemak"); // "Nasi lemak siap, bro!"
```

Function Expression:

```
javascript

const makan = function() {
  return "Jom makan!";
};
```

Parameters and Return:

Parameters are what you give the function (like type above). return is what it gives back. No return? It’s like makcik forgetting to serve the plate—undefined lah.

Default Parameters (ES6+):

```
javascript

function order(food = "nasi") {
  return `You get ${food}.`;
}
order(); // "You get nasi."
```

# 4. Closures: Secret Recipe Keeper

Closure is like your makcik’s secret sambal recipe—she keeps it inside her kitchen even after you leave. In JavaScript, a function remembers variables from its outer scope even after that scope is gone.

```
javascript

function makeSambal() {
  let secret = "extra chili";
  return function() {
    return `Sambal with ${secret}!`;
  };
}
let mySambal = makeSambal();
mySambal(); // "Sambal with extra chili!"
```

Here, the inner function “closes over” secret, keeping it alive even after makeSambal finishes. Useful for private data or counters, lah.

# 5. Prototypes: Family Tree of Objects

Prototypes are like your kampung family tree—every object inherits traits from its “parent” (prototype). JavaScript uses this to share properties and methods.

Every object has a prototype` linked to it. You can “borrow” stuff from it.

Example:

```
javascript

let food = { tasty: true };
// food prototype is Object.prototype
console.log(food.toString()); // Inherited from Object.prototype
```

Custom Prototype:
```
javascript

function Stall(name) {
  this.name = name;
}
Stall.prototype.sell = function() {
  return `${this.name} sells roti!`;
};
let myStall = new Stall("Ali");
myStall.sell(); // "Ali sells roti!"
```

Modern way? Use class (ES6+), but it’s just fancy syntax for prototypes:

```
javascript

class Stall {
  constructor(name) {
    this.name = name;
  }
  sell() {
    return `${this.name} sells roti!`;
  }
}
```

# 6. this: Who’s Talking Lah?

this is like pointing to “who’s doing the action” in your kampung story. Its value depends on how a function is called:

Global context: this is window (in browsers) or undefined (in strict mode).
```
javascript

console.log(this); // window (in browser, non-strict)
```

Object method:

 this is the object calling the method.
 ```
javascript

let stall = {
  name: "Makcik",
  sell: function() {
    return `${this.name} sells food!`;
  }
};
stall.sell(); // "Makcik sells food!"
```

Constructor:

 this is the new object being made.
```
javascript

function Stall(name) {
  this.name = name;
  return this;
}
```

Gotcha: In callbacks or loose functions, this can get confusing. Use bind(), call(), or arrow functions to control it:
```
javascript

let stall = {
  name: "Ali",
  sell: function() {
    setTimeout(() => console.log(this.name), 1000);
  }
};
stall.sell(); // "Ali" (arrow function keeps 'this' as stall)
```

# 7. Async/Await and Promises: Waiting for Nasi

JavaScript is single-threaded, but it can handle waiting tasks like waiting for your mee goreng without stopping the whole stall). Promises and async/await make this smooth.

Promises: Like promising to deliver food. It’s either pending, fulfilled (done), or rejected (failed).
```
javascript

let cookPromise = new Promise((resolve, reject) => {
  setTimeout(() => resolve("Nasi siap!"), 1000);
  // or reject("Burnt lah!") if fail
  if (false) {
    reject("Burnt lah!");
  }
});
cookPromise.then(food => console.log(food)).catch(err => console.log(err));
(error => console.log(error));
```

Async/Await (ES6+): Cleaner way to handle Promises, like ordering food and waiting politely:

```
javascript

async function cook() {
  try {
    let food = await cookPromise;
    return console.log(food); // "Nasi siap!"
  } catch (err) {
    console.log(err);
  }
}
cook();
```

Use async/await for readable code, but it’s just Promises underneath, lah.

# 8. ES6+ Features: Modern Kampung Coding

ES6 (2015) and later brought cool stuff to JavaScript, like upgrading your kampung house with Wi-Fi and aircond.

Arrow Functions:

Short syntax, no own this (uses parent’s this). Great for callbacks.
```
javascript

  let add = (a, b) => x + y; a + b;
  add(2, 3); // 5
  // Or with block:
  let greet = name => { return `Jom, ${name}!`; };
  greet("Ali"); // "Jom, Ali!"
```

Destructuring:

Unpack stuff like taking out snacks from a goodie bag.
```
javascript

  let stall = { name: "Makcik", food:" "roti"};
  let { name, food } = stall; // name = "Makcik", food = "roti"
  let [a, b] = [1, 2]; // a = 1, b = 2
```

Modules:

Split code into files, like separate stalls in a pasar. Export and import stuff.
```
javascript

// File: food.js
export const dish = "nasi lemak";
export function sell() {
  return "Come buy lah!";
};

// File: main.js
import { dish, sell } from "./food.js";
console.log(dish); // "nasi lemak"
sell(); // "Come buy lah!"
```

Other ES6+ goodies: template literals (hello ${name}), spread/rest (...), let/const, and classes (mentioned earlier).
Kampung Summary
JavaScript is like running a kampung stall, lah:
Variables (let, const) store your stock.
Data types define what you sell (string, number, object).
Functions cook your orders.
Closures keep your recipes.
Prototypes pass down family traits.
this points to who’s running the show.
Promises and async/await handle delivery orders.
ES6+ features like arrow functions, destructuring, and modules make your stall stylo-milo.
If you got blur parts, just holla, and I’ll clear it up kampung-style again, sia. Jom, code like a pro! 😎
