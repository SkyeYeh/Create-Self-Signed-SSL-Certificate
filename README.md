# Create-Self-Signed-SSL-Certificate

* Create Root CA
* Create Intermediate CA
* Create Certificate
* Reference

## Create Root CA

1.  Create CA Public Key & Private Key

    ```
    openssl req -out rootcacert.cer -newkey rsa:2048 -keyout rootcakey.pem -x509 -days 7300
    ```

    > Enter PEM pass phrase: ``changeit``
    >
    > Verifying - Enter PEM pass phrase: ``changeit``
    >
    > Country Name (2 letter code) [AU]: ``.``
    >
    > State or Province Name (full name) [Some-State]: ``.``
    >
    > Locality Name (eg, city) []: ``.``
    >
    > Organization Name (eg, company) [Internet Widgits Pty Ltd]: ``Demo Pty Ltd``
    >
    > Organizational Unit Name (eg, section) []: ``.``
    >
    > Common Name (e.g. server FQDN or YOUR name) []: ``Demo Root CA``
    >
    > Email Address []: ``.``

## Create Intermediate CA

1.   Create Intermediate CA Certificate Request & Private Key

    ```
    openssl req -out intermediatecacrt.pem -newkey rsa:2048 -keyout intermediatecakey.pem
    ```

    > Enter PEM pass phrase: ``changeit``
    >
    > Verifying - Enter PEM pass phrase: ``changeit``
    >
    > Country Name (2 letter code) [AU]: ``TW``
    >
    > State or Province Name (full name) [Some-State]: ``.``
    >
    > Locality Name (eg, city) []: ``.``
    >
    > Organization Name (eg, company) [Internet Widgits Pty Ltd]: ``Demo Pty Ltd``
    >
    > Organizational Unit Name (eg, section) []: ``.``
    >
    > Common Name (e.g. server FQDN or YOUR name) []: ``Demo Intermediate CA``
    >
    > Email Address []: ``.``
    >
    > A challenge password []: ``.``
    >
    > An optional company name []: ``.``

2.   Create Intermediate CA Public key

    ```
    openssl x509 -in intermediatecacrt.pem -out intermediatecacert.cer -days 1460 -req -CA rootcacert.cer -CAkey rootcakey.pem -CAcreateserial -sha256
    ```

    > Enter pass phrase for rootcakey.pem: ``changeit``

## Create Certificate

4.   Create Certificate Request & Private Key

    ```
    openssl req -out crt.pem -newkey rsa:2048 -keyout key.pem
    ```

    > Enter PEM pass phrase: ``changeit``
    >
    > Verifying - Enter PEM pass phrase: ``changeit``
    >
    > Country Name (2 letter code) [AU]: ``.``
    >
    > State or Province Name (full name) [Some-State]: ``.``
    >
    > Locality Name (eg, city) []: ``.``
    >
    > Organization Name (eg, company) [Internet Widgits Pty Ltd]: ``.``
    >
    > Organizational Unit Name (eg, section) []: ``.``
    >
    > Common Name (e.g. server FQDN or YOUR name) []: ``10.0.0.1``
    >
    > Email Address []: ``.``
    >
    > A challenge password []: ``.``
    >
    > An optional company name []: ``.``

5.   Create CA Public key

    ```
    openssl x509 -in crt.pem -out cert.cer -days 90 -req -CA intermediatecacert.cer -CAkey intermediatecakey.pem -CAcreateserial -sha256
    ```

    > Enter pass phrase for intermediatecakey.pem: ``changeit``

## Reference
* [OpenSSL Manpages Commands](https://www.openssl.org/docs/manmaster/man1/)
* [How to Create A Self Signed Certificate](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate.html)
* [OPENSSL常用語法彙整](https://www.sslbuyer.com/index.php?option=com_content&view=article&id=129:openssl-command-intro&catid=25&Itemid=4031)
