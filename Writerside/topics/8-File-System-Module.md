# 8. File System Module

Node.js provides a built-in module called `fs` (File System), which allows you to interact with the file system on your server. With this module, you can perform various file operations such as reading, writing, updating, deleting, and renaming files. You can also use Node.js as a file server to handle file requests from clients.

#### Node.js as a File Server

Using Node.js as a file server, you can read and serve files to users, or allow file manipulations like creating, updating, deleting, or renaming files. The `fs` module provides both asynchronous and synchronous methods for these operations. It's important to use asynchronous methods whenever possible to prevent blocking the event loop.

Below are some common operations you can perform with the `fs` module:

## i. Read Files

To read the contents of a file, you can use the `fs.readFile()` method asynchronously. This method reads the entire file into memory.

### Example: Reading a File Asynchronously

```javascript
import * as fs from 'fs';
import * as http from 'http';
import * as path from 'path';

// Define the path to the data directory and the file
const dataDir = path.join(__dirname, 'data');
const filePath = path.join(dataDir, 'example.txt');

// Ensure the data directory exists
if (!fs.existsSync(dataDir)) {
  fs.mkdirSync(dataDir, { recursive: true });
  console.log('Data directory created.');
}

// Example: Serving a file using Node.js
const server = http.createServer((req, res) => {
  // Reading the requested file asynchronously
  fs.readFile(filePath, 'utf-8', (err, data) => {
    if (err) {
      res.writeHead(404, { 'Content-Type': 'text/plain' });
      res.end('File not found');
    } else {
      res.writeHead(200, { 'Content-Type': 'text/plain' });
      res.end(data);
    }
  });
});

// Listen on port 3000
server.listen(3000, () => {
  console.log('Server is running at http://localhost:3000');
});
```

#### Explanation:
- **`fs.existsSync()`**: Checks if the `data` directory exists.
- **`fs.mkdirSync()`**: Creates the `data` directory if it doesn't exist.
- **`path.join()`**: Constructs the file paths in a cross-platform manner.

#### Use Case:
- Serve files like HTML, CSS, JavaScript, or other resources to clients in a web server.

## ii. Create Files

You can create new files in Node.js using `fs.writeFile()` or `fs.appendFile()`. The `writeFile()` method creates a new file and writes content to it, overwriting the file if it already exists, while `appendFile()` adds data to the end of an existing file or creates the file if it doesn’t exist.

### Example: Creating a New File

```javascript
import * as fs from 'fs';
import * as path from 'path';

const dataDir = path.join(__dirname, 'data');
const filePath = path.join(dataDir, 'newfile.txt');

// Ensure the data directory exists
if (!fs.existsSync(dataDir)) {
  fs.mkdirSync(dataDir, { recursive: true });
}

// Creating and writing to a file
fs.writeFile(filePath, 'Hello, this is a newly created file!', (err) => {
  if (err) throw err;
  console.log('File created and written successfully');
});
```

### Example: Appending Data to a File

```javascript
import * as fs from 'fs';
import * as path from 'path';

const dataDir = path.join(__dirname, 'data');
const filePath = path.join(dataDir, 'newfile.txt');

// Appending data to an existing file
fs.appendFile(filePath, '\nAppending more content to the file.', (err) => {
  if (err) throw err;
  console.log('Content appended successfully');
});
```

#### Use Case:
- Creating new files for logging, storing data, or dynamically generating files.

## iii. Update Files

You can update a file by either writing to it again (overwriting the content) using `fs.writeFile()` or appending new content to the existing file using `fs.appendFile()`.

### Example: Overwriting a File

```javascript
import * as fs from 'fs';
import * as path from 'path';

const dataDir = path.join(__dirname, 'data');
const filePath = path.join(dataDir, 'newfile.txt');

// Overwriting the content of a file
fs.writeFile(filePath, 'This content replaces the old one.', (err) => {
  if (err) throw err;
  console.log('File content overwritten successfully');
});
```

### Example: Appending to a File

```javascript
import * as fs from 'fs';
import * as path from 'path';

const dataDir = path.join(__dirname, 'data');
const filePath = path.join(dataDir, 'newfile.txt');

// Appending content to a file
fs.appendFile(filePath, '\nMore content being added.', (err) => {
  if (err) throw err;
  console.log('File updated with new content');
});
```

