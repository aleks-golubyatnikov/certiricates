## Generate certificate:
```
openssl genrsa -out private.key 2048
openssl req -new -sha256 -days 365 -key private.key -out request.csr
openssl x509 -in request.csr -out gitlab.internal.com.crt -req -signkey private.key -days 365
```
## Copy certificate:
```
cp gitlab.internal.com.crt /etc/gitlab/trusted-certs
gitlab-ctl reconfigure
```
