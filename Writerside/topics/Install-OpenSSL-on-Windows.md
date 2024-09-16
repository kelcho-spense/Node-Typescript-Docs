# 7.1 Install OpenSSL on Windows

You can generate the `server-key.pem` and `server-cert.pem` files on Windows using OpenSSL. Here's a step-by-step guide to generate the self-signed certificates on Windows.

### Step 1: Install OpenSSL on Windows

If you don't have OpenSSL installed on Windows, follow these steps:

1. Download the OpenSSL Windows installer from [this link](https://slproweb.com/products/Win32OpenSSL.html).
2. Install OpenSSL, and during the installation, ensure that you add OpenSSL to the system PATH.

After installation, you should be able to use `openssl` commands from the Command Prompt or PowerShell.

### Step 2: Open Command Prompt or PowerShell

Once OpenSSL is installed, open a **Command Prompt** or **PowerShell** with administrative privileges.

### Step 3: Generate the Private Key and Self-Signed Certificate

#### 1. Generate the Private Key (`server-key.pem`)

Run the following command to generate a 2048-bit RSA private key:

```bash
openssl genrsa -out server-key.pem 2048
```

This command will create the `server-key.pem` file, which contains the private key.

#### 2. Generate the Certificate Signing Request (CSR)

Next, generate a Certificate Signing Request (CSR) using the private key:

```bash
openssl req -new -key server-key.pem -out server-csr.pem
```

When prompted, enter the required information. Here's an example:

```
Country Name (2 letter code) [AU]:US
State or Province Name (full name) [Some-State]:California
Locality Name (eg, city) []:San Francisco
Organization Name (eg, company) [Internet Widgits Pty Ltd]:MyCompany
Organizational Unit Name (eg, section) []:Development
Common Name (e.g. server FQDN or YOUR name) []:localhost
Email Address []:admin@mycompany.com
```

For the **Common Name**, use `localhost` if you're generating the certificate for local development.

#### 3. Generate the Self-Signed Certificate (`server-cert.pem`)

Finally, generate a self-signed certificate using the CSR and the private key:

```bash
openssl x509 -req -in server-csr.pem -signkey server-key.pem -out server-cert.pem -days 365
```

This will create the `server-cert.pem` file, which is valid for 365 days.

### Step 4: Use the Certificates in Your Node.js Application

You should now have the following files in your working directory:
- `server-key.pem`: The private key.
- `server-cert.pem`: The self-signed certificate.

These files can be used in your Node.js HTTPS and HTTP/2 applications. Here's how to use them:

```javascript
import * as https from 'https';
import * as http2 from 'http2';
import * as fs from 'fs';

// Load the certificate and key files
const options = {
  key: fs.readFileSync('server-key.pem'),
  cert: fs.readFileSync('server-cert.pem'),
};

// HTTPS server example
const httpsServer = https.createServer(options, (req, res) => {
  res.writeHead(200);
  res.end('Hello, secure HTTPS world!');
});
httpsServer.listen(443, () => {
  console.log('HTTPS server running at https://localhost');
});

// HTTP/2 server example
const http2Server = http2.createSecureServer(options);
http2Server.on('stream', (stream, headers) => {
  stream.respond({ ':status': 200 });
  stream.end('Hello, HTTP/2 world!');
});
http2Server.listen(8443, () => {
  console.log('HTTP/2 server running at https://localhost:8443');
});
```

### Final Notes:
- You can use these certificates for local development. When using them in a browser, it will show a security warning because the certificates are self-signed and not trusted by default.
- In production, you'll need to obtain SSL certificates from a trusted Certificate Authority (CA), such as Let's Encrypt or a paid provider like DigiCert or GlobalSign.
