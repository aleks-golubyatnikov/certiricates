## Utils:
```
sudo apt install gnutls-bin
openssl version –a
```
## Generate certificate:
```
#openssl
#ca
openssl genrsa -out ca.key 2048
openssl req -new -key ca.key -subj "/CN=linux02" -config ca.cnf -out ca.csr
openssl x509 -req -days 365 -in ca.csr -signkey ca.key -extensions req_ext -extfile ca.cnf -out ca.crt

#server
openssl genrsa -out gitlab.internal.com.key 2048
openssl req -new -key gitlab.internal.com.key -subj "/CN=gitlab.internal.com" -config server.cnf -out gitlab.internal.com.csr
openssl x509 -req -days 365 -in gitlab.internal.com.csr -CA ca.crt -CAkey ca.key -CAcreateserial -extensions req_ext -extfile server.cnf -out gitlab.internal.com.crt

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
