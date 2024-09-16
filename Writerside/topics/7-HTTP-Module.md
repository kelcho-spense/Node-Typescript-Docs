# 7. HTTP Module

The HTTP module in Node.js provides the functionality to create web servers and handle HTTP requests and responses. Additionally, Node.js supports HTTP/2 and HTTPS for improved performance and secure communication. Each protocol offers unique features and is widely used to handle web traffic.

#### HTTP

The HTTP (Hypertext Transfer Protocol) module in Node.js is used to create and manage basic web servers. It provides functionality to create a server, listen for incoming requests, and respond with data. This is one of the core modules in Node.js, allowing developers to set up a web server with minimal setup.

##### Key Features:
- Handling basic HTTP requests and responses (GET, POST, PUT, DELETE, etc.)
- Managing request headers and response data
- Easy access to URL and query parameters

##### Example: Basic HTTP Server in Node.js with TypeScript

```javascript
import * as http from 'http';

// Create a basic HTTP server
const server = http.createServer((req, res) => {
  // Set the response header and status code
  res.writeHead(200, { 'Content-Type': 'text/plain' });

  // Send a response based on the URL
  if (req.url === '/') {
    res.end('Welcome to the homepage!');
  } else if (req.url === '/about') {
    res.end('About us page');
  } else {
    res.end('404 - Page not found');
  }
});

// Listen on port 3000
server.listen(3000, () => {
  console.log('Server is running on http://localhost:3000');
});
```

##### Explanation:
- **`http.createServer()`**: Creates an HTTP server that listens for incoming requests.
- **`req.url`**: Captures the URL of the incoming request, which allows routing to different responses.
- **`res.writeHead()`**: Sets the HTTP status code and headers before sending the response.
- **`res.end()`**: Ends the response and sends it to the client.

##### Use Cases:
- Lightweight web servers for testing or small applications.
- Handling basic APIs and static file serving.

---

#### HTTP/2

HTTP/2 is a major revision of the HTTP protocol that introduces several improvements over HTTP/1.1, including better performance and reduced latency. Node.js provides native support for HTTP/2 via the `http2` module. Key benefits include multiplexing (allowing multiple requests and responses to be sent over a single connection), header compression, and server push.

##### Key Features:
- **Multiplexing**: Multiple requests and responses over a single TCP connection.
- **Header Compression**: Compresses HTTP headers to reduce overhead.
- **Server Push**: The server can push resources (like CSS, JS files) to the client before the client even requests them.

##### Example: Basic HTTP/2 Server in Node.js with TypeScript

```javascript
import * as http2 from 'http2';
import * as fs from 'fs';

// Load SSL certificates for secure connection
// learn how to generate server-key.pem & server-cert.pem from 7.1 Install OpenSSL on Windows
const options = {
  key: fs.readFileSync('server-key.pem'),
  cert: fs.readFileSync('server-cert.pem')
};

// Create an HTTP/2 server
const server = http2.createSecureServer(options);

server.on('stream', (stream, headers) => {
  // Respond to HTTP/2 requests
  const path = headers[':path'];
  
  if (path === '/') {
    stream.respond({
      'content-type': 'text/html',
      ':status': 200
    });
    stream.end('<h1>Welcome to the HTTP/2 Server</h1>');
  } else if (path === '/about') {
    stream.respond({
      'content-type': 'text/html',
      ':status': 200
    });
    stream.end('<h1>About Us</h1>');
  } else {
    stream.respond({
      ':status': 404
    });
    stream.end('404 - Not Found');
  }
});

// Listen on port 8443
server.listen(8443, () => {
  console.log('HTTP/2 server is running on https://localhost:8443');
});
```

##### Explanation:
- **`http2.createSecureServer()`**: Creates a secure HTTP/2 server using SSL certificates.
- **`stream.respond()`**: Sends headers and status code for each incoming request.
- **`stream.end()`**: Ends the response and sends data to the client.

##### Use Cases:
- Modern web applications that require faster loading times and efficient resource loading.
- Applications that need to support Server Push for improved user experience.

---

#### HTTPS

HTTPS (Hypertext Transfer Protocol Secure) is an extension of HTTP that provides secure communication over the network using SSL/TLS encryption. In Node.js, the `https` module is used to create secure servers. HTTPS ensures that all data exchanged between the server and the client is encrypted, protecting sensitive information from being intercepted.

##### Key Features:
- **SSL/TLS Encryption**: Secure communication using certificates.
- **Secure Data Transmission**: Protects sensitive information (like login credentials, payment info).

##### Example: Basic HTTPS Server in Node.js with TypeScript

```javascript
import * as https from 'https';
import * as fs from 'fs';

// Load SSL certificates
const options = {
  key: fs.readFileSync('server-key.pem'),
  cert: fs.readFileSync('server-cert.pem')
};

// Create a basic HTTPS server
const server = https.createServer(options, (req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });

  // Respond to requests based on URL
  if (req.url === '/') {
    res.end('Welcome to the secure homepage!');
  } else if (req.url === '/about') {
    res.end('About us page (secure)');
  } else {
    res.end('404 - Secure page not found');
  }
});

// Listen on port 443 (default for HTTPS)
server.listen(443, () => {
  console.log('HTTPS server is running on https://localhost');
});
```

##### Explanation:
- **SSL Certificates**: The `options` object includes the private key and certificate required for secure connections.
- **`https.createServer()`**: Creates an HTTPS server using SSL/TLS encryption.
- **Port 443**: The standard port for HTTPS, ensuring that all communication is encrypted.

##### Use Cases:
- Websites or APIs that need to handle sensitive data securely, such as login systems, financial transactions, or private communications.
- Any application that wants to ensure security and privacy by default.

---

### Summary

- **HTTP**: Ideal for building simple web servers or APIs where performance and security are not primary concerns. However, it is commonly used for local development and testing.
- **HTTP/2**: Offers significant performance improvements over HTTP/1.1, including multiplexing and server push, which makes it ideal for modern web applications.
- **HTTPS**: Provides a secure communication layer with SSL/TLS encryption, essential for any application handling sensitive user data. It ensures the integrity and privacy of data exchanged between client and server.

These modules enable developers to create flexible, high-performance, and secure applications for various use cases.