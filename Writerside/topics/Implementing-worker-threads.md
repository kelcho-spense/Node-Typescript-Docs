# 12.1 Implementing worker threads

To utilize worker threads in Node.js using TypeScript and TSX, follow the steps below.

### Project Setup

1. **Initialize the Project:**

   Create a new directory for your project and initialize it with `npm`.

   ```bash
   mkdir worker-threads-ts
   cd worker-threads-ts
   npm init -y
   ```

2. **Install Dependencies:**

   Install the necessary dependencies, including TypeScript and TSX.

   ```bash
   npm install typescript tsx --save-dev
   ```

3. **Initialize TypeScript Configuration:**

   Generate a `tsconfig.json` file with the following command:

   ```bash
   npx tsc --init
   ```

   Update the `tsconfig.json` to support ES Modules and other necessary settings:

   ```json
   {
     "compilerOptions": {
       "target": "ES2020",
       "module": "ESNext",
       "moduleResolution": "node",
       "outDir": "./dist",
       "rootDir": "./src",
       "strict": true,
       "esModuleInterop": true,
       "forceConsistentCasingInFileNames": true,
       "skipLibCheck": true
     },
     "include": ["src"]
   }
   ```

4. **Update `package.json`:**

   Add a script to run your TypeScript files using TSX.

   ```json
   {
     "scripts": {
       "start": "tsx src/main.ts"
     }
   }
   ```

5. **Project Structure:**

   Create a `src` directory to house your TypeScript files.

   ```bash
   mkdir src
   ```

### Basic Worker Thread Example

Below is a simple example demonstrating how to create a worker thread that executes a separate TypeScript file.

**1. Create the Worker Script (`worker.ts`):**

```javascript
// src/worker.ts
import { parentPort } from 'worker_threads';

// Define the type of tasks the worker can handle
type Task = number;

// Function to calculate factorial
function factorial(n: number): number {
  if (n === 0 || n === 1) return 1;
  return n * factorial(n - 1);
}

// Listen for messages from the main thread
parentPort?.on('message', (task: Task) => {
  console.log(`Worker received task: ${task}`);
  
  const result = factorial(task);
  
  // Send the result back to the main thread
  parentPort?.postMessage(result);
});
```

**2. Create the Main Script (`main.ts`):**

```javascript
// src/main.ts
import { Worker } from 'worker_threads';
import path from 'path';
import { fileURLToPath } from 'url';

// Helper to get __dirname in ES Modules
const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

// Define the type for worker results
type WorkerResult = number;

// Function to run a worker
function runWorker(task: number): Promise<WorkerResult> {
  return new Promise((resolve, reject) => {
    const workerPath = path.resolve(__dirname, './worker.ts');
    const worker = new Worker(workerPath, {
      // Specify the TypeScript file; TSX handles transpilation
      // No additional execArgv needed for TSX
    });

    // Send task to the worker
    worker.postMessage(task);

    // Listen for messages from the worker
    worker.on('message', (result: WorkerResult) => {
      resolve(result);
    });

    // Handle errors
    worker.on('error', reject);

    // Handle worker exit
    worker.on('exit', (code) => {
      if (code !== 0)
        reject(new Error(`Worker stopped with exit code ${code}`));
    });
  });
}

// Example usage
async function main() {
  const number = 10;
  console.log(`Calculating factorial of ${number} in worker thread...`);

  try {
    const result = await runWorker(number);
    console.log(`Factorial of ${number} is ${result}`);
  } catch (err) {
    console.error(err);
  }
}

main();
```

**3. Run the Application:**

Execute the following command to run the main script using the `start` script defined in `package.json`:

```bash
npm run start
```

**Output:**

```
Calculating factorial of 10 in worker thread...
Worker received task: 10
Factorial of 10 is 3628800
```

### Explanation:

- **worker.ts:**
    - Uses TypeScript's type annotations for better type safety.
    - Listens for messages (`Task` type) from the main thread.
    - Calculates the factorial of the received number and sends the result back.

- **main.ts:**
    - Creates a new worker thread by instantiating the `Worker` class with the path to `worker.ts`.
    - Sends a number to the worker thread to compute its factorial.
    - Awaits the result and logs it to the console.
    - Includes error handling for worker errors and exit codes.

### Communication Between Main Thread and Worker

Worker threads communicate with the main thread using messages. They can send and receive messages using the `postMessage` method and the `message` event.

#### Example: Sending and Receiving Multiple Messages

**1. Update `worker.ts` to Handle Multiple Tasks:**

```javascript
// src/worker.ts
import { parentPort } from 'worker_threads';

// Define the types for tasks and messages
type Task = number | 'exit';
type Message = number;

// Function to calculate factorial
function factorial(n: number): number {
  if (n === 0 || n === 1) return 1;
  return n * factorial(n - 1);
}

// Listen for messages from the main thread
parentPort?.on('message', (task: Task) => {
  if (task === 'exit') {
    parentPort?.close();
  } else {
    console.log(`Worker received task: ${task}`);
    const result = factorial(task);
    parentPort?.postMessage(result);
  }
});
```

**2. Update `main.ts` to Send Multiple Tasks:**

