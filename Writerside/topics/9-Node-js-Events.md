# 9. Node.js Events

Node.js is built around an **event-driven architecture**, which is one of its core strengths. Events play a significant role in Node.js by enabling asynchronous and non-blocking communication between different parts of an application. The `EventEmitter` class, part of the `events` module, allows you to create, emit, and listen for events in a Node.js application.

In this section, we'll explore how Node.js events work, how to create and use custom events, and practical examples of their usage.

#### What are Events in Node.js?

In Node.js, an event refers to a specific action or occurrence, such as a user request, a timer completion, or the availability of data. The system "emits" these events, and functions called **listeners** or **handlers** react to them.

Node.js provides the `events` module, which includes the `EventEmitter` class. Objects created from `EventEmitter` can trigger and handle various events. This allows asynchronous, event-based programming, where actions happen in response to certain triggers rather than sequential execution.

#### The EventEmitter Class

The `EventEmitter` class is the core of event handling in Node.js. It allows you to:
- Register event listeners
- Emit (trigger) events
- Remove event listeners

The basic pattern for working with `EventEmitter` involves creating an event emitter instance, defining listeners for specific events, and emitting those events when needed.

##### Example: Basic Usage of EventEmitter

```javascript
import { EventEmitter } from 'events';

// Create an instance of EventEmitter
const eventEmitter = new EventEmitter();

// Define a listener for the 'greet' event
eventEmitter.on('greet', (name) => {
  console.log(`Hello, ${name}!`);
});

// Emit the 'greet' event
eventEmitter.emit('greet', 'Alice');  // Output: Hello, Alice!
```

##### Explanation:
- **`eventEmitter.on()`**: Registers a listener for the `greet` event.
- **`eventEmitter.emit()`**: Emits (triggers) the `greet` event, causing the registered listener to execute.

#### Registering Multiple Event Listeners

You can register multiple listeners for the same event. Each listener will be executed in the order in which it was registered.

##### Example: Multiple Event Listeners

```javascript
import { EventEmitter } from 'events';

const eventEmitter = new EventEmitter();

// Register multiple listeners for the same event
eventEmitter.on('greet', (name) => {
  console.log(`Listener 1: Hello, ${name}!`);
});

eventEmitter.on('greet', (name) => {
  console.log(`Listener 2: How are you, ${name}?`);
});

// Emit the 'greet' event
eventEmitter.emit('greet', 'Bob');

// Output:
// Listener 1: Hello, Bob!
// Listener 2: How are you, Bob?
```

#### Handling Event with Arguments

When emitting an event, you can pass additional arguments to the listeners. The arguments can be used to customize the event's behavior.

##### Example: Passing Arguments to Event Handlers

```javascript
import { EventEmitter } from 'events';

const eventEmitter = new EventEmitter();

// Register a listener with multiple arguments
eventEmitter.on('status', (code, message) => {
  console.log(`Status Code: ${code}, Message: ${message}`);
});

// Emit the 'status' event with arguments
eventEmitter.emit('status', 200, 'OK');  // Output: Status Code: 200, Message: OK
```

#### One-Time Event Listeners

Sometimes, you may want a listener to respond to an event only once. The `once()` method allows you to register a listener that is automatically removed after it is triggered for the first time.

##### Example: One-Time Event Listener

```javascript
import { EventEmitter } from 'events';

const eventEmitter = new EventEmitter();

// Register a one-time listener for the 'greet' event
eventEmitter.once('greet', (name) => {
  console.log(`Hello for the first and only time, ${name}!`);
});

// Emit the 'greet' event twice
eventEmitter.emit('greet', 'Charlie');  // Output: Hello for the first and only time, Charlie!
eventEmitter.emit('greet', 'Charlie');  // No output, listener is removed after the first emit
```

#### Removing Event Listeners

To remove a specific event listener, use the `removeListener()` or `off()` methods. These methods allow you to remove a previously registered listener.

##### Example: Removing Event Listeners

```javascript
import { EventEmitter } from 'events';

const eventEmitter = new EventEmitter();

const greetListener = (name: string) => {
  console.log(`Hello, ${name}!`);
};

// Register the listener
eventEmitter.on('greet', greetListener);

// Emit the 'greet' event
eventEmitter.emit('greet', 'David');  // Output: Hello, David!

// Remove the listener
eventEmitter.off('greet', greetListener);

// Emit the 'greet' event again (no output this time)
eventEmitter.emit('greet', 'David');
```

#### EventEmitter Inheritance

You can inherit from the `EventEmitter` class to create custom classes that emit events. This is useful when you want to add event-driven behavior to your own objects.

##### Example: Custom Class Inheriting EventEmitter

```javascript
import { EventEmitter } from 'events';

class MyEmitter extends EventEmitter {
  log(message: string) {
    console.log(message);
    this.emit('logged', { message });
  }
}

const myEmitter = new MyEmitter();

// Register a listener for the 'logged' event
myEmitter.on('logged', (data) => {
  console.log(`Event emitted with message: ${data.message}`);
});

// Call the custom method which emits the 'logged' event
myEmitter.log('This is a custom event');  // Output:
                                          // This is a custom event
                                          // Event emitted with message: This is a custom event
```

#### Handling Errors in Event Emitters

When an error occurs within an event emitter, it’s a common practice to emit an `'error'` event. If there are no listeners for the `'error'` event, Node.js will throw the error and crash the program. Always ensure there is a listener for error events when emitting errors.

##### Example: Emitting and Handling Errors

```javascript
import { EventEmitter } from 'events';

const eventEmitter = new EventEmitter();

// Register an error event listener
eventEmitter.on('error', (err) => {
  console.error('Error occurred:', err);
});

// Emit an error event
eventEmitter.emit('error', new Error('Something went wrong!'));

// Output: Error occurred: Error: Something went wrong!
```

#### Built-in Node.js Event Emitters

Many Node.js core modules use the `EventEmitter` class internally to handle asynchronous operations. Some examples include:

- **`http`**: The HTTP module uses events to handle incoming requests and responses.
- **`net`**: The Networking module uses events for managing socket connections.
- **`fs`**: The File System module emits events when files are opened, read, or closed.

##### Example: Using Events in the `http` Module

```javascript
import * as http from 'http';

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello, Event-Driven World!');
});

// Register a listener for the 'connection' event
server.on('connection', () => {
  console.log('New connection established');
});

// Start the server
server.listen(3000, () => {
  console.log('Server listening on http://localhost:3000');
});
```

---