#### Use Case:
- Updating logs, modifying data files, or appending new data to reports.

## iv. Delete Files

You can delete files using the `fs.unlink()` method.

### Example: Deleting a File

```javascript
import * as fs from 'fs';
import * as path from 'path';

const dataDir = path.join(__dirname, 'data');
const filePath = path.join(dataDir, 'newfile.txt');

// Deleting a file
fs.unlink(filePath, (err) => {
  if (err) throw err;
  console.log('File deleted successfully');
});
```

#### Use Case:
- Removing temporary files or cleaning up old data files that are no longer needed.

## v. Rename Files

To rename a file, use the `fs.rename()` method.

### Example: Renaming a File

```javascript
import * as fs from 'fs';
import * as path from 'path';

const dataDir = path.join(__dirname, 'data');
const oldFilePath = path.join(dataDir, 'oldfile.txt');
const newFilePath = path.join(dataDir, 'newfile.txt');

// Renaming a file
fs.rename(oldFilePath, newFilePath, (err) => {
  if (err) throw err;
  console.log('File renamed successfully');
});
```

#### Use Case:
- Organizing files, managing file versions, or moving files within the same directory.

---

### Complete Example: Node.js HTTPS File Server with CRUD Operations

Below is an enhanced example that:

- Reads and writes files from the `./src/data` folder.
- Creates the `data` folder when the server starts if it doesn't exist.
- Provides endpoints for all CRUD operations.
- Demonstrates how to interact with the server using HTTP methods.

#### Project Structure

```
HTTPS/
├── src/
│   ├── data/
│   └── index.ts
├── package.json
├── tsconfig.json
└── node_modules/
```

#### Step-by-Step Implementation

1. **Initialize the Project**

   Ensure you have initialized your project and installed necessary dependencies.

   ```bash
   npm init -y
   npm install typescript tsx --save-dev
   ```