```javascript
// src/main.ts
import { Worker } from 'worker_threads';
import path from 'path';
import { fileURLToPath } from 'url';

// Helper to get __dirname in ES Modules
const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

// Define the type for worker results
type WorkerResult = number;

// Function to run a worker
function runWorker(task: number | 'exit'): Promise<WorkerResult> {
  return new Promise((resolve, reject) => {
    const workerPath = path.resolve(__dirname, './worker.ts');
    const worker = new Worker(workerPath, {
      // Specify the TypeScript file; TSX handles transpilation
    });

    // Send task to the worker
    worker.postMessage(task);

    // Listen for messages from the worker
    worker.on('message', (result: WorkerResult) => {
      resolve(result);
    });

    // Handle errors
    worker.on('error', reject);

    // Handle worker exit
    worker.on('exit', (code) => {
      if (code !== 0)
        reject(new Error(`Worker stopped with exit code ${code}`));
    });
  });
}

// Example usage
async function main() {
  const tasks: number[] = [5, 7, 10];
  const promises = tasks.map(runWorker);

  try {
    const results = await Promise.all(promises);
    tasks.forEach((task, index) => {
      console.log(`Factorial of ${task} is ${results[index]}`);
    });
  } catch (err) {
    console.error(err);
  }
}

main();
```

**3. Run the Application:**

```bash
npm run start
```

**Output:**

```
Worker received task: 5
Worker received task: 7
Worker received task: 10
Factorial of 5 is 120
Factorial of 7 is 5040
Factorial of 10 is 3628800
```

### Explanation:

- **worker.ts:**
    - Enhanced to handle multiple tasks by accepting an array of numbers.
    - Listens for the `'exit'` message to gracefully close the worker.

- **main.ts:**
    - Sends multiple tasks to separate worker threads by mapping over the `tasks` array.
    - Awaits all promises using `Promise.all` and logs each result accordingly.

## Practical Example: CPU-Intensive Task

To illustrate the benefits of worker threads, let's compare a CPU-intensive task executed in the main thread versus using worker threads.

**Task:** Calculate the Fibonacci number for a large `n` (e.g., `n = 40`).

### 1. Without Worker Threads (Single-Threaded)

**fibonacci.ts:**

```javascript
// src/fibonacci.ts
function fibonacci(n: number): number {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

const n = 40;
console.log(`Fibonacci of ${n} is ${fibonacci(n)}`);
```

**Run the Script:**

Execute the script using TSX.

```bash
tsx src/fibonacci.ts
```

**Output:**

```
Fibonacci of 40 is 102334155
```

**Note:** While this computation is running, the main thread is blocked and cannot handle other tasks.

### 2. Using Worker Threads (Multithreaded)

**worker-fibonacci.ts:**

```javascript
// src/worker-fibonacci.ts
import { parentPort } from 'worker_threads';

// Function to calculate Fibonacci
function fibonacci(n: number): number {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

// Listen for messages from the main thread
parentPort?.on('message', (n: number) => {
  const result = fibonacci(n);
  parentPort?.postMessage(result);
});
```

**main-fibonacci.ts:**

```javascript
// src/main-fibonacci.ts
import { Worker } from 'worker_threads';
import path from 'path';
import { fileURLToPath } from 'url';

// Helper to get __dirname in ES Modules
const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

// Define the type for worker results
type WorkerResult = number;

// Function to run the Fibonacci worker
function runFibonacci(n: number): Promise<WorkerResult> {
  return new Promise((resolve, reject) => {
    const workerPath = path.resolve(__dirname, './worker-fibonacci.ts');
    const worker = new Worker(workerPath, {
      // Specify the TypeScript file; TSX handles transpilation
    });

    // Send task to the worker
    worker.postMessage(n);

    // Listen for messages from the worker
    worker.on('message', (result: WorkerResult) => {
      resolve(result);
    });

    // Handle errors
    worker.on('error', reject);

    // Handle worker exit
    worker.on('exit', (code) => {
      if (code !== 0)
        reject(new Error(`Worker stopped with exit code ${code}`));
    });
  });
}

// Example usage
async function main() {
  const n = 40;
  console.log(`Calculating Fibonacci of ${n} using worker thread...`);

  const fibPromise = runFibonacci(n);
  
  // Meanwhile, main thread can perform other tasks
  console.log('Main thread is free to perform other tasks.');

  try {
    const result = await fibPromise;
    console.log(`Fibonacci of ${n} is ${result}`);
  } catch (err) {
    console.error(err);
  }
}

main();
```

**3. Run the Script:**

```bash
tsx src/main-fibonacci.ts
```

**Output:**

```
Calculating Fibonacci of 40 using worker thread...
Main thread is free to perform other tasks.
Fibonacci of 40 is 102334155
```

### Explanation:

- **Single-Threaded Approach:**
    - The main thread is blocked while calculating the Fibonacci number, making the application unresponsive during the computation.

- **Worker Thread Approach:**
    - The computation is offloaded to a worker thread.
    - The main thread remains free to handle other tasks, enhancing responsiveness and efficiency.

## Conclusion

Worker threads in Node.js provide a powerful way to handle CPU-intensive tasks without blocking the main event loop. By leveraging multithreading with TypeScript and TSX, applications can achieve better performance, efficient resource utilization, and improved scalability. Understanding and implementing worker threads is essential for building high-performance Node.js applications that can handle a wide range of workloads effectively.
