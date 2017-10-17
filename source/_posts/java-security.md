---
title: Java Security
---

安全网络应用程序开发的“三大项”――认证、授权和传输


Platform Security

Cryptography(密码)
1. Java Cryptography Architecture(JCA) 
2. includes the Java Cryptographic Extension (JCE)
JCA是一个跟密码处理相关的框架, 通过provider模式支持各种密码算法的注册, 这些算法用在各种安全方面的应用,譬如加密, 密钥生成和管理,随机的安全数字生成,数字证书验证等等。 
Message Authentication Code (MAC) 消息认证码算法
3.Authentication and Access Control
Java Authentication and Authorization Service (JAAS)

4.Secure Communications
Java Secure Socket Extension (JSSE)
Java GSS-API (JGSS)
Java SASL API

5. Public Key Infrastructure (PKI)
X.509 Certificate

Java 认证和授权服务（JAAS）
解决“三大项”中的前两项――认证、授权
验证用户就是他或者她所宣称的那个人（认证）。
保证允许他或者她访问所要求的资源（授权）。

JAAS 所使用的认证方案以两种非常重要的实体为基础：principal 和 subject。实际被认证的人或者服务称为 subject。 principal是一个惟一的实体(subject,比如个人或者组)的名字、帐号、社会安全号或者类似的惟一标识。

Java 安全套接字扩展（JSSE） -- 通过安全网络连接进行完整的信息交换（传输）
Java 安全套接字扩展（JSSE）利用 SSL/TLS 可以进行安全的 Internet 通信，它提供了一个具有完整功能的应用程序框架――一个 Java 版本的 SSL 和 TLS 协议

非对称密钥加密:
1. RSA
2. DSA (DSA只是一种算法，和RSA不同之处在于它不能用作加密和解密，也不能进行密钥交换，只用于签名,它比RSA要快很多.)
2. Elgamal
3. 背包算法
4. Rabin
5. D-H
5. ECC（椭圆曲线加密算法）

对称密钥加密:
1. DES
2. 3DES
3. AES 这个标准用来替代原先的DES
4. TDEA算法
5. Blowfish算法
6. RC5算法
7. IDEA算法

散列算法:(用户可以通过Hash算法对目标信息生成一段特定长度的唯一的Hash值，却不能通过这个Hash值重新获得目标信息)
1. SHA-1是基于MD4算法
2. MD5


非对称密钥加密:
1. 公钥加密： 我的朋友用公钥加密 ，只有我用私钥可以解开
2. 私钥加密： 我用私钥加密，我的朋友可以收到，并确实是我发的。这个过程叫做数字签名

keystore 存储:
*  密钥实体（Key entity）——密钥（secret key）又或者是私钥和配对公钥（采用非对称加密） 
*  可信任的证书实体（trusted certificate entries）——只包含公钥

支援的加密演算法
key-pair:DSA signature: SHA1withDSA
key-pair:RSA signature： MD5withRSA

支援的證書格式
X.500
X.509
RFC 1421

truststore和keystore的性质是一样的，都是存放key的一个仓库，区别在于:
* truststore里存放的是只包含公钥的数字证书，代表了可以信任的证书，
* 而keystore是包含私钥的。

there are a few different types of keystores in Java: JKS, JCEKS, PKCS12, PKCS11 and DKS

* JKS
* JCEKS
* PKCS12
* PKCS11
* DKS


证书: 是通过一个公正实体（CA，证书授权机构）来颁发并验证合法性。证书包含三方面内容：
证书可以保证发给需求者的内容确实是属于内容拥有者的。
1，实体名，即证书持有者。 
2，与主体相关的公开密钥。 
3，验证证书信息的数字签名。证书由证书发行人签名。 

SSL 双向认证
``` shell
keytool -genkey -alias mydomain -keyalg RSA -keystore keystore.jks -keysize 2048
//Generate a certificate signing request (CSR) for an existing Java keystore
keytool -certreq -alias mydomain -keystore keystore.jks -file mydomain.csr
keytool -import -trustcacerts -alias mydomain -file mydomain.crt -keystore keystore.jks
```

``` shell
为存在的keystore生成自签名证书:
keytool -genkey -keyalg RSA -alias selfsigned -keystore keystore.jks -storepass password -validity 360 -keysize 2048
```

