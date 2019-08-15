# 前言
最近项目里面用到了SSL双向认证和传输加密的技术，研究了一下，想把相关的指令和代码分享出来，以期后来者能够少踩坑，顺利解决问题。我们的项目服务器是C，客户端为Java，CS架构，中间通过Socket通讯。

# OpenSSL和Java KeyStore本质上没有关系，只是客户端用到Java，Java里面SSL认证加密的密码和证书需要存储到KeyStore这个容器里面，所以OpenSSL产生的相关资料需要导入keyStore容器。当然也可以反过来，用Java的KeyTool产生资料，再导出密码、证书，给服务器端C使用。不过OpenSSL比Java的keytool强大很多，故选择前一种方式。OpenSSL产生资料，存入KeyStore。本文只介绍用到的一系列指令。
这里涉及到三份证书。根证书(root.crt)，自签名的，用于给其它证书授权。服务器证书(server.crt)。以及客户端证书(client.crt)，一般系统不需要，我们的系统是双向认证，客户端也要证书。

# 指令列表
openssl genrsa -out rootkey.pem 2048  
生成根证书的密匙。  
openssl req -x509 -new -key rootkey.pem -out root.crt  
生成根证书。注意-x509，与步骤4和7不同。需要输入机构相关信息。  
openssl genrsa -out clientkey.pem 2048  
生成客户端的密匙。  
openssl req -new -key clientkey.pem -out client.csr 生成客户端证书的请求文件。请求根证书来签发。  
openssl x509 -req -in client.csr -CA root.crt -CAkey rootkey.pem -CAcreateserial -days 3650-out client.crt  
用根证书来签发客户端请求文件，生成客户端证书client.crt。  
openssl genrsa -out serverkey.pem 2048  
生成服务器端的密匙。  
openssl req -new -key serverkey.pem -out server.csr  
生成服务器端证书的请求文件。请求根证书来签发。  
openssl x509 -req -in server.csr -CA root.crt -CAkey rootkey.pem -CAcreateserial -days 3650-out server.crt  
用根证书来签发服务器端请求文件，生成服务器端证书server.crt。  
openssl pkcs12 -export -in client.crt -inkey clientkey.pem -out client.pkcs12  
打包客户端资料为pkcs12格式(client.pkcs12)。需要输入密码，请记住。  
openssl pkcs12 -export -in server.crt -inkey serverkey.pem -out server.pkcs12  
打包服务器端资料为pkcs12格式(server.pkcs12 )。需要输入密码，请记住。  
keytool -importkeystore -srckeystore client.pkcs12 -destkeystore client.jks -srcstoretype pkcs12  
生成客户端keystore(client.jks)。使用keytool的importkeystore指令。pkcs12转jks。需要pkcs12密码和jks密码。  
keytool -importkeystore -srckeystore server.pkcs12 -destkeystore server.jks -srcstoretype pkcs12  
生成服务器端keystore(server.jks)。使用keytool的importkeystore指令。pkcs12转jks。需要pkcs12密码和jks密码。  
keytool -importcert -keystore server.jks -file root.crt  
这一步不一定需要的。我发现服务器使用JDK6和JDK7的时候，必须要把根证书加到服务器证书里面，否则交谈失败。nul certification。Google里面很多结果，但是都不灵光。  

# 后记
理解概念是最重要的，路有很多条。以上指令还缺少几条，用来生成服务器端的trustkeystore，客户端的trustkeystore。trustkeystore里面不包含私匙。  




# 补上TrustKeyStore指令
keytool -importcert -alias ca -file root.crt -keystore clienttrust.jks  
生成Client端的对外KeyStore。先把根证书放到里面。  
keytool -importcert -alias clientcert -file client.crt -keystore clienttrust.jks  
把Client证书加到对外KeyStore里面。  
keytool -importcert -alias ca -file root.crt -keystore servertrust.jks  
生成Server端的对外KeyStore。先把根证书放到里面。  
keytool -importcert -alias servercert -file server.crt -keystore servertrust.jks  
把Server证书加到对外KeyStore里面。  

https://blog.csdn.net/kimylrong/article/details/43525333  