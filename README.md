________________________________________
📦 Tổng Quan Các File Sẽ Tạo:
File/Folder            Vai trò
client.key             Private key cho client
client.csr             Certificate Signing Request của client
client.crt             client cert đã được ký bởi CA
client.keystore.p12    Keystore dùng cho client 
client.truststore.p12  Truststore để client tin CA
________________________________________

✅ BƯỚC 1: Tạo Client key và CSR
openssl genrsa -out client.key 2048
openssl req -new -key client.key -out client.csr -subj "/CN=Client"

________________________________________

✅ BƯỚC 2: CA của napas ký CSR của Client và trả về file client.crt

________________________________________

✅ BƯỚC 3: Tạo keystore và truststore

1. Client keystore 
openssl pkcs12 -export \
  -in client.crt -inkey client.key \
  -out client.keystore.p12 -name client.keystore \
  -password pass:billing

2. Client truststore (import cert CA)
- Bên napas tạo

________________________________________

✅ BƯỚC 6: Cấu hình Java springboot

# enable/disable https
server.ssl.enabled=true
# keystore format
server.ssl.key-store-type=PKCS12
# keystore location
server.ssl.key-store=classpath:keystore/keystore.p12
# keystore password
server.ssl.key-store-password=changeit 
# SSL protocol to use 
server.ssl.protocol=TLS
# Enabled SSL protocols 
server.ssl.enabled-protocols=TLSv1.2 
server.ssl.client-auth=need
# trust store location
server.ssl.trust-store=classpath:keystore/truststore.p12
# trust store password
server.ssl.trust-store-password=changeit

________________________________________
