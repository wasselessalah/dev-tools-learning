# 📦 Node.js Modules

## 📌 Overview

A **module** in Node.js is a reusable piece of code that can be imported and used in another file or project.

Modules help you:

* Organize code
* Reuse functionality
* Separate responsibilities
* Make large projects easier to maintain
* Avoid putting everything into one file

Example:

```text
project/
├── src/
│   ├── app.js
│   ├── math.js
│   └── user.js
└── package.json
```

Instead of putting all logic inside `app.js`, you can create separate modules.

---

# 🧠 Types of Node.js Modules

Node.js modules can generally be divided into three categories:

```text
Node.js Modules
│
├── Core Modules
├── Local Modules
└── Third-party Modules
```

## 1. Core Modules

Built into Node.js.

Examples:

```text
fs
path
http
os
url
crypto
events
stream
util
```

No installation is required.

---

## 2. Local Modules

Modules that you create yourself.

Example:

```text
src/
├── app.js
└── math.js
```

---

## 3. Third-party Modules

Packages installed from npm.

Examples:

```text
express
axios
jsonwebtoken
prisma
dotenv
```

Install a package:

```bash
npm install express
```

Then import it into your application.

---

# 🔵 CommonJS Modules

CommonJS is the traditional module system used by Node.js.

It uses:

```javascript
require()
```

and:

```javascript
module.exports
```

---

# 📤 Exporting with CommonJS

Create:

```text
math.js
```

```javascript
function add(a, b) {
  return a + b;
}

function subtract(a, b) {
  return a - b;
}

module.exports = {
  add,
  subtract,
};
```

---

# 📥 Importing with CommonJS

In another file:

```text
app.js
```

```javascript
const math = require("./math");

console.log(math.add(10, 5));
console.log(math.subtract(10, 5));
```

Output:

```text
15
5
```

---

# 🎯 Destructuring CommonJS Imports

Instead of:

```javascript
const math = require("./math");

console.log(math.add(10, 5));
```

You can use:

```javascript
const { add, subtract } = require("./math");

console.log(add(10, 5));
console.log(subtract(10, 5));
```

---

# 📦 Exporting a Single Function

You can export one function directly:

```javascript
function add(a, b) {
  return a + b;
}

module.exports = add;
```

Then:

```javascript
const add = require("./math");

console.log(add(10, 5));
```

---

# 📦 Exporting Multiple Values

```javascript
const name = "Wassel";

function hello() {
  return `Hello ${name}`;
}

module.exports = {
  name,
  hello,
};
```

Import:

```javascript
const { name, hello } = require("./user");

console.log(name);
console.log(hello());
```

---

# 🟢 ES Modules

Modern JavaScript and Node.js also support **ES Modules (ESM)**.

ES Modules use:

```javascript
import
```

and:

```javascript
export
```

Example:

```javascript
export function add(a, b) {
  return a + b;
}
```

Import:

```javascript
import { add } from "./math.js";

console.log(add(10, 5));
```

---

# ⚙️ Enabling ES Modules

There are several ways to use ESM in Node.js.

A common approach is adding:

```json
{
  "type": "module"
}
```

to:

```text
package.json
```

Example:

```json
{
  "name": "node-app",
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "start": "node src/app.js"
  }
}
```

Now Node.js treats `.js` files as ES Modules.

---

# 📤 Named Exports

Example:

```javascript
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}
```

Import:

```javascript
import { add, subtract } from "./math.js";

console.log(add(10, 5));
console.log(subtract(10, 5));
```

---

# 📦 Default Export

You can export one main value as the default:

```javascript
export default function add(a, b) {
  return a + b;
}
```

Import:

```javascript
import add from "./math.js";

console.log(add(10, 5));
```

The imported name can be different:

```javascript
import calculate from "./math.js";
```

---

# 🔀 Named vs Default Export

### Named export

```javascript
export function add() {}
```

Import:

```javascript
import { add } from "./math.js";
```

### Default export

```javascript
export default function add() {}
```

Import:

```javascript
import add from "./math.js";
```

---

# 🟡 Core Modules

Node.js provides many built-in modules.

You don't need to install them with npm.

---

# 📁 `fs` Module

The `fs` module allows you to work with files.

Import with ESM:

```javascript
import fs from "node:fs";
```

Read a file:

```javascript
const data = fs.readFileSync("hello.txt", "utf8");

console.log(data);
```

Write a file:

```javascript
fs.writeFileSync("hello.txt", "Hello Node.js!");
```

---

# 📂 `path` Module

The `path` module helps work with file and directory paths.

```javascript
import path from "node:path";
```

Example:

```javascript
const filePath = path.join("src", "data", "users.json");

console.log(filePath);
```

On Linux:

```text
src/data/users.json
```

Using `path.join()` helps create platform-compatible paths.

---

# 💻 `os` Module

The `os` module provides information about the operating system.

```javascript
import os from "node:os";
```

Examples:

```javascript
console.log(os.platform());
console.log(os.arch());
console.log(os.cpus().length);
console.log(os.totalmem());
console.log(os.freemem());
```

---

# 🌐 `http` Module

Node.js includes an HTTP server module.

```javascript
import http from "node:http";

const server = http.createServer((req, res) => {
  res.writeHead(200, {
    "Content-Type": "text/plain",
  });

  res.end("Hello Node.js");
});

server.listen(3000, () => {
  console.log("Server running on port 3000");
});
```

Start:

```bash
node app.js
```

Then open:

```text
http://localhost:3000
```

---

# 🔐 `crypto` Module

The `crypto` module provides cryptographic functionality.

```javascript
import crypto from "node:crypto";
```

Example:

```javascript
const hash = crypto
  .createHash("sha256")
  .update("hello")
  .digest("hex");

console.log(hash);
```

> Passwords should normally be hashed using a password-specific algorithm such as Argon2 or bcrypt rather than a plain SHA-256 hash.

---

# 🎯 Third-party Modules

Third-party modules are usually installed from npm.

For example:

```bash
npm install express
```

Then:

```javascript
import express from "express";

const app = express();

app.get("/", (req, res) => {
  res.send("Hello Express");
});

app.listen(3000);
```

Here:

```text
expre
```
