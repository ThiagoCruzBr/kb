Certificados Digitais
=====================

Linux
######

Algumas referências usadas: `DigitalOcean  <https://www.digitalocean.com/community/tutorials/openssl-essentials-working-with-ssl-certificates-private-keys-and-csrs>`_.

Informações de um Certificado
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

No linux, há algumas formas de verificar informações de certificados, veja algumas formas e suas ferramentas.

* **Keytool**

No Ubuntu::
    keytool -list -keystore /etc/ssl/certs/java/cacerts

No RedHat::
    keytool -list -keystore /etc/pki/java/cacerts

Exemplo de saída::

    Keystore type: JKS
    Keystore provider: SUN

    Your keystore contains 174 entries

    debian:staat_der_nederlanden_root_ca_-_g3.pem, Jul 14, 2016, trustedCertEntry,
    Certificate fingerprint (SHA1): D8:EB:6B:41:51:92:59:E0:F3:E7:85:00:C0:3D:B6:88:97:C9:EE:FC
    debian:pscprocert.pem, Jul 14, 2016, trustedCertEntry,
    Certificate fingerprint (SHA1): 70:C1:8D:74:B4:28:81:0A:E4:FD:A5:75:D7:01:9F:99:B0:3D:50:74
    debian:chambers_of_commerce_root_-_2008.pem, Jul 14, 2016, trustedCertEntry,
    Certificate fingerprint (SHA1): 78:6A:74:AC:76:AB:14:7F:9C:6A:30:50:BA:9E:A8:7E:FE:9A:CE:3C
    debian:ca_disig_root_r2.pem, Jul 14, 2016, trustedCertEntry,
    ...


* **OpenSSL**

Para verificar informações como fingerprint, versão, algoritmos de assinatura, dentro outros a ferramenta OpenSSL. Algumas `referências sobre <https://www.sslshopper.com/article-most-common-openssl-commands.html>`_.
Verificar Certificados - listar informação do certificado padrão x509::

    openssl x509 -in <CERTIFICADO>.crt -text -noout

Exemplo de saída::

    Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            72:c6:63:0d:00:00:00:00:00:0b
    Signature Algorithm: sha1WithRSAEncryption
        Issuer: DC=local, DC=kos, DC=teste, CN=TESTE-CA
        Validity
            Not Before: Mar  3 12:00:45 2016 GMT
            Not After : Mar  3 12:00:45 2017 GMT

            . . .



Verificar Certificados
~~~~~~~~~~~~~~~~~~~~~~

Listar somente o certificado em base 64::

    openssl x509 -in <CERTIFICADO>.crt

Exemplo de saída::

    -----BEGIN CERTIFICATE-----
    MIIFxDCCBKygAwIBAgIKcsZjDQAAAAAACzANBgkqhkiG9w0BAQUFADB2MRIwEAYK
    CZImiZPyLGQBGRYCYnIxEzARBgoJkiaJk/IsZAEZFgNnb3YxFTATBgoJkiaJk/Is
    ZAEZFgVjYXBlczEVMBMGCgmSJomT8ixkARkWBXRlc3RlMR0wGwYDVQQDExR0ZXN0

    . . .

    RjnZwvaHNvLbgvIaGjaKD9Z78U0O5hapnPZyyjZe7RVa23FZmIVkiR0m0Pk0E+G2
    /Ra4zk0NJyCAtNKN8ERWcmWrU+CTGCQJuNvmR0zQZTWnytMDMus7/Q==
    -----END CERTIFICATE-----


* **Fingerprint** - para verificar uma impressão digital (fingerprint) de um certificado no linux::

    openssl x509 -noout -in <CERTIFICADO>.cer -fingerprint

Exemplo de saída::

    SHA1 Fingerprint=89:50:B2:D1:3C:68:FC:A2:B2:A6:EE:22:20:EC:6B:4D:B8:5B:C2:70


Importar um certificado
~~~~~~~~~~~~~~~~~~~~~~~~

Veja como importar um certificado.

* **Ubuntu**::

    \\ Sem alias
    keytool -importcert -keystore  /etc/ssl/certs/java/cacerts -file <CERTIFICADO.CER>

    \\ Com alias.
    keytool -importcert -alias activedirectory:vtestead02 -keystore  /etc/ssl/certs/java/cacerts -file <CERTIFICADO.CER>

* **Red Hat**:::

    keytool -importcert -keystore /etc/pki/java/cacerts -file certificado.cer

Ele ficará armazenado na keystore. Valide pela sua impressão digital (fingerprint)


Gerar um Certificado
~~~~~~~~~~~~~~~~~~~~

Certificados Auto-Assinados - utilizando o OpenSSL com chave privada de 2048 (dominio_TESTE.key) e um certificado auto-assinado (dominio_TESTE.crt) para utilização em sites com HTTPS::

    openssl req -newkey rsa:2048 -nodes -keyout dominio_TESTE.key -x509 -days 365 -out <dominio_TESTE>.crt




Exportar um Certificado
~~~~~~~~~~~~~~~~~~~~~~~~

Formato Java Keystore - para exportar um certificado de uma java keystore para formato x509::

    keytool -keystore </PASTA/.keystore OU arquivo.kjs> -exportcert -alias NOME_CERTIFICADO | openssl x509 -inform der -text > <CERTIFICADO>.crt




Remover um Certificado
~~~~~~~~~~~~~~~~~~~~~~
Para remover certificados::

    keytool -delete -keystore /etc/ssl/certs/java/cacerts -alias <CERTIFICADO_DESEJADO_PARA_EXCLUIR>
