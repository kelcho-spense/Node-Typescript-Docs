# 6. Node.js Globals

Node.js provides several built-in global objects. These are available globally and don't require importing any modules. They can be accessed from any part of your application. Understanding these global objects is crucial for writing effective and efficient Node.js applications.

#### What are Globals?
In Node.js, **Globals** are objects or functions that are available throughout the entire runtime without the need to import or require any additional modules. They can be accessed directly within any module or file without needing to reference them explicitly.

However, it's important to note that Node.js aims to minimize the use of global objects as it encourages modularity and explicit dependencies. Using global objects should be done with caution to avoid issues with maintainability and scope.

#### Common Node.js Global Objects

The following are some of the most commonly used global objects in Node.js:

##### i. `__dirname`
The `__dirname` global object represents the directory name of the current module (i.e., the file containing the running code). It provides the absolute path to the directory where the currently executing file is located.

This is especially useful for handling file paths dynamically. For example, you may need to access resources or files relative to your module, no matter where your application is being run from.

```javascript
import * as path from 'path';
import * as fs from 'fs';

// Example: Use __dirname to handle file paths dynamically
const filePath = path.join(__dirname, 'data', 'example.txt');

// Checking if the file exists before reading
if (fs.existsSync(filePath)) {
  const fileContent = fs.readFileSync(filePath, 'utf-8');
  console.log('File Content:', fileContent);
} else {
  console.log(`File not found at ${filePath}`);
}

// Output __dirname value
console.log('Current directory:', __dirname);
```

- **Use case**: File path operations that need the relative path from the current module's directory, such as reading or writing files within the application.

##### ii. `__filename`
The `__filename` global object provides the absolute path of the current file being executed, including the file name itself. It can be useful in logging, debugging, or when you need to dynamically work with the path of the current script.

```javascript
import * as path from 'path';

// Example: __filename in action
console.log('Full file path:', __filename);

// Extract directory name from __filename
const currentDir = path.dirname(__filename);
console.log('Directory name:', currentDir);

// Extract base file name (e.g., 'index.js') from __filename
const baseFileName = path.basename(__filename);
console.log('Base file name:', baseFileName);

// Extract file extension (e.g., '.js')
const fileExtension = path.extname(__filename);
console.log('File extension:', fileExtension);

// Construct a path relative to this file
const relativePath = path.join(currentDir, 'config', 'settings.json');
console.log('Relative file path:', relativePath);
```

- **Use case**: Dynamic operations involving the file path of the current file, like logging its location, or performing actions based on the current file’s directory.

##### iii. `exports`
In Node.js, `exports` is an object that is used to expose functionalities from a module to other files. It's used to define the public interface of the module. You can export variables, functions, or classes to be available for other files using the `exports` object.

```javascript
// Example: Using exports in a module
export const sayHello = () => {
  console.log('Hello, World!');
};
```

In another file, you can import this function as follows:

```javascript
import { sayHello } from './moduleFile';
sayHello();  // Output: Hello, World!
```

- **Use case**: Sharing functions, variables, or classes from one file to be used in other parts of your application.

##### iv. `module`
The `module` object represents the current module and provides information about it. It's an object that contains properties like `exports` (for exporting the module’s content) and `filename` (to get the absolute path of the module file).

```javascript
// Example: Inspecting the module object
console.log(module);  // Output: Module details (such as exports, id, filename)
```

Using `module.exports`, you can overwrite the default exports object.

##### v. `require()`
`require()` is a built-in function used to include modules. It is used to load and use modules from other files or node modules. In TypeScript, you typically use the `import` syntax, but `require()` is still relevant in some use cases like when working with older JavaScript modules.

```javascript
// Example: Using require() to load a module
const fs = require('fs');
const data = fs.readFileSync('example.txt', 'utf8');
console.log(data);
```

In TypeScript, the equivalent of `require()` is typically:

```javascript
import * as fs from 'fs';
const data = fs.readFileSync('example.txt', 'utf8');
console.log(data);
```

#### ES Global Objects

In addition to the Node.js-specific global objects, Node.js also exposes many of the ECMAScript (ES) global objects that are part of the JavaScript language itself. These objects are available in all JavaScript environments (browser, server-side, etc.), and they include objects like:

- **`console`**: Used for printing information, warnings, or errors to the terminal.

  ```javascript
  console.log('This is a message');
  console.error('This is an error');
  ```

- **`setTimeout()` / `setInterval()`**: Used to execute a function after a specified delay or repeatedly at intervals.

  ```javascript
  setTimeout(() => {
    console.log('This message appears after 2 seconds');
  }, 2000);

  setInterval(() => {
    console.log('This message repeats every 3 seconds');
  }, 3000);
  ```

- **`Promise`**: A global object that represents an asynchronous operation that may complete at some point in the future.

  ```javascript
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Promise resolved!');
    }, 1000);
  });

  promise.then(message => {
    console.log(message);  // Output: Promise resolved!
  });
  ```

- **`Buffer`**: Available globally in Node.js, the `Buffer` class is used to handle binary data.

  ```javascript
  const buffer = Buffer.from('Hello, world!', 'utf8');
  console.log(buffer.toString());  // Output: Hello, world!
  ```

- **`process`**: A Node.js-specific object that provides information about the current process, as well as methods for controlling it.

  ```javascript
  console.log(process.env);  // Outputs environment variables
  process.exit(1);  // Terminates the process with an error code
  ```

These objects are available globally in Node.js as part of the underlying JavaScript language and the Node.js runtime.

---

This section on **Node.js Globals** introduces developers to essential global objects they will frequently encounter when building applications in Node.js. It provides a solid foundation for understanding the Node.js environment and its integration with ECMAScript features.