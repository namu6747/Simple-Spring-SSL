# Simple-Spring-SSL

## Server
`
openssl genrsa -out key.pem 2048

openssl req -new -key key.pem -out csr.csr

openssl x509 -req -days 365 -in csr.csr -signkey key.pem -out cert.pem

openssl pkcs12 -export -in cert.pem -inkey key.pem -out keystore.p12 -name youralias -CAfile cert.pem -caname root

server:
  ssl:
    key-alias: youralias
    key-store: classpath:keystore.p12
    key-store-password: 1234

`

## Client
`
keytool -import -file cert.pem -alias servercert -keystore cli.jks -storepass 123456

server.ssl.trust-store=classpath:cli.jks
server.ssl.trust-store-password=123456

`
