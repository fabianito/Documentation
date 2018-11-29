Generate CA key & certificate - fill out the information when asked for such as country & organization name.
```
openssl genrsa -out MyRootCA.key 2048
openssl req -x509 -new -nodes -key MyRootCA.key -sha256 -days 1024 -out MyRootCA.pem
```

Generate client key & certificate signing request - fill out info
IMPORTANT: CN / Common Name should be the clients IP or FQDN
```
openssl genrsa -out MyClient1.key 2048
openssl req -new -key MyClient1.key -out MyClient1.csr
```

Generate client certificate based on our own CA certificate
```
openssl x509 -req -in MyClient1.csr -CA MyRootCA.pem -CAkey MyRootCA.key -CAcreateserial -out MyClient1.pem -days 1024 -sha256
```
Konviertieren eines pem zu einem pk12:
```
openssl pkcs12 -export -in Beispiel.crt -inkey Beispiel.key -out Zertname.p12
```