2. **Configure TypeScript (`tsconfig.json`)**

   Create a `tsconfig.json` in your project root with the following content:

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
     "include": ["src"],
     "exclude": ["node_modules"]
   }
   ```

3. **Update `package.json` Scripts**

   Add the following scripts to your `package.json`:

   ```json
   {
     "name": "https-file-server",
     "version": "1.0.0",
     "type": "module",
     "scripts": {
       "start": "tsx src/index.ts",
       "build": "tsc",
       "serve": "node dist/index.js"
     },
     "devDependencies": {
       "typescript": "^4.0.0",
       "tsx": "^4.19.1"
     }
   }
   ```

4. **Create the HTTPS Server (`src/index.ts`)**

   ```typescript
   import * as https from 'https';
   import * as fs from 'fs';
   import * as path from 'path';
   import { IncomingMessage, ServerResponse } from 'http';
   import { URL } from 'url';

   // Define the path to the data directory
   const dataDir = path.join(__dirname, 'data');

   // Ensure the data directory exists
   if (!fs.existsSync(dataDir)) {
     fs.mkdirSync(dataDir, { recursive: true });
     console.log('Data directory created.');
   }

   // Define SSL certificates
   const options = {
     key: fs.readFileSync(path.join(__dirname, '..', 'server-key.pem')),
     cert: fs.readFileSync(path.join(__dirname, '..', 'server-cert.pem'))
   };

   // Create HTTPS server
   const server = https.createServer(options, (req: IncomingMessage, res: ServerResponse) => {
     const parsedUrl = new URL(req.url || '', `https://${req.headers.host}`);
     const pathname = parsedUrl.pathname;

     // Set CORS headers (optional, for API usage)
     res.setHeader('Access-Control-Allow-Origin', '*');
     res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE, OPTIONS');
     res.setHeader('Access-Control-Allow-Headers', 'Content-Type');

     // Handle preflight requests
     if (req.method === 'OPTIONS') {
       res.writeHead(204);
       res.end();
       return;
     }

     // Routing
     if (pathname === '/' && req.method === 'GET') {
       // Read the empty file
       const filePath = path.join(dataDir, 'data.txt');
       fs.readFile(filePath, 'utf-8', (err, data) => {
         if (err) {
           // If file doesn't exist, send an empty response
           res.writeHead(200, { 'Content-Type': 'application/json' });
           res.end(JSON.stringify({ message: 'File is empty or does not exist.' }));
         } else {
           res.writeHead(200, { 'Content-Type': 'application/json' });
           res.end(JSON.stringify({ content: data }));
         }
       });
     } else if (pathname === '/create' && req.method === 'POST') {
       // Create or overwrite the file with posted data
       let body = '';
       req.on('data', chunk => {
         body += chunk.toString();
       });
       req.on('end', () => {
         const filePath = path.join(dataDir, 'data.txt');
         fs.writeFile(filePath, body, (err) => {
           if (err) {
             res.writeHead(500, { 'Content-Type': 'application/json' });
             res.end(JSON.stringify({ error: 'Failed to write data.' }));
           } else {
             res.writeHead(201, { 'Content-Type': 'application/json' });
             res.end(JSON.stringify({ message: 'Data saved successfully.' }));
           }
         });
       });
     } else if (pathname === '/read' && req.method === 'GET') {
       // Read the file with data
       const filePath = path.join(dataDir, 'data.txt');
       fs.readFile(filePath, 'utf-8', (err, data) => {
         if (err) {
           res.writeHead(404, { 'Content-Type': 'application/json' });
           res.end(JSON.stringify({ error: 'File not found.' }));
         } else {
           res.writeHead(200, { 'Content-Type': 'application/json' });
           res.end(JSON.stringify({ content: data }));
         }
       });
     } else if (pathname === '/update' && req.method === 'PUT') {
       // Update the file by appending data
       let body = '';
       req.on('data', chunk => {
         body += chunk.toString();
       });
       req.on('end', () => {
         const filePath = path.join(dataDir, 'data.txt');
         fs.appendFile(filePath, body, (err) => {
           if (err) {
             res.writeHead(500, { 'Content-Type': 'application/json' });
             res.end(JSON.stringify({ error: 'Failed to update data.' }));
           } else {
             res.writeHead(200, { 'Content-Type': 'application/json' });
             res.end(JSON.stringify({ message: 'Data updated successfully.' }));
           }
         });
       });
     } else if (pathname === '/delete' && req.method === 'DELETE') {
       // Delete the file
       const filePath = path.join(dataDir, 'data.txt');
       fs.unlink(filePath, (err) => {
         if (err) {
           res.writeHead(404, { 'Content-Type': 'application/json' });
           res.end(JSON.stringify({ error: 'File not found.' }));
         } else {
           res.writeHead(200, { 'Content-Type': 'application/json' });
           res.end(JSON.stringify({ message: 'File deleted successfully.' }));
         }
       });
     } else if (pathname === '/rename' && req.method === 'PUT') {
       // Rename the file
       const oldPath = path.join(dataDir, 'data.txt');
       const newPath = path.join(dataDir, 'renamedData.txt');
       fs.rename(oldPath, newPath, (err) => {
         if (err) {
           res.writeHead(500, { 'Content-Type': 'application/json' });
           res.end(JSON.stringify({ error: 'Failed to rename file.' }));
         } else {
           res.writeHead(200, { 'Content-Type': 'application/json' });
           res.end(JSON.stringify({ message: 'File renamed successfully.' }));
         }
       });
     } else {
       res.writeHead(404, { 'Content-Type': 'application/json' });
       res.end(JSON.stringify({ error: 'Route not found.' }));
     }
   });

   // Listen on port 8443 (using a non-privileged port for development)
   const PORT = process.env.PORT || 8443;
   server.listen(PORT, () => {
     console.log(`HTTPS server is running on https://localhost:${PORT}`);
   });
   ```

#### Explanation:

1. **Directory Setup:**
    - **`dataDir`**: Specifies the path to the `data` folder within `./src/data`.
    - **Directory Creation:** Checks if the `data` directory exists; if not, it creates it using `fs.mkdirSync()`.

2. **SSL Certificates:**
    - Ensure that your `server-key.pem` and `server-cert.pem` files are located in the project root (one level above `src`).
    - Adjust the paths accordingly if your certificates are stored elsewhere.

3. **Server Routing:**
    - **`/` (GET):** Reads and returns the content of an empty or non-existent `data.txt` file.
    - **`/create` (POST):** Receives data in the request body and saves it to `data.txt`.
    - **`/read` (GET):** Reads and returns the content of `data.txt`.
    - **`/update` (PUT):** Appends data to `data.txt`.
    - **`/delete` (DELETE):** Deletes `data.txt`.
    - **`/rename` (PUT):** Renames `data.txt` to `renamedData.txt`.

4. **CORS Headers:**
    - Added CORS headers to allow cross-origin requests, which is useful if you interact with the API from different origins.

5. **Port Configuration:**
    - The server listens on port `8443` by default to avoid permission issues. You can change this by setting the `PORT` environment variable.

#### Interacting with the Server

You can interact with the server using tools like **curl**, **Postman**, or any HTTP client.

1. **Read the Empty File (`/` Endpoint)**

   **Request:**

   ```bash
   curl -k https://localhost:8443/
   ```

   **Response:**

   ```json
   {
     "message": "File is empty or does not exist."
   }
   ```

2. **Create/Overwrite the File (`/create` Endpoint)**

   **Request:**

   ```bash
   curl -k -X POST https://localhost:8443/create -d "This is the initial content of the file."
   ```

   **Response:**

   ```json
   {
     "message": "Data saved successfully."
   }
   ```

3. **Read the File with Data (`/read` Endpoint)**

   **Request:**

   ```bash
   curl -k https://localhost:8443/read
   ```

   **Response:**

   ```json
   {
     "content": "This is the initial content of the file."
   }
   ```

4. **Update the File by Appending Data (`/update` Endpoint)**

   **Request:**

   ```bash
   curl -k -X PUT https://localhost:8443/update -d " Appended content."
   ```

   **Response:**

   ```json
   {
     "message": "Data updated successfully."
   }
   ```

5. **Read the Updated File (`/read` Endpoint)**

   **Request:**

   ```bash
   curl -k https://localhost:8443/read
   ```

   **Response:**

   ```json
   {
     "content": "This is the initial content of the file. Appended content."
   }
   ```

6. **Delete the File (`/delete` Endpoint)**

   **Request:**

   ```bash
   curl -k -X DELETE https://localhost:8443/delete
   ```

   **Response:**

   ```json
   {
     "message": "File deleted successfully."
   }
   ```

7. **Rename the File (`/rename` Endpoint)**

   **Note:** Ensure that `data.txt` exists before attempting to rename it.

   **Request:**

   ```bash
   curl -k -X PUT https://localhost:8443/rename
   ```

   **Response:**

   ```json
   {
     "message": "File renamed successfully."
   }
   ```

   **Post-Rename Read:**

   After renaming, attempting to read `data.txt` will result in a 404 error unless you update the endpoints accordingly.

#### Security Considerations

- **Self-Signed Certificates:** The above setup uses self-signed certificates, which are suitable for development and testing. Browsers will display a security warning when accessing the server. For production environments, obtain certificates from a trusted Certificate Authority (CA) like [Let's Encrypt](https://letsencrypt.org/).

- **Data Validation:** The current implementation does not include data validation. In a production setting, validate and sanitize incoming data to prevent security vulnerabilities.

- **Error Handling:** Enhanced error handling can be implemented to provide more detailed responses and logging.

---

## Additional CRUD Operations Example

To further expand the server's capabilities, you can add more granular CRUD operations based on file names or manage multiple files within the `data` directory.

### Example: CRUD Operations with Dynamic File Names

```typescript
import * as https from 'https';
import * as fs from 'fs';
import * as path from 'path';
import { IncomingMessage, ServerResponse } from 'http';
import { URL } from 'url';

