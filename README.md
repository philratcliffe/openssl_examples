
This repo contains some example OpenSSL config files and some OpenSSL command line examples.

# OpenSSL command line examples
### Generate a self signed certificate and key
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes

### Run an https server
sudo openssl s_server -key key.pem -cert cert.pem -accept 443 -www
