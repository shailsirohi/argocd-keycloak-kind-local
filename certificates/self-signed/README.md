# Source website

https://www.section.io/engineering-education/how-to-get-ssl-https-for-localhost/

# Steps

1. Generate private key

mkdir CA
cd CA
openssl genrsa -out CA.key -des3 2048

2. Generate root CA certificate
openssl req -x509 -sha256 -new -nodes -days 3650 -key CA.key -out CA.pem
for FQDN. I use: 127-0-0-1.sslip.io. Howeverm, as per documentation not necessary

3. Generate localhost certificates
mkdir localhost
cd localhost
nano localhost.ext

``````````````````````````````````````
localhost.ext contents:
authorityKeyIdentifier = keyid,issuer
basicConstraints = CA:FALSE
keyUsage = keyEncipherment, digitalSignature, nonRepudiation, dataEncipherment
subjectAltName = @alt_names
[alt_names]
DNS.1   = argo.127-0-0-1.sslip.io
DNS.2   = 127-0-0-1.sslip.io
DNS.3   = keycloak.127-0-0-1.sslip.io
````````````````````````````````````````````````

4. Generate localhost private key

openssl genrsa -out localhost.key -des3 2048

5. Generate localhost CSR
openssl req -new -key localhost.key -out localhost.csr

6. Generae localhost cert
openssl x509 -req -in localhost.csr -CA ../CA.pem -CAkey ../CA.key -CAcreateserial -days 3650 -sha256 -extfile localhost.ext -out localhost.crt

7. decrypt localhost key
openssl rsa -in localhost.key -out localhost.decrypted.key