// Define the path to the data directory
const dataDir = path.join(__dirname, 'data');

// Ensure the data directory exists
if (!fs.existsSync(dataDir)) {
  fs.mkdirSync(dataDir, { recursive: true });
  console.log('Data directory created.');
}

// Define SSL certificates
const options = {
  key: fs.readFileSync(path.join(__dirname, '..', 'server-key.pem')),
  cert: fs.readFileSync(path.join(__dirname, '..', 'server-cert.pem'))
};

// Utility function to parse JSON body
const parseRequestBody = (req: IncomingMessage): Promise<any> => {
  return new Promise((resolve, reject) => {
    let body = '';
    req.on('data', chunk => {
      body += chunk.toString();
    });
    req.on('end', () => {
      try {
        const parsed = JSON.parse(body);
        resolve(parsed);
      } catch (err) {
        reject(err);
      }
    });
  });
};

// Create HTTPS server
const server = https.createServer(options, async (req: IncomingMessage, res: ServerResponse) => {
  const parsedUrl = new URL(req.url || '', `https://${req.headers.host}`);
  const pathname = parsedUrl.pathname;
  const method = req.method;

  // Set CORS headers (optional, for API usage)
  res.setHeader('Access-Control-Allow-Origin', '*');
  res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE, OPTIONS');
  res.setHeader('Access-Control-Allow-Headers', 'Content-Type');

  // Handle preflight requests
  if (method === 'OPTIONS') {
    res.writeHead(204);
    res.end();
    return;
  }

  // Route: /file?name=filename.txt
  const fileName = parsedUrl.searchParams.get('name');
  if (!fileName) {
    res.writeHead(400, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify({ error: 'File name is required as a query parameter (e.g., ?name=filename.txt)' }));
    return;
  }

  const filePath = path.join(dataDir, fileName);

  // Routing based on method
  switch (method) {
    case 'GET':
      // Read File
      fs.readFile(filePath, 'utf-8', (err, data) => {
        if (err) {
          res.writeHead(404, { 'Content-Type': 'application/json' });
          res.end(JSON.stringify({ error: 'File not found.' }));
        } else {
          res.writeHead(200, { 'Content-Type': 'application/json' });
          res.end(JSON.stringify({ content: data }));
        }
      });
      break;

    case 'POST':
      // Create File
      try {
        const body = await parseRequestBody(req);
        fs.writeFile(filePath, body.content, (err) => {
          if (err) {
            res.writeHead(500, { 'Content-Type': 'application/json' });
            res.end(JSON.stringify({ error: 'Failed to write data.' }));
          } else {
            res.writeHead(201, { 'Content-Type': 'application/json' });
            res.end(JSON.stringify({ message: 'Data saved successfully.' }));
          }
        });
      } catch (error) {
        res.writeHead(400, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ error: 'Invalid JSON data.' }));
      }
      break;

    case 'PUT':
      // Update File by Appending
      try {
        const body = await parseRequestBody(req);
        fs.appendFile(filePath, body.content, (err) => {
          if (err) {
            res.writeHead(500, { 'Content-Type': 'application/json' });
            res.end(JSON.stringify({ error: 'Failed to update data.' }));
          } else {
            res.writeHead(200, { 'Content-Type': 'application/json' });
            res.end(JSON.stringify({ message: 'Data updated successfully.' }));
          }
        });
      } catch (error) {
        res.writeHead(400, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ error: 'Invalid JSON data.' }));
      }
      break;

    case 'DELETE':
      // Delete File
      fs.unlink(filePath, (err) => {
        if (err) {
          res.writeHead(404, { 'Content-Type': 'application/json' });
          res.end(JSON.stringify({ error: 'File not found.' }));
        } else {
          res.writeHead(200, { 'Content-Type': 'application/json' });
          res.end(JSON.stringify({ message: 'File deleted successfully.' }));
        }
      });
      break;

    default:
      res.writeHead(405, { 'Content-Type': 'application/json' });
      res.end(JSON.stringify({ error: 'Method not allowed.' }));
      break;
  }
});

