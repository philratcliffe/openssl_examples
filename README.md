# OpenSSL Examples

This document provides examples of common OpenSSL command-line operations.

## Command Line Examples

### Generate a self-signed certificate and key

Creates a new 4096-bit RSA private key (`key.pem`) and a corresponding self-signed certificate (`cert.pem`) valid for 365 days. The `-nodes` option prevents encrypting the private key (no passphrase).

```bash
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
```

### Run a basic HTTPS server

Starts a simple HTTPS test server on port 443 using the key and certificate generated above. The `-www` option provides basic HTTP-like responses (connection info, certificate details). Requires root/administrator privileges to bind to port 443.

```bash
sudo openssl s_server -key key.pem -cert cert.pem -accept 443 -www
```

### Run an insecure SSLv2 server for testing purposes

**Warning:** SSLv2 is **NOT secure** and has known vulnerabilities! It should **only** be used for specific testing purposes, such as verifying if a client correctly identifies servers still supporting this insecure protocol.

**NOTE:** SSLv2 support is typically **disabled by default** in modern OpenSSL builds. To run the command below, you might need to obtain the OpenSSL source code and compile it manually with the `enable-ssl2` configuration option.

```bash
sudo openssl s_server -ssl2 -key key.pem -cert cert.pem -accept 443 -www
```

### Generate a self-signed certificate and key for domain1

Generates a 2048-bit RSA key (`domain1.key.pem`) and a self-signed certificate (`domain1.cert.pem`) using settings from `domain1.config`. The certificate uses SHA256 for the signature and is valid for 365 days. `-nodes` ensures the key is not encrypted.

*Requires a configuration file named `domain1.config` in the same directory.*

```bash
openssl req -config domain1.config -new -x509 -sha256 -newkey rsa:2048 -nodes -keyout domain1.key.pem -days 365 -out domain1.cert.pem
```

### Generate a self-signed certificate and key for domain2

Similar to the above, but for `domain2`, using `domain2.config`.

*Requires a configuration file named `domain2.config` in the same directory.*

```bash
openssl req -config domain2.config -new -x509 -sha256 -newkey rsa:2048 -nodes -keyout domain2.key.pem -days 365 -out domain2.cert.pem
```

### Run an HTTPS server that supports SNI

Starts an HTTPS test server on port 443 that can serve different certificates based on the hostname requested by the client (Server Name Indication - SNI).

* If a client connects **without** specifying a hostname via SNI, the default certificate (`domain1.cert.pem` / `domain1.key.pem`) is returned.
* If a client requests `www.domain1.com`, the `domain1` certificate/key are used.
* If a client requests `www.domain2.com`, the `domain2` certificate/key (specified with `-cert2`/`-key2`) are used.

For local testing, you might need to modify your system's hosts file (e.g., `/etc/hosts` on Linux/macOS, `C:\Windows\System32\drivers\etc\hosts` on Windows). Replace `192.168.1.155` with the actual IP address of the machine running the server:

```
# Example hosts file entry
192.168.1.155   [www.domain1.com](https://www.domain1.com) [www.domain2.com](https://www.domain2.com)
```

Run the SNI-enabled server (requires root/administrator privileges for port 443):

```bash
sudo openssl s_server -accept 443 -www \
    -servername [www.domain1.com](https://www.domain1.com) -cert domain1.cert.pem -key domain1.key.pem \
    -servername [www.domain2.com](https://www.domain2.com) -cert2 domain2.cert.pem -key2 domain2.key.pem
```
```
