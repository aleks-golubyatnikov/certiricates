# Generate certificates.
## Private key:
```
openssl genrsa -out private.key 2048
```
## Certificate request:
```
openssl req -new -days 365 -key private.key -out request.csr
```
## Certificate:
```
openssl x509 -in request.csr -out certificate.crt -req -signkey private.key -days 365
```
