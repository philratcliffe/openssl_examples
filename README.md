
# Example OpenSSL commands and config files.

## OpenSSL command line examples
### Generate a self signed certificate and key
```bash
$ openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
```

### Run an https server
```bash
$ sudo openssl s_server -key key.pem -cert cert.pem -accept 443 -www
```
### Generate a self signed certificate and key for domain1
```bash
$ openssl req -config domain1.config -new -x509 -sha256 -newkey rsa:2048 -nodes -keyout domain1.key.pem -days 365 -out domain1.cert.pem
```

### Generate a self signed certificate and key for domain2
```bash
$ openssl req -config domain2.config -new -x509 -sha256 -newkey rsa:2048 -nodes -keyout domain2.key.pem -days 365 -out domain2.cert.pem
```

### Run an https server that supports SNI
When a client connects without indicating a hostname, the domain1 cert is
returned, otherwise it returns the cert requested (either domain1.com or
domain2.com).
```bash
$ sudo openssl s_server -accept 443 -www -servername www.domain1.com -cert domain1.cert.pem -key domain1.key.pem -servername www.domain2.com -cert2
domain2.cert.pem -key2 domain2.key.pem
```

