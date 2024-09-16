# 7.1 Install OpenSSL on Windows

You can generate the `server-key.pem` and `server-cert.pem` files on Windows using OpenSSL. Here's a step-by-step guide to generate the self-signed certificates on Windows.


Configuring OpenSSL on Windows involves downloading and installing the appropriate binaries, setting up environment variables, and verifying the installation. Below is a step-by-step guide to help you through the process.

## 1. **Understanding OpenSSL**

**OpenSSL** is an open-source toolkit for the Transport Layer Security (TLS) and Secure Sockets Layer (SSL) protocols. It is widely used for securing communications over computer networks and for managing cryptographic keys and certificates.

## 2. **Downloading OpenSSL**

OpenSSL doesn't provide official Windows binaries, but trusted third-party providers offer precompiled versions:

### **Shining Light Productions**

1. **Visit the Website:**
    - Go to [Shining Light Productions OpenSSL Binaries](https://slproweb.com/products/Win32OpenSSL.html).

2. **Choose the Right Installer:**
    - **Version:** Select the latest stable version compatible with your needs.
    - **Architecture:** Choose between Win32 (32-bit) or Win64 (64-bit) based on your system.
    - **Light vs. Full:** The "Light" version includes the essential components, while the "Full" version contains additional tools.

3. **Download the Installer:**
    - Click on the appropriate `.exe` installer to download.

## 3. **Installing OpenSSL**

### **Using Shining Light Productions Installer**

1. **Run the Installer:**
    - Double-click the downloaded `.exe` file.

2. **Follow the Installation Wizard:**
    - **License Agreement:** Accept the terms and proceed.
    - **Destination Directory:** Choose the installation path (default is usually fine).
    - **Select Components:**
        - Ensure that the "OpenSSL binaries" and "OpenSSL libraries" are selected.
        - Optionally, include the "Copy OpenSSL DLLs to the Windows system directory" if desired.
    - **Install:** Click `Install` to begin the process.

3. **Complete Installation:**
    - Finish the wizard once installation is complete.

## 5. **Configuring Environment Variables**

To use OpenSSL from the Command Prompt, you need to add its `bin` directory to the system `PATH`.

1. **Locate OpenSSL Installation Directory:**
    - Typically, it's installed in `C:\OpenSSL-Win64` or `C:\OpenSSL-Win32`.

2. **Add to PATH:**
    - **Open System Properties:**
        - Press `Win + X`, select `System`, then `Advanced system settings`.
    - **Environment Variables:**
        - Click on `Environment Variables`.
    - **Edit PATH:**
        - Under `System variables`, find and select `Path`, then click `Edit`.
    - **Add New Entry:**
        - Click `New` and enter the path to the OpenSSL `bin` directory, e.g., `C:\OpenSSL-Win64\bin`.
    - **Save Changes:**
        - Click `OK` on all dialogs to apply the changes.

3. **Verify PATH Update:**
    - Open a new Command Prompt window and run:
      ```shell
      echo %PATH%
      ```
    - Ensure the OpenSSL `bin` path is listed.

## 6. **Verifying the Installation**

1. **Open Command Prompt:**
    - Press `Win + R`, type `cmd`, and press `Enter`.

2. **Check OpenSSL Version:**
    - Run the command:
      ```shell
      openssl version
      ```
    - You should see output similar to:
      ```
      OpenSSL 1.1.1k  25 Mar 2021
      ```

3. **Basic Test:**
    - Generate a test key:
      ```shell
      openssl genrsa -out test.key 2048
      ```
    - If the command executes without errors and `test.key` is created, OpenSSL is functioning correctly.

## 7. **Common Configuration Tasks**

### **Generating a Private Key and Certificate Signing Request (CSR)**

### Step 1. Generate Private Key:
   ```shell
   openssl genrsa -out myprivate.key 2048
   ```

### Step 2. Generate CSR:
   ```shell
   openssl req -new -key myprivate.key -out myrequest.csr
   ```
    - Follow the prompts to enter information like Country, State, Organization, etc.


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

These files can be used in your Node.js HTTPS and HTTP/2 applications. 

### Final Notes:
- You can use these certificates for local development. When using them in a browser, it will show a security warning because the certificates are self-signed and not trusted by default.
- In production, you'll need to obtain SSL certificates from a trusted Certificate Authority (CA), such as Let's Encrypt or a paid provider like DigiCert or GlobalSign.