// Listen on port 8443 (using a non-privileged port for development)
const PORT = process.env.PORT || 8443;
server.listen(PORT, () => {
  console.log(`HTTPS server is running on https://localhost:${PORT}`);
});
```

#### Explanation:

1. **Dynamic File Handling:**
    - **Query Parameter:** The server expects a `name` query parameter to specify the file name (e.g., `?name=example.txt`).

2. **HTTP Methods:**
    - **GET:** Reads the specified file.
    - **POST:** Creates a new file with the provided content.
    - **PUT:** Appends content to the existing file.
    - **DELETE:** Deletes the specified file.

3. **Data Directory:**
    - Ensures the `./src/data` directory exists upon server startup.

4. **JSON Parsing:**
    - Implements a utility function `parseRequestBody` to parse JSON data from the request body.

5. **CORS Support:**
    - Allows cross-origin requests, which is beneficial for API consumption from different domains.

#### Interacting with the Enhanced Server

1. **Create a New File (`POST /file?name=example.txt`)**

   **Request:**

   ```bash
   curl -k -X POST https://localhost:8443/file?name=example.txt \
     -H "Content-Type: application/json" \
     -d '{"content": "This is the initial content."}'
   ```

   **Response:**

   ```json
   {
     "message": "Data saved successfully."
   }
   ```

2. **Read the File (`GET /file?name=example.txt`)**

   **Request:**

   ```bash
   curl -k https://localhost:8443/file?name=example.txt
   ```

   **Response:**

   ```json
   {
     "content": "This is the initial content."
   }
   ```

3. **Update the File by Appending Data (`PUT /file?name=example.txt`)**

   **Request:**

   ```bash
   curl -k -X PUT https://localhost:8443/file?name=example.txt \
     -H "Content-Type: application/json" \
     -d '{"content": " Appended content."}'
   ```

   **Response:**

   ```json
   {
     "message": "Data updated successfully."
   }
   ```

4. **Read the Updated File (`GET /file?name=example.txt`)**

   **Request:**

   ```bash
   curl -k https://localhost:8443/file?name=example.txt
   ```

   **Response:**

   ```json
   {
     "content": "This is the initial content. Appended content."
   }
   ```

5. **Delete the File (`DELETE /file?name=example.txt`)**

   **Request:**

   ```bash
   curl -k -X DELETE https://localhost:8443/file?name=example.txt
   ```

   **Response:**

   ```json
   {
     "message": "File deleted successfully."
   }
   ```

6. **Error Handling:**

    - **Missing File Name:**

      **Request:**

      ```bash
      curl -k https://localhost:8443/file
      ```

      **Response:**

      ```json
      {
        "error": "File name is required as a query parameter (e.g., ?name=filename.txt)"
      }
      ```

    - **File Not Found:**

      **Request:**

      ```bash
      curl -k https://localhost:8443/file?name=nonexistent.txt
      ```

      **Response:**

      ```json
      {
        "error": "File not found."
      }
      ```

    - **Invalid JSON Data:**

      **Request:**

      ```bash
      curl -k -X POST https://localhost:8443/file?name=example.txt \
        -H "Content-Type: application/json" \
        -d 'Invalid JSON'
      ```

      **Response:**

      ```json
      {
        "error": "Invalid JSON data."
      }
      ```

#### Security Considerations

- **Input Validation:**
    - Validate the `name` query parameter to prevent directory traversal attacks (e.g., using `../` to access unauthorized directories).
    - Implement checks to ensure that only permitted file types are accessed or manipulated.

- **Authentication & Authorization:**
    - Implement authentication mechanisms to restrict access to the file operations.
    - Use authorization to control what actions authenticated users can perform.

- **Error Messages:**
    - Avoid exposing sensitive information in error messages. Provide generic error responses to prevent information leakage.

- **Rate Limiting:**
    - Implement rate limiting to prevent abuse of the API endpoints.

---

## Final Checklist

Before running your enhanced server, ensure the following:

1. **File Existence:**
    - `src/index.ts` exists and contains the updated TypeScript code.
    - `src/data` directory is created automatically when the server starts.

2. **Dependencies Installed:**
    - Run `npm install` to ensure all dependencies are installed.

3. **TypeScript Configuration:**
    - `tsconfig.json` is properly configured as shown above.

4. **SSL Certificates:**
    - Ensure `server-key.pem` and `server-cert.pem` are placed in the project root (`..` relative to `src/index.ts`).

5. **Scripts Set Up:**
    - `package.json` has the necessary scripts (`start`, `build`, `serve`).

6. **Run the Server Correctly:**
    - Use `npm run start` to execute your server with `tsx`.

7. **Permissions:**
    - Using port `8443` avoids the need for administrative rights. Change the port only if necessary.

8. **Environment Variables:**
    - If using environment variables for configuration, ensure they're correctly set before running the server.

---

## Running the Server

1. **Start the Server:**

   ```bash
   npm run start
   ```

2. **Expected Output:**

   ```
   Data directory created.
   HTTPS server is running on https://localhost:8443
   ```

3. **Interact with the Server:**

    - **Accessing `/` to Read an Empty File:**

      ```bash
      curl -k https://localhost:8443/
      ```

      **Response:**

      ```json
      {
        "message": "File is empty or does not exist."
      }
      ```

    - **Creating, Reading, Updating, Deleting, and Renaming Files:**
        - Use the provided endpoints (`/create`, `/read`, `/update`, `/delete`, `/rename`) with appropriate HTTP methods as demonstrated in the examples above.

---

## Troubleshooting Tips

If you encounter issues while running the server, consider the following troubleshooting steps:

### 1. **"Permission Denied" or "EACCES" Error on Port 443**

- **Cause:** Binding to port `443` requires elevated privileges.
- **Solution:**
    - **Use a Higher Port:** Change to a port like `8443` for development.
    - **Run as Administrator:** If you must use port `443`, run your terminal or command prompt with administrative rights.

### 2. **"Module Not Found" Error**

- **Cause:** Node.js (or `tsx`) can't locate the specified module or file.
- **Solution:**
    - **Verify File Paths:** Ensure that all file paths in your code are correct.
    - **Check File Extensions:** Ensure that your TypeScript files have the `.ts` extension.
    - **Run from Correct Directory:** Execute the server from the project root where `package.json` is located.

### 3. **SSL Certificate Errors in Browser**

- **Cause:** Self-signed certificates are not trusted by browsers.
- **Solution:**
    - For development, proceed by accepting the risk in the browser.
    - For production, obtain certificates from a trusted CA like [Let's Encrypt](https://letsencrypt.org/).

### 4. **JSON Parsing Errors**

- **Cause:** Sending invalid JSON data in the request body.
- **Solution:**
    - Ensure that the data sent in `POST` and `PUT` requests is valid JSON.
    - Use tools like Postman to format your requests correctly.

### 5. **File Not Found Errors**

- **Cause:** Attempting to read or manipulate a file that doesn't exist.
- **Solution:**
    - Ensure that the file exists before performing operations.
    - Use appropriate error handling to manage such scenarios gracefully.

### 6. **CORS Issues**

- **Cause:** Cross-Origin Resource Sharing (CORS) restrictions.
- **Solution:**
    - Ensure that the server sets the appropriate CORS headers as shown in the example.
    - Adjust the headers based on your security requirements.

---

## Security Best Practices

While implementing file operations, it's crucial to adhere to security best practices to protect your server and data.

### 1. **Input Validation**

- **Sanitize File Names:**
    - Prevent directory traversal by validating and sanitizing the `name` query parameter.
    - Reject file names containing `../` or other potentially malicious patterns.

- **Restrict File Types:**
    - Allow only specific file types to be created or manipulated.

### 2. **Authentication & Authorization**

- **Implement Authentication:**
    - Use authentication mechanisms (e.g., JWT, OAuth) to restrict access to authorized users.

- **Role-Based Access Control (RBAC):**
    - Define roles and permissions to control what actions authenticated users can perform.

### 3. **Error Handling**

- **Avoid Detailed Error Messages:**
    - Do not expose stack traces or sensitive error details to clients.
    - Log detailed errors on the server side while sending generic messages to clients.

### 4. **Rate Limiting**

- **Prevent Abuse:**
    - Implement rate limiting to restrict the number of requests a client can make within a specific timeframe.

### 5. **Secure Data Storage**

- **Protect Sensitive Data:**
    - Encrypt sensitive data stored in files.
    - Set appropriate file permissions to restrict unauthorized access.

### 6. **Regular Updates**

- **Keep Dependencies Updated:**
    - Regularly update Node.js and all dependencies to patch known vulnerabilities.

- **Monitor Security Advisories:**
    - Stay informed about security advisories related to the tools and libraries you use.

---

## Conclusion

By following the steps outlined above, you can create a robust and secure HTTPS server in Node.js that performs comprehensive CRUD operations on files within the `./src/data` directory. This setup is ideal for development and testing purposes. For production environments, ensure that you implement additional security measures and obtain trusted SSL certificates.

If you encounter any specific issues or have further questions, feel free to ask for more detailed assistance!