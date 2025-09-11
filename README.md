# Build-Your-Own-NPM-Library
Learn how to create, package, and publish your own NPM library. This guide covers project setup, writing reusable code, managing dependencies, and publishing to NPM—perfect for beginners and experienced developers alike.

---
## 🛠 Step 1: Setup Your Project
1. Create a new folder for your library:
```bash
mkdir your-library
cd your-library
```

2. Initialize it as an npm package:
```bash
npm init -y
```
This will create a package.json file with default values.

---
## 📂 Step 2: Project Structure
A typical minimal setup:
```perl
my-library/
 ├── src/
 │    └── index.js   # Your library's main code
 ├── package.json
 └── README.md
```

Example src/index.js:
```js
function greet(name) {
  return `Hello, ${name}! 👋`;
}

module.exports = { greet };
```

---
## ⚙️ Step 3: Configure package.json
Make sure main points to your entry file:
```json
{
  "name": "your-library",
  "version": "1.0.0",
  "main": "src/index.js",
  "keywords": ["library", "example"],
  "author": "Your Name",
  "license": "MIT"
}
```

---
## 🧪 Step 4: Test Locally
Before publishing, test your library (Optional):
```bash
npm link
```

Then in another project:
```bash
npm link my-library
```

And use it:
```js
const { greet } = require("my-library");
console.log(greet("Bogart")); // Hello, Bogart! 👋
```

---
## 📦 Step 5: Prepare for Publishing

1. Create a .gitignore (ignore node_modules).
2. Add a README.md with usage instructions.
3. Optionally, add TypeScript support (index.d.ts or compile from .ts).

---
## 🚀 Step 6: Publish to NPM
1. Login to npm (you need an account):
```bash
npm login
```

Publish:
```bash
npm publish --access public
```

### ⚠️ Note:
Package names must be unique in npm.
If taken, prefix it with your scope, e.g., @username/my-library.

---
## 🔄 Step 7: Update Your Library
When you make changes:
```bash
npm version patch   # for bug fixes
npm version minor   # for new features
npm version major   # for breaking changes

npm publish
```
---
## ✅ Bonus (TypeScript + Build Setup)

If you want to ship TypeScript/ESM/CJS support:
- Use Rollup, tsup, or Vite for bundling.
- Add a build script in package.json:
```json
"scripts": {
  "build": "tsup src/index.ts --format cjs,esm --dts"
}
```




