# 10. Node.js Errors

Error handling is an essential part of building reliable applications, and Node.js provides several mechanisms for dealing with errors in an efficient way. By understanding the different types of errors and how to handle them, you can write more robust applications.

Node.js uses both synchronous and asynchronous error handling mechanisms. Since Node.js is event-driven and non-blocking, errors can occur in both synchronous code (where they can be caught immediately) and asynchronous code (where error handling is more complex).

In this section, we will cover:
- Common types of errors in Node.js
- Error handling in both synchronous and asynchronous code
- The `Error` object and custom error classes
- Best practices for error handling

#### a. Types of Errors in Node.js

In Node.js, errors can generally be categorized into several types:

1. **Operational Errors**: Errors that occur during the normal operation of a program. These include network issues, file not found errors, permission errors, etc.
2. **Programmer Errors**: Errors caused by bugs in the code, such as accessing an undefined variable or calling a function that doesn’t exist.
3. **System Errors**: Errors that occur due to issues outside of the application, such as hardware failures or network problems.

#### b. The `Error` Object

In Node.js, all errors are represented as instances of the built-in `Error` class. The `Error` object provides information about what went wrong and can be thrown and caught using the `try/catch` mechanism in synchronous code.

##### Example: Throwing and Catching Errors

```javascript
try {
  throw new Error('Something went wrong!');
} catch (err) {
  console.error('Caught an error:', err.message);
}
```

##### Properties of the `Error` Object:
- **`message`**: A human-readable description of the error.
- **`name`**: The name of the error (defaults to `'Error'`).
- **`stack`**: A stack trace that shows where the error occurred in the code.

#### c. Handling Synchronous Errors

Synchronous code runs in a blocking fashion, so errors can be caught immediately using `try/catch` blocks.

##### Example: Handling Synchronous Errors

```javascript
try {
  // Some synchronous code that could throw an error
  const result = JSON.parse('{ malformed JSON }');
} catch (err) {
  console.error('Error occurred during parsing:', err.message);
}
```

In synchronous code, `try/catch` blocks are straightforward to use and help to prevent the application from crashing.

#### d. Handling Asynchronous Errors

Handling errors in asynchronous code is more challenging because of Node.js's non-blocking nature. There are several patterns for handling errors in asynchronous operations, such as callbacks, promises, and `async/await`.

##### 1. Error Handling in Callbacks

When working with callbacks, errors are typically passed as the first argument to the callback function. This is often referred to as the "error-first" callback pattern.

```javascript
import * as fs from 'fs';

// Example: Handling errors in a callback
fs.readFile('nonexistentfile.txt', 'utf-8', (err, data) => {
  if (err) {
    console.error('Error reading file:', err.message);
    return;
  }
  console.log('File content:', data);
});
```

##### 2. Error Handling in Promises

When using promises, errors can be caught using `.catch()`.

```javascript
// Example: Handling errors in a promise
const asyncOperation = new Promise((resolve, reject) => {
  const errorOccurred = true;
  if (errorOccurred) {
    reject(new Error('Something went wrong!'));
  } else {
    resolve('Operation succeeded');
  }
});

asyncOperation
  .then((result) => {
    console.log(result);
  })
  .catch((err) => {
    console.error('Error in promise:', err.message);
  });
```

##### 3. Error Handling with `async/await`

With `async/await`, error handling is similar to synchronous code. You can use `try/catch` blocks to handle errors in asynchronous code.

```javascript
async function readFileAsync() {
  try {
    const data = await fs.promises.readFile('nonexistentfile.txt', 'utf-8');
    console.log('File content:', data);
  } catch (err) {
    console.error('Error reading file:', err.message);
  }
}

readFileAsync();
```

#### e. Custom Error Classes

Node.js allows you to create custom error classes that extend the built-in `Error` class. This can be useful for defining specific error types and improving error categorization.

##### Example: Creating a Custom Error Class

```javascript
class ValidationError extends Error {
  constructor(message: string) {
    super(message);
    this.name = 'ValidationError';
  }
}

try {
  throw new ValidationError('Invalid input data');
} catch (err) {
  if (err instanceof ValidationError) {
    console.error('Caught a validation error:', err.message);
  }
}
```

##### Explanation:
- **`ValidationError`**: This custom error class inherits from the `Error` class and can be used to signal validation errors.

#### f. Uncaught Exceptions

When an error occurs but is not caught by any error-handling code, Node.js emits an `uncaughtException` event. Unhandled errors can cause the application to crash. You can listen for `uncaughtException` events to log the error, clean up resources, and exit the application gracefully.

##### Example: Handling Uncaught Exceptions

```javascript
process.on('uncaughtException', (err) => {
  console.error('Uncaught exception:', err.message);
  process.exit(1);  // Exit the process after handling the error
});

// Simulate an uncaught exception
throw new Error('This is an uncaught exception');
```

##### Warning:
- Catching `uncaughtException` should be done cautiously. It's better to handle errors locally in the code, rather than relying on this global handler, as catching an uncaught exception could leave your application in an inconsistent state.

#### g. Unhandled Promise Rejections

In Node.js, when a promise is rejected and no `.catch()` handler is provided, the runtime emits an `unhandledRejection` event. Failing to handle promise rejections can lead to memory leaks or unintended behavior.

##### Example: Handling Unhandled Promise Rejections

```javascript
process.on('unhandledRejection', (reason, promise) => {
  console.error('Unhandled promise rejection:', reason);
  // Optionally exit the process
  process.exit(1);
});

// Simulate an unhandled promise rejection
new Promise((_, reject) => reject(new Error('This promise was rejected but not handled')));
```

#### h. Best Practices for Error Handling in Node.js

1. **Handle Errors Explicitly**: Always handle errors explicitly in your code using `try/catch`, `.catch()`, or callbacks.
2. **Fail Fast**: When encountering critical errors, especially programmer errors (like referencing an undefined variable), fail fast by crashing the application and logging the issue.
3. **Graceful Shutdown**: When handling errors, especially in production, gracefully shut down the application after logging the error to avoid further corruption or inconsistent states.
4. **Use Custom Error Classes**: Create custom error classes for specific categories of errors to make it easier to catch and handle them appropriately.
5. **Log Errors**: Log errors in a consistent and informative format to help with debugging and post-mortem analysis.
6. **Avoid Using `process.exit()` Recklessly**: Exiting the process abruptly without proper cleanup (e.g., closing database connections) can cause issues. Always ensure a graceful shutdown when exiting due to errors.

---