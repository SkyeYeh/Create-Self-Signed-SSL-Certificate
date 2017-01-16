# Create-Self-Signed-SSL-Certificate

* Create Root CA
* Create Intermediate CA
* Create Certificate
* Reference

## Create Root CA

1.  Create CA Public Key & Private Key

    ```
    openssl req -out rootcacert.cer -keyout rootcakey.pem -newkey rsa:2048 -days 7300 -x509
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
    openssl req -out intermediatecacsr.pem -keyout intermediatecakey.pem -newkey rsa:2048
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

    *   The Country Name, State or Province Name and Organization Name need match with Root CA (or edit [ policy_match ], [ policy_match ] in default openssl.cfg)

2.   Create Intermediate CA Public key

    ```
    mkdir demoCA
    echo. 2>"demoCA/index.txt"
    openssl ca -in intermediatecacsr.pem -out intermediatecacert.cer -cert rootcacert.cer -keyfile rootcakey.pem -days 1460 -outdir . -extensions v3_ca -create_serial
    ```

    > Enter pass phrase for rootcakey.pem: ``changeit``
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
    openssl req -out csr.pem -keyout key.pem -newkey rsa:2048
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

2.   Create CA Public key

    ```
    openssl x509 -in csr.pem -out cert.cer -CA intermediatecacert.cer -CAkey intermediatecakey.pem -days 90 -req -CAcreateserial
    ```

    > Enter pass phrase for intermediatecakey.pem: ``changeit``

    *   -req

        Change input from public key to certificate request

    *   -CAcreateserial

        Create a random serial

    *   -sha256 (default)

## Reference
* [OpenSSL Manpages Commands](https://www.openssl.org/docs/manmaster/man1/)
* [How to Create A Self Signed Certificate](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate.html)
* [OPENSSL常用語法彙整](https://www.sslbuyer.com/index.php?option=com_content&view=article&id=129:openssl-command-intro&catid=25&Itemid=4031)
