# 5. Modules

Here’s the updated version of the **Modules** documentation that uses **TypeScript** to illustrate how to work with modules in both **CommonJS** and **ES6** formats.

---
Node.js supports two module systems: **CommonJS** (the original module system used by Node.js) and **ES6 Modules** (the standardized modern module system). This document illustrates how to use modules in **TypeScript**, with examples for both module systems.

#### What is a Module in Node.js?

A **module** in Node.js is a reusable block of code that can be imported into other parts of your application. Modules can contain functions, objects, or entire classes that are encapsulated for reuse across multiple files.

Key points:
- **CommonJS**: The traditional module system for Node.js.
- **ES6 Modules**: A modern, standardized JavaScript module system.
- **TypeScript**: TypeScript adds static typing to JavaScript, enhancing modules with strong types.

---

#### Built-in Modules

Node.js provides built-in modules that offer useful utilities such as working with files, creating HTTP servers, handling paths, and more. You can include and use these modules directly in TypeScript.

Here’s how you can use built-in modules in TypeScript:

```javascript
import * as http from 'http';
import * as fs from 'fs';

const server = http.createServer((req, res) => {
  fs.readFile('index.html', (err, data) => {
    if (err) {
      res.writeHead(404, { 'Content-Type': 'text/plain' });
      res.end('File Not Found');
    } else {
      res.writeHead(200, { 'Content-Type': 'text/html' });
      res.end(data);
    }
  });
});

server.listen(8080, () => {
  console.log('Server running at http://localhost:8080/');
});
```

---

#### Include Modules

TypeScript supports both CommonJS and ES6 module systems. Here’s how to include modules in each system.

##### 1. CommonJS Syntax in TypeScript

You can use `require()` to import modules in a **CommonJS** format:

```javascript
const http = require('http');  // CommonJS syntax
```

##### 2. ES6 Module Syntax in TypeScript

You can use `import` in TypeScript to leverage **ES6 Modules**:

```javascript
import * as http from 'http';  // ES6 module syntax
```

TypeScript automatically understands the syntax and compiles it into appropriate module systems based on your configuration.

---

#### Create Your Own Modules

In TypeScript, you can create custom modules in both **CommonJS** and **ES6** formats. When using TypeScript, you’ll also benefit from static typing and type definitions for better clarity and error-checking.

##### 1. Create a CommonJS Module in TypeScript

In **CommonJS**, you define and export functions or objects using `module.exports`. TypeScript supports this syntax and compiles it accordingly.

###### Example: CommonJS Module in TypeScript

**mathOperations.ts**:
```javascript
function add(a: number, b: number): number {
  return a + b;
}

function subtract(a: number, b: number): number {
  return a - b;
}

module.exports = {
  add,
  subtract
};
```

In this example, the `add` and `subtract` functions are defined and exported using `module.exports`. You can include these functions in other files using `require()`.

##### 2. Create an ES6 Module in TypeScript

With **ES6 modules**, TypeScript allows you to use the `export` keyword to export functionality. This is more modern and aligns with JavaScript’s ES6 standards.

###### Example: ES6 Module in TypeScript

**mathOperations.ts**:
```javascript
export function add(a: number, b: number): number {
  return a + b;
}

export function subtract(a: number, b: number): number {
  return a - b;
}
```

Here, the `export` keyword is used to expose the `add` and `subtract` functions to other files.

---

#### Include Your Own Module

Once you’ve created your custom module, you can include it in other files. TypeScript supports importing both **CommonJS** and **ES6** modules.

##### 1. Including a CommonJS Module in TypeScript

To include a **CommonJS** module in TypeScript, use `require()`:

**app.ts**:
```javascript
const math = require('./mathOperations');

const result = math.add(10, 5);
console.log(`Addition result: ${result}`);
```

In this example, we use `require()` to import the `mathOperations.ts` file and then use the `add` function from the imported module.

##### 2. Including an ES6 Module in TypeScript

To include an **ES6 module** in TypeScript, use `import`:

**app.ts**:
```javascript
import { add, subtract } from './mathOperations';

console.log(`Addition: ${add(10, 5)}`);
console.log(`Subtraction: ${subtract(10, 5)}`);
```

Here, we use the `import` syntax to bring in the `add` and `subtract` functions from `mathOperations.ts`. TypeScript’s static typing ensures that only the exported members are accessible, and type checking is enforced.

##### Folder Structure:
Your project might have the following folder structure when working with modules in TypeScript:

```
/my-project
  ├── /modules
  │     └── mathOperations.ts
  ├── app.ts
  └── tsconfig.json
```

In this structure, you can import your custom modules like this:

```javascript
import { add } from './modules/mathOperations';  // ES6
```

```javascript
const math = require('./modules/mathOperations');  // CommonJS
```

##### tsconfig.json Configuration:
To ensure TypeScript compiles your code correctly, you’ll need a `tsconfig.json` file, which defines TypeScript compiler options, including how to handle modules.

Here’s an example configuration:

**tsconfig.json**:
```json
{
  "compilerOptions": {
     "target": "ES6", /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */
     "module": "CommonJS", /* Specify what module code is generated. */
     "rootDir": "./src", /* Specify the root folder within your source files. */
     "outDir": "./dist", /* Specify an output folder for all emitted files. */
     "forceConsistentCasingInFileNames": true, /* Ensure that casing is correct in imports. */
     "esModuleInterop": true, /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables 'allowSyntheticDefaultImports' for type compatibility. */
     "strict": true, /* Enable all strict type-checking options. */
     "skipLibCheck": true /* Skip type checking all .d.ts files. */
  },
  "include": ["./**/*.ts"],
  "exclude": ["node_modules", "dist"]
}
```

This configuration ensures that TypeScript compiles `.ts` files into the `dist` directory, and it compiles based on the module system you specify (`commonjs` or `esnext` for ES6).

---

### Summary:

1. **What is a Module?**
    - A reusable block of code in Node.js. Modules help organize code efficiently.

2. **Built-in Modules**:
    - Node.js provides various built-in modules like `http`, `fs`, and `path`.

3. **Include Modules**:
    - Use `require()` for **CommonJS** modules.
    - Use `import` for **ES6** modules.

4. **Create Your Own Modules**:
    - **CommonJS**: Use `module.exports` to export code.
    - **ES6**: Use the `export` keyword to export code.

5. **Include Your Own Module**:
    - **CommonJS**: Use `require()` to import modules.
    - **ES6**: Use `import` to bring in modules.

6. **TypeScript Integration**:
    - TypeScript adds static typing to both **CommonJS** and **ES6** modules.
    - Use `tsconfig.json` to configure the TypeScript compiler based on your preferred module system.

---