# 8. File System Module

Node.js provides a built-in module called `fs` (File System), which allows you to interact with the file system on your server. With this module, you can perform various file operations such as reading, writing, updating, deleting, and renaming files. You can also use Node.js as a file server to handle file requests from clients.

#### Node.js as a File Server

Using Node.js as a file server, you can read and serve files to users, or allow file manipulations like creating, updating, deleting, or renaming files. The `fs` module provides both asynchronous and synchronous methods for these operations. It's important to use asynchronous methods whenever possible to prevent blocking the event loop.

Below are some common operations you can perform with the `fs` module:

#### i. Read Files

To read the contents of a file, you can use the `fs.readFile()` method asynchronously. This method reads the entire file into memory.

##### Example: Reading a File Asynchronously

```javascript
import * as fs from 'fs';
import * as http from 'http';

// Example: Serving a file using Node.js
const server = http.createServer((req, res) => {
  // Reading the requested file asynchronously
  fs.readFile('example.txt', 'utf-8', (err, data) => {
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

##### Explanation:
- **`fs.readFile()`**: Reads the contents of the `example.txt` file asynchronously.
- **Error Handling**: If the file does not exist, an error message is sent back to the client.

##### Use Case:
- Serve files like HTML, CSS, JavaScript, or other resources to clients in a web server.

#### ii. Create Files

You can create new files in Node.js using `fs.writeFile()` or `fs.appendFile()`. The `writeFile()` method creates a new file and writes content to it, overwriting the file if it already exists, while `appendFile()` adds data to the end of an existing file or creates the file if it doesn’t exist.

##### Example: Creating a New File

```javascript
import * as fs from 'fs';

// Example: Creating and writing to a file
fs.writeFile('newfile.txt', 'Hello, this is a newly created file!', (err) => {
  if (err) throw err;
  console.log('File created and written successfully');
});
```

##### Example: Appending Data to a File

```javascript
import * as fs from 'fs';

// Example: Appending data to an existing file
fs.appendFile('newfile.txt', '\nAppending more content to the file.', (err) => {
  if (err) throw err;
  console.log('Content appended successfully');
});
```

##### Use Case:
- Creating new files for logging, storing data, or dynamically generating files.

#### iii. Update Files

You can update a file by either writing to it again (overwriting the content) using `fs.writeFile()` or appending new content to the existing file using `fs.appendFile()`.

##### Example: Overwriting a File

```javascript
import * as fs from 'fs';

// Example: Overwriting the content of a file
fs.writeFile('newfile.txt', 'This content replaces the old one.', (err) => {
  if (err) throw err;
  console.log('File content overwritten successfully');
});
```

##### Example: Appending to a File

```javascript
import * as fs from 'fs';

// Example: Appending content to a file
fs.appendFile('newfile.txt', '\nMore content being added.', (err) => {
  if (err) throw err;
  console.log('File updated with new content');
});
```

##### Use Case:
- Updating logs, modifying data files, or appending new data to reports.

#### iv. Delete Files

You can delete files using `fs.unlink()` method.

##### Example: Deleting a File

```javascript
import * as fs from 'fs';

// Example: Deleting a file
fs.unlink('newfile.txt', (err) => {
  if (err) throw err;
  console.log('File deleted successfully');
});
```

##### Use Case:
- Removing temporary files or cleaning up old data files that are no longer needed.

#### v. Rename Files

To rename a file, use the `fs.rename()` method.

##### Example: Renaming a File

```javascript
import * as fs from 'fs';

// Example: Renaming a file
fs.rename('oldfile.txt', 'newfile.txt', (err) => {
  if (err) throw err;
  console.log('File renamed successfully');
});
```

##### Use Case:
- Organizing files, managing file versions, or moving files within the same directory.

---

### Complete Example: Node.js File Server with File Operations

Below is an example of how you can create a basic file server in Node.js that handles reading, creating, updating, and deleting files dynamically.

```javascript
import * as fs from 'fs';
import * as http from 'http';

// Example: Node.js File Server
const server = http.createServer((req, res) => {
  const url = req.url;
  
  // Read file
  if (url === '/read') {
    fs.readFile('example.txt', 'utf-8', (err, data) => {
      if (err) {
        res.writeHead(404, { 'Content-Type': 'text/plain' });
        res.end('File not found');
      } else {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end(data);
      }
    });

  // Create file
  } else if (url === '/create') {
    fs.writeFile('example.txt', 'This is a newly created file!', (err) => {
      if (err) {
        res.writeHead(500, { 'Content-Type': 'text/plain' });
        res.end('Error creating file');
      } else {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('File created successfully');
      }
    });

  // Update file
  } else if (url === '/update') {
    fs.appendFile('example.txt', '\nAppending new content to the file.', (err) => {
      if (err) {
        res.writeHead(500, { 'Content-Type': 'text/plain' });
        res.end('Error updating file');
      } else {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('File updated successfully');
      }
    });

  // Delete file
  } else if (url === '/delete') {
    fs.unlink('example.txt', (err) => {
      if (err) {
        res.writeHead(404, { 'Content-Type': 'text/plain' });
        res.end('File not found');
      } else {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('File deleted successfully');
      }
    });

  // Rename file
  } else if (url === '/rename') {
    fs.rename('example.txt', 'renamedfile.txt', (err) => {
      if (err) {
        res.writeHead(500, { 'Content-Type': 'text/plain' });
        res.end('Error renaming file');
      } else {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('File renamed successfully');
      }
    });
  
  // Default route
  } else {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Use /read, /create, /update, /delete, or /rename to interact with the file system.');
  }
});

// Start the server
server.listen(3000, () => {
  console.log('File server is running at http://localhost:3000');
});
```

##### Explanation:
- **Routes**: Each URL (`/read`, `/create`, `/update`, `/delete`, `/rename`) corresponds to different file operations.
- **Error Handling**: Each operation checks for errors and responds with the appropriate message.
