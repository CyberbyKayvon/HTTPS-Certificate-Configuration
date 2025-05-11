# HTTPS-Certificate-Configuration
This project demonstrates how to create and deploy HTTPS certificates using a custom-built Certificate Authority (CA) and Apache2 web server on Ubuntu Linux. The project ensures a secure, signed connection is established and verified in both Linux and Windows browsers.

# HTTPS Certificate Setup with Local Certificate Authority (CA)

## 📌 Overview
This project demonstrates how to create and deploy HTTPS certificates using a custom-built Certificate Authority (CA) and Apache2 web server on Ubuntu Linux. The project ensures a secure, signed connection is established and verified in both Linux and Windows browsers.

## 🔧 Tools & Environment
- Ubuntu 22.04 (Virtual Machine)
- Apache2 Web Server
- OpenSSL (for certificate creation)
- Firefox (Linux & Windows)
- Windows 10 (client test machine)

## 🔐 Steps Implemented
1. Created a self-signed root CA using OpenSSL.
2. Generated a server private key and certificate signing request (CSR).
3. Signed the CSR with the custom CA.
4. Installed the signed certificate on an Apache2 HTTPS virtual host.
5. Configured Firefox (Ubuntu & Windows) to trust the local CA.
6. Verified HTTPS connection showed as secure.
7. (Extra Credit) Adjusted SAN and network settings for Windows browser validation.

## 📸 Screenshots
- Initial self-signed cert warning in Firefox
- Successful certificate chain validation (Linux and Windows)
- OpenSSL certificate output
- CA import steps

## ✍️ Notable Configuration Files
- `san.cnf`: Specifies Subject Alternative Name for certificate (includes LAN IP).
- `default-ssl.conf`: Apache site configuration to enable HTTPS.
- `myCA.pem`: Root certificate file used for signing.

## 🧠 Learning Outcomes
- Hands-on experience with TLS certificate generation and management.
- Understanding of trusted root certificates and browser validation.
- Gained insight into browser security warnings and proper CA chains.

## 🏆 Extra Credit
- Configured VM networking (Bridged mode) to test from another OS.
- Successfully trusted and validated custom CA in Windows Firefox.

## 📁 References
- [OpenSSL Documentation](https://www.openssl.org/docs/)
- [Apache SSL Config Guide](https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html)

Potential Security Risk Ahead: HTTPS Certificate Authority Project

mkdir myCA  
cd myCA  
openssl genrsa -out myCA.key 2048  
openssl req -x509 -new -nodes -key myCA.key -sha256 -days 3650 -out myCA.pem  

openssl genrsa -out server.key 2048  
openssl req -new -key server.key -out server.csr -config san.cnf  

openssl x509 -req -in server.csr -CA myCA.pem -CAkey myCA.key -CAcreateserial -out server.crt -days 365 -sha256 -extfile san.cnf -extensions req_ext  

sudo a2enmod ssl  
sudo a2ensite default-ssl  
sudo systemctl reload apache2  
