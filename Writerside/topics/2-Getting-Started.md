# 2. Getting Started

#### Download Node.js

To get started with Node.js, you'll first need to download and install it on your system. Node.js comes with its package manager, **npm** (Node Package Manager), which allows you to install libraries and dependencies for your project.

1. Visit the official Node.js website: [https://nodejs.org/](https://nodejs.org/)
2. You'll see two versions available:
    - **LTS (Long Term Support)**: This version is recommended for most users as it is stable and gets security updates for a longer time.
    - **Current**: This is the latest version with new features, but it may not be as stable as the LTS version.

3. Download the appropriate installer for your operating system (Windows, macOS, or Linux).
4. Run the installer and follow the setup instructions.

Once installed, you can verify the installation by opening your terminal (or Command Prompt) and typing:

```bash
node -v
```

This should display the version of Node.js installed.

You can also verify npm by running:

```bash
npm -v
```

This will show the version of npm that was installed alongside Node.js.

#### Command Line Interface (CLI)

Node.js provides a simple command-line interface (CLI) for running JavaScript and TypeScript code. You can interact with Node.js through your terminal or command prompt.

##### 1. Running Node.js REPL (Read-Eval-Print Loop)
Node.js comes with a REPL, which allows you to execute JavaScript code interactively.

To start the REPL, just type `node` in your terminal and press Enter:

```bash
$ node
Welcome to Node.js v16.13.0.
Type ".help" for more information.
> console.log('Hello, Node.js!');
Hello, Node.js!
>
```

Here, you can execute any JavaScript code, and Node.js will evaluate it immediately. To exit the REPL, type `.exit` or press `Ctrl + C` twice.

##### 2. Running Node.js Scripts

To execute a JavaScript or TypeScript file with Node.js, you can use the following command:

```bash
node <filename.js>
```

For example, if you have a file `app.js` that contains:

```javascript
console.log('Hello, World!');
```

You can run it with:

```bash
node app.js
```

This will output:

```
Hello, World!
```

##### 3. Installing Global Packages

Node.js also uses npm for package management. You can install Node.js packages globally (so they are accessible anywhere) using npm:

```bash
npm install -g typescript
```

In this example, we installed **TypeScript** globally, allowing you to use the `tsc` (TypeScript Compiler) command.

#### Initiate the Node.js File

Before creating your first Node.js project, it’s a good practice to set up a project directory and initialize it using npm. This will allow you to manage your dependencies and set up your project structure.

##### 1. Create a New Project Directory

Open your terminal and create a new directory for your project:

```bash
mkdir my-node-app
cd my-node-app
```

##### 2. Initialize the Project with npm

To initialize a new Node.js project, use the following command:

```bash
npm init
```

This command will ask you several questions about your project (like project name, version, entry point, etc.), but you can skip through them by pressing Enter or manually configure them.

Alternatively, you can use the `-y` flag to accept the default settings:

```bash
npm init -y
```

This will generate a `package.json` file, which is a configuration file that stores metadata about your project, including dependencies, scripts, and more.

The `package.json` file will look something like this:

```json
{
  "name": "my-node-app",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

##### 3. Create Your First Node.js File

Now, create a new file called `index.ts` (or `index.js` if you’re using plain JavaScript):

```bash
touch index.ts
```

Inside this file, you can write your first Node.js code using TypeScript:

```typescript
const message: string = 'Hello, TypeScript with Node.js!';
console.log(message);
```

##### 4. Run the TypeScript File

To run TypeScript with Node.js, you need to transpile the TypeScript code into JavaScript using the TypeScript compiler (`tsc`).

First, install TypeScript as a dev dependency:

```bash
npm install typescript --save-dev
```

You can now compile the `index.ts` file into JavaScript:

```bash
npx tsc index.ts
```

This will generate a JavaScript file (`index.js`), which can then be executed using Node.js:

```bash
node index.js
```

The output will be:

```
Hello, TypeScript with Node.js!
```

##### 5. Automate the Compilation and Running (Optional)

To make the process of compiling and running TypeScript easier, you can add a script to your `package.json`:

```json
{
  "scripts": {
    "start": "tsc && node index.js"
  }
}
```

Now, you can compile and run your project with one command:

```bash
npm start
```

This will compile the TypeScript and run the generated JavaScript file.

---

This section guides you through the process of downloading and setting up Node.js, familiarizing you with the command line interface, and creating your first Node.js project.