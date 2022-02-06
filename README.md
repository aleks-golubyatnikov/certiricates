## Install utils:
```
# certtool
sudo apt install gnutls-bin
```

## Verify openssl version:
```
openssl version –a
```

## Generate certificate:
```
#openssl
openssl genrsa -out private.key 2048
openssl req -new -sha256 -days 365 -key private.key -out request.csr
openssl x509 -in request.csr -out gitlab.internal.com.crt -req -signkey private.key -days 365

#certtool
certtool --generate-privkey --outfile ca-key.pem
certtool --generate-self-signed --load-privkey ca-key.pem --outfile ca.pem

certtool --generate-privkey --outfile gitlab.internal.com-key.pem --bits 2048
certtool --generate-request --load-privkey gitlab.internal.com-key.pem --outfile request.pem
certtool --generate-certificate --load-request request.pem --outfile gitlab.internal.com.pem --load-ca-certificate ca.pem --load-ca-privkey ca-key.pem
certtool -i --infile gitlab.internal.com.pem

openssl x509 -in gitlab.internal.com.pem -inform PEM -out gitlab.internal.com.crt

```
## Copy certificate:
```
cp gitlab.internal.com.crt /etc/gitlab/trusted-certs

gitlab-ctl reconfigure
```
