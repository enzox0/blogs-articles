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
2. Open VS code to edit:
```bash
code .
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
This will redirect to NPM login page:
<img width="1888" height="919" alt="image" src="https://github.com/user-attachments/assets/f3757eb6-1ae0-4026-a355-6aedfb79e02f" />


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

# GitHub Setup
Once you have your NPM library ready, connecting it to GitHub is a smart move—it allows version control, collaboration, and CI/CD integration.

---
## 1️⃣ Create a GitHub Repository
1. Go to GitHub and click New Repository.
2. Name your repository (e.g., my-library), add a description.
3. Keep it public (if open-source) or private.
4. Do not initialize with README, .gitignore, or license (do it locally).

---
## 2️⃣ Initialize Git in Your Project
Inside your project folder:
```bash
git init
```
This creates a local Git repository.

---
## 3️⃣ Connect Local Repo to GitHub
After creating the GitHub repo, you’ll see instructions like:

```bash
git remote add origin https://github.com/username/you-library.git
git branch -M main
git push -u origin main
```
Explanation:
- git remote add origin ... → links local repo to GitHub.
- git branch -M main → ensures the main branch is named main.
- git push -u origin main → pushes your code to GitHub.

---
## 4️⃣ Add, Commit, and Push Code
```bash
git add .
git commit -m "Initial commit"
git push
```

---
## 5️⃣ Optional: Add .gitignore
Typical .gitignore for Node.js:
```bash
node_modules
dist
.env
.DS_Store
```

---
## 6️⃣ Optional: Connect to NPM
If you want GitHub to be linked in your package.json:
```bash
{
  "repository": {
    "type": "git",
    "url": "git+https://github.com/username/my-library.git"
  },
  "bugs": {
    "url": "https://github.com/username/my-library/issues"
  },
  "homepage": "https://github.com/username/my-library#readme"
}
```
This helps users find your repo when installing from NPM.

---
## 7️⃣ Optional: Auto-Publish on Push
You can set up GitHub Actions to automatically build and publish your library to NPM when you push a new version tag.





