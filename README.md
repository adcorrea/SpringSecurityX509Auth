# springSecurityX589Auth
Projeto funcional com autenticação usando certificado X589 do Spring Security.

Pré requisitos

JDK 11

OPENSSL -> https://slproweb.com/products/Win32OpenSSL.html

instalar usando o roteiro no link https://forum.casadodesenvolvedor.com.br/topic/44836-como-instalar-o-openssl-passo-a-passo-para-instalar-o-openssl-no-windows/#:~:text=Passo%20a%20passo%20para%20instalar%20o%20OpenSSL%20no,do%20Windows.%20...%204%204.%20Execute%20o%20OpenSSL


Executar no CMD DOS os comandos abaixo para gerar o certificado:


openssl req -x509 -newkey rsa:4096 -keyout serverPrivateKey.pem -out server.crt -days 3650 -nodes


openssl pkcs12 -export -out keyStore.p12 -inkey serverPrivateKey.pem -in server.crt


keytool -import -trustcacerts -alias root -file server.crt -keystore trustStore.jks

openssl req -new -newkey rsa:4096 -out request.csr -keyout myPrivateKey.pem -nodes


openssl x509 -req -days 360 -in request.csr -CA server.crt -CAkey serverPrivateKey.pem -CAcreateserial -out nardone.crt -sha256

openssl x509 -text -noout -in nardone.crt

openssl pkcs12 -export -out client_nardone.p12 -inkey myPrivateKey.pem -in nardone.crt -certfile server.crt


##IMPORTANTE

Usar changeit nos password e nardone em Common Name (e.g. server FQDN or YOUR name) quando gerar os certificados.


##IMPORTAR O CERTIFICADO NO CHROME

To import that file into the client certificates of Chrome, do the following:
1. Click Chrome ➤ Settings.
2. Click the Advanced tab.
3. Click View Certificates.
4. Click Import.
5. Locate client_nardone.p12 in the directory where it is stored,
   and double-click it.
6. Use changeit as the password when requested.

The next step is to add the server certificate to the Trusted Root Certificate
Authorities list in the web browser.


# =>Teste

https://localhost:8443/admin


