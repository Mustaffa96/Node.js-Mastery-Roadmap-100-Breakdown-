Oi, bro, let’s dive into this Node.js installation and package manager vibe, Kampung Style! Picture us chilling under a coconut tree, sipping teh tarik, while setting up some coding magic on your laptop. We’ll keep it relaxed, fun, and straight-up like we’re just lepak-ing at the mamak stall. Let’s go step-by-step, with that kampung swagger.

# 1. Install Node.js – Like Building a Kampung House

Node.js is like the foundation of your coding rumah. Without it, your JavaScript can’t run outside the browser, macam ayam takde kandang. Here’s how to set it up, no stress.

* **Step 1: Check Your OS, Lah**

  Whether you’re on Windows, macOS, or Linux, Node.js got you covered. It’s like picking the right kain for your baju – one size fits all, but you gotta download the right one.

* **Step 2: Download Node.js**

  Head to the official Node.js website. You’ll see two versions:

   * **LTS (Long-Term Support):** This one’s stable, macam your mak’s nasi lemak recipe. Pick this for most projects.
  
   * **Current:** This is the cutting-edge version, like trying your cousin’s experimental rendang. Fun, but maybe buggy.

Click the big green button for LTS, download the installer, and run it. It’s like hammering nails into your kampung house – just follow the wizard, click “Next” sampai selesai.

  * **Step 3: Verify Installation**

After installing, open your terminal (Windows: Command Prompt or PowerShell; macOS/Linux: Terminal). Type these commands to check if Node.js and npm (Node Package Manager) are ready to rock:

```bash
node -v
npm -v
```

If you see version numbers (e.g., v20.17.0 for Node, 10.8.2 for npm), you’re golden, bro! If not, something’s wrong – maybe reinstall or holler for help.

# 2. Using npm, yarn, or pnpm – Like Choosing Your Pasar Ingredients

Node.js comes with npm, but you got options like yarn or pnpm, macam picking between ikan kembung, ayam, or daging at the pasar. Each one’s a package manager to handle your project’s dependencies (libraries, tools, etc.). Let’s break it down, kampung style.

**npm – The OG, Like Your Mak’s Sambal**

npm is the default package manager, solid and reliable like your mak’s sambal recipe. It’s already installed with Node.js, so no extra work.

* **Initialize a Project:**

Create a folder for your project, like `kampung-app`, and run:

```bash
mkdir kampung-app
cd kampung-app
npm init -y
```

This creates a `package.json` file, macam your project’s resipi book, listing all your dependencies.

* **Install a Package:**

Wanna add a library, like express (for building web apps)? Run:

```bash
npm install express
```

This downloads `express` and adds it to `node_modules` and `package.json`. It’s like buying cili padi for your sambal – now you’re ready to cook.

* **Run Scripts:**
In `package.json`, you can define scripts. For example, add this under `"scripts"`:

```json
"start": "node index.js"
```

Then, run:

```bash
npm start
```

It’s like shouting “Makan time!” and your app runs.

**Yarn – The Hip Cousin, Like Adding Extra Gula to Your Teh Tarik**

Yarn is faster and fancier than npm, with better caching, macam your cousin who shows up with a new phone. To install Yarn:

```bash
npm install -g yarn
```

* **Initialize a Project:**

Same vibe as npm:

```bash
yarn init -y
```

* **Install a Package:**

Add express with:

```bash
yarn add express
```

* **Run Scripts:**

Same as npm, just use:

```bash
yarn start
```

Yarn’s got a cleaner output and feels smoother, like a well-paved kampung road.

**pnpm – The Lightweight Atuk, Like Using a Bicycle Instead of a Car**

pnpm is super efficient, saving disk space and running fast, macam your atuk who’s still fit riding his basikal. To install pnpm:

```bash
npm install -g pnpm
```

* **Initialize a Project:**

```bash
pnpm init
```

* **Install a Package:**

```bash
pnpm add express
```

* **Run Scripts:**

```bash
pnpm start
```

pnpm’s cool because it reuses packages across projects, like sharing kuih with the whole kampung.

**Which One to Pick?**

* npm: Stick with it if you’re new or want no hassle.
* Yarn: Go for it if you want speed and a modern vibe.
* pnpm: Choose this if you’re working on big projects or wanna save space.

# 3. Running Basic Scripts – Like Cooking Nasi Goreng Kampung
Now that you got Node.js and a package manager, let’s write and run a simple script. It’s like whipping up nasi goreng with whatever’s in the fridge.

* **Create a Script:**
Make a file called index.js in your kampung-app folder:

```javascript
console.log("Apa khabar, kampung! Let’s code!");
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Selamat datang to my kampung app!");
});

app.listen(3000, () => {
  console.log("Server running at http://localhost:3000, bro!");
});
```

* **Run the Script:**
If you didn’t set up a `"start"` script in `package.json`, you can run it directly:

```bash
node index.js
```

Or, if you added the `"start"` script, use:

```bash
npm start
```

(Or `yarn start`/`pnpm start` if you’re using those.)

* **Check It Out:**
Open your browser and go to http://localhost:3000. You’ll see “Selamat datang to my kampung app!” It’s like serving your nasi goreng to the whole kampung – simple but satisfying.

# 4. Kampung Tips for Smooth Coding

* **Keep Node Updated:** Run npm install -g npm or check the Node.js site for updates, macam sharpening your parang.

* **Clear Cache if Problems:** If npm/yarn/pnpm acts up, clear the cache:
  * npm: npm cache clean --force
  * Yarn: yarn cache clean
  * pnpm: pnpm store prune

* **Use .gitignore:** Don’t upload node_modules to GitHub. Add it to .gitignore, like keeping your dirty laundry out of sight.

* **Learn More:** Check out Node.js docs or npm docs for deeper knowledge, macam listening to your nenek’s stories.

# 5. Wrap-Up, Kampung Style

There you go, bro! You’ve installed Node.js, picked your package manager (npm, yarn, or pnpm), and ran a basic script, all while chilling kampung style. It’s like building a small rumah, stocking the dapur with ingredients, and cooking a quick dish for your friends. Now, go explore, write more code, and make your kampung app the talk of the town!

