# 4. Typescript Configuration

### TypeScript Configuration for a Node.js App with `tsx`

To configure TypeScript in a Node.js project, and add `tsx` (a tool that allows running TypeScript files without needing to transpile manually), we need to make sure the project has the appropriate dependencies and configurations in place. Additionally, we'll create scripts for starting, developing, and building the app.

### 1. Setting Up the Node.js App

Start by initializing a new Node.js project if you haven't done so already:

```bash
mkdir my-node-app
cd my-node-app
npm init -y
```

### 2. Installing Dependencies

Next, you'll need to install TypeScript, `tsx`, and types for Node.js:

```bash
npm install typescript tsx @types/node --save-dev
```

- **typescript**: The TypeScript compiler (to compile `.ts` files to JavaScript).
- **tsx**: A package that allows you to run TypeScript and JSX (React) files directly without needing to compile.
- **@types/node**: Provides TypeScript definitions for Node.js modules (like `http`, `fs`, etc.).

### 3. Creating the `tsconfig.json`

TypeScript requires a `tsconfig.json` file for configuration. This file tells the TypeScript compiler how to transpile the `.ts` files. You can create it by running the following command:

```bash
npx tsc --init
```

Then, modify the `tsconfig.json` file to suit a typical Node.js environment:

**`tsconfig.json`**:
```json
{
  "compilerOptions": {
    "target": "ES6",                     // Target ES6 or later to support modern JavaScript features
    "module": "CommonJS",                 // Use CommonJS modules for Node.js
    "rootDir": "./src",                   // Source files location
    "outDir": "./dist",                   // Output directory for compiled files
    "esModuleInterop": true,              // Enables support for `import` in CommonJS
    "strict": true,                       // Enables strict type-checking options
    "skipLibCheck": true,                 // Skips type checking of declaration files
    "moduleResolution": "node",           // For resolving node modules
    "resolveJsonModule": true,            // Enable importing `.json` files
    "allowSyntheticDefaultImports": true, // Allow default imports from modules
    "forceConsistentCasingInFileNames": true // Ensure consistent casing in file names
  },
  "include": ["src/**/*.ts"],             // Include all TypeScript files in the `src` folder
  "exclude": ["node_modules"]             // Exclude `node_modules`
}
```

### 4. Project Structure

Here's an example of how your project structure should look:

```
/my-node-app
  ├── /src
  │     ├── index.ts
  ├── /dist                  # Will be generated after build
  ├── package.json
  ├── tsconfig.json
  └── node_modules
```

### 5. Creating Scripts for Start, Dev, and Build

Next, let's modify the `package.json` file to include scripts for starting, developing, and building the app:

**`package.json`**:
```json
{
  "name": "my-node-app",
  "version": "1.0.0",
  "main": "dist/index.js",
  "scripts": {
    "start": "node dist/index.js",              // Start the compiled JavaScript code
    "dev": "tsx watch src/index.ts",            // Start the app in development mode with hot-reloading
    "build": "tsc"                              // Compile TypeScript files to JavaScript
  },
  "devDependencies": {
    "@types/node": "^18.0.0",
    "tsx": "^3.12.7",
    "typescript": "^4.4.3"
  }
}
```

#### Explanation of the scripts:

- **`start`**: This runs the app using `node` after the TypeScript files have been compiled to JavaScript (found in the `dist` directory).

- **`dev`**: This runs the app directly with `tsx`, allowing you to start the TypeScript app in development mode without needing to compile first. `tsx watch` enables hot-reloading so that the server automatically reloads whenever you make changes.

- **`build`**: This runs the TypeScript compiler (`tsc`), which compiles the TypeScript files from the `src` directory into JavaScript in the `dist` directory.

### 6. Example TypeScript File

Now, let's create a simple `index.ts` file in the `src` folder to demonstrate the setup:

**`src/index.ts`**:
```javascript
import http from 'http';

const hostname = 'localhost';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello, TypeScript with Node.js!\n');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

### 7. Running the App

#### Development Mode:
To run the app in development mode with hot-reloading (using `tsx`), run:

```bash
npm run dev
```

This will start the server, and any changes made to your TypeScript files will automatically restart the server.

#### Build and Start:
To compile your TypeScript code to JavaScript and then run the app:

1. First, build the app:

   ```bash
   npm run build
   ```

   This will transpile the TypeScript files and output them to the `dist` directory.

2. Then, run the app:

   ```bash
   npm start
   ```

This will execute the compiled JavaScript files in the `dist` directory.

### Summary

1. **Install Dependencies**: Install `typescript`, `tsx`, and `@types/node` to set up TypeScript and run TypeScript files in Node.js without manual compilation.
2. **`tsconfig.json`**: Configure TypeScript settings for a Node.js project with `"module": "CommonJS"` and `"target": "ES6"`.
3. **Scripts**:
    - `start`: Runs the compiled app using `node`.
    - `dev`: Runs the app directly in TypeScript with `tsx` in watch mode.
    - `build`: Compiles TypeScript files to JavaScript.
4. **Project Structure**: Keep TypeScript source files in a `src` directory and output compiled files into a `dist` directory.

This setup provides an efficient workflow for TypeScript development in a Node.js environment, leveraging `tsx` for development and `tsc` for building production-ready code.