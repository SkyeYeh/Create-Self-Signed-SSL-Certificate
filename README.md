# Create-Self-Signed-SSL-Certificate

* [Create Root CA](#create-root-ca)
* [Create Intermediate CA](#create-intermediate-ca)
* [Create Certificate](#create-certificate)
* [Reference](#reference)

## Create Root CA

1.  Create CA Public Key & Private Key

    ```
    openssl req -out rootca.crt -keyout rootca.key -newkey rsa:2048 -days 7300 -x509
    ```

    > Enter PEM pass phrase: ``changeit``
    >
    > Verifying - Enter PEM pass phrase: ``changeit``
    >
    > Country Name (2 letter code) [AU]: ``TW``
    >
    > State or Province Name (full name) [Some-State]: ``Taiwan``
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

    *   -x509

        Change output from certificate request to public key.

    *   -sha256 (default)

## Create Intermediate CA

1.   Create Intermediate CA Certificate Request & Private Key

    ```
    openssl req -out intermediateca.csr -keyout intermediateca.key -newkey rsa:2048
    ```

    > Enter PEM pass phrase: ``changeit``
    >
    > Verifying - Enter PEM pass phrase: ``changeit``
    >
    > Country Name (2 letter code) [AU]: ``TW``
    >
    > State or Province Name (full name) [Some-State]: ``Taiwan``
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

    *   The Country Name, State or Province Name and Organization Name need match with Root CA

        (or edit [ policy_match ], [ policy_match ] in default openssl.cfg)

2.   Create Intermediate CA Public key

    ```
    mkdir demoCA
    echo. 2>"demoCA/index.txt"
    openssl ca -in intermediateca.csr -out intermediateca.crt -cert rootca.crt -keyfile rootca.key -days 1460 -outdir . -extensions v3_ca -create_serial
    ```

    > Enter pass phrase for rootca.key: ``changeit``
    >
    > Sign the certificate? [y/n]: ``y``
    >
    > 1 out of 1 certificate requests certified, commit? [y/n] ``y``

    *   -outdir .

        Set new_certs_dir = .(default is ./demoCA/newcerts)

    *   -extensions v3_ca

        Set basicConstraints = critical,CA:true ([ v3_ca ] in default openssl.cfg)

    *   -create_serial

        Create a random serial

    *   -md sha256 (default)

## Create Certificate

1.   Create Certificate Request & Private Key

    ```
    openssl req -out cert.csr -keyout cert.key -newkey rsa:2048 -nodes
    ```

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
    > Common Name (e.g. server FQDN or YOUR name) []: ``www.domain.dom (use your domain)``
    >
    > Email Address []: ``.``
    >
    > A challenge password []: ``.``
    >
    > An optional company name []: ``.``

    *   -nodes

        No need PEM pass, Then no need enter the password when server start.

2.   Create CA Public key

    ```
    openssl x509 -in cert.csr -out cert.crt -CA intermediateca.crt -CAkey intermediateca.key -days 90 -req -CAcreateserial
    ```

    > Enter pass phrase for intermediateca.key: ``changeit``

    *   -req

        Change input from public key to certificate request

    *   -CAcreateserial

        Create a random serial

    *   -sha256 (default)

## Reference
* [OpenSSL Manpages Commands](https://www.openssl.org/docs/manmaster/man1/)
* [How to Create A Self Signed Certificate](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate.html)
* [OPENSSL常用語法彙整](https://www.sslbuyer.com/index.php?option=com_content&view=article&id=129:openssl-command-intro&catid=25&Itemid=4031)
