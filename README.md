
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


