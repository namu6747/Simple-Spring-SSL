# Simple-Spring-SSL

## Server
```
# Create a Private Key
openssl genrsa -out key.pem 2048

# Create a Certificate Signing Request
openssl req -new -key key.pem -out csr.csr

# Create a Self-Signed Certificate
openssl x509 -req -days 365 -in csr.csr -signkey key.pem -out cert.pem

# Create a Server KeyStore
openssl pkcs12 -export -in cert.pem -inkey key.pem -out keystore.p12 -name youralias -CAfile cert.pem -caname root

server:
  ssl:
    key-alias: youralias
    key-store: classpath:keystore.p12
    key-store-password: 1234
```

## Client
```
# Create a Client KeyStore
keytool -import -file cert.pem -alias servercert -keystore cli.jks -storepass 123456

server.ssl.trust-store=classpath:cli.jks
server.ssl.trust-store-password=123456
```

- 서버 애플리케이션 실행 후 클라이언트 애플리케이션의 SslApplicationTests.contextLoads() 실행
- 클라이언트 애플리케이션에서 SSLContext 초기화된 Http Request 전용 빈 등록해보기
- 다중 서버로 KeyStore N : M 구조 실행해보기
