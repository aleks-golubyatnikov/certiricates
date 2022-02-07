## Generate certificates:
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

openssl x509 -text -noout -in gitlab.internal.com.crt | grep -A 1 "Subject Alternative Name"  
openssl x509 -text -noout -in gitlab.internal.com.crt | grep -A 1 "Subject Alternative Name"  

#remove password
openssl rsa -in gitlab.internal.com.key -out gitlab.internal.com.key

#copy cert files
cp gitlab.internal.com.crt gitlab.internal.com.key /etc/gitlab/ssl

#docker trust
mkdir /etc/docker/certs.d/gitlab.internal.com:5050/
cp gitlab.internal.com.crt /etc/docker/certs.d/gitlab.internal.com:5050/ca.crt
```
