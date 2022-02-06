# Generate certificates:
## Private key:
```
openssl genrsa -out private.key 2048
```
## Certificate request:
```
openssl req -new -sha256 -days 365 -key private.key -out request.csr
```
## Certificate:
```
openssl x509 -in request.csr -out gitlab.internal.com.crt -req -signkey private.key -days 365
```

# Install certificates:
## Copy certificate
```
cp gitlab.internal.com.crt /etc/gitlab/trusted-certs
```
## Reconfigure Gitlab
```
gitlab-ctl reconfigure
```
