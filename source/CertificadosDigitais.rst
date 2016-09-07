Certificados Digitais
=====================

Linux
######

Algumas referências usadas: `DigitalOcean  <https://www.digitalocean.com/community/tutorials/openssl-essentials-working-with-ssl-certificates-private-keys-and-csrs>`_.

Informações de um Certificado
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

No linux, há algumas formas de verificar informações de certificados, veja algumas formas e suas ferramentas.
* **Keytool**
** **Ubuntu**::
    keytool -list -keystore /etc/ssl/certs/java/cacerts

** **RedHat**::
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
        Subject: CN=TESTEA-CA.teste.kos.local
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (1024 bit)
                Modulus:
                    00:a7:52:4f:4d:e8:1e:fc:92:e9:8f:51:7c:c0:95:
                    14:a6:46:e9:53:59:9c:56:19:56:af:a3:4c:0f:52:
                    ce:ca:e8:b0:13:0c:6d:d0:5d:db:8f:02:08:a6:ec:
                    fd:ad:64:2f:56:13:fb:c1:32:25:05:d5:c8:0c:e8:
                    46:81:74:9f:8a:8f:88:db:37:20:81:1b:55:c8:c3:
                    de:c4:91:fd:16:e8:5f:9a:90:b1:7d:b9:90:54:65:
                    64:09:02:35:52:0c:7c:cb:41:6f:3c:f7:12:91:87:
                    71:a1:6d:8f:40:03:ce:5c:9d:b1:8e:5e:f5:be:5c:
                    90:00:db:7c:f8:04:56:a2:97
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            1.3.6.1.4.1.311.20.2:
                . .D.o.m.a.i.n.C.o.n.t.r.o.l.l.e.r
            X509v3 Extended Key Usage:
                TLS Web Client Authentication, TLS Web Server Authentication
            X509v3 Key Usage:
                Digital Signature, Key Encipherment
            S/MIME Capabilities:
......0...`.H.e...*0...`.H.e...-0...`.H.e....0...`.H.e....0...+....0
..*.H..
            X509v3 Subject Key Identifier:
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
    ZS1WLVRFU1RFQUQwMS1DQTAeFw0xNjAzMDMxMjAwNDVaFw0xNzAzMDMxMjAwNDVa
    MCkxJzAlBgNVBAMTHlYtVEVTVEVBRDAyLnRlc3RlLmNhcGVzLmdvdi5icjCBnzAN
    BgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAp1JPTege/JLpj1F8wJUUpkbpU1mcVhlW
    r6NMD1LOyuiwEwxt0F3bjwIIpuz9rWQvVhT7wTYlBdXIDOhGgXSfio+I2zcggRtV
    yMPexJH9FuhfmpCxfbmQVGVkCQI1Ugx8y0NvPPcSkYdxo22PQAPOXJ2xjl71vlyQ
    ANt8+ARWopcCAwEAAaOCAyMwggMfMC8GCSsGAQQBgjcUAgQiHiAARABvAG0AYQBp
    AG4AQwBvAG4AdAByAG8AbABsAGUAcjAdBgNVHSUEFjAUBggrBgEFBQcDAgYIKwYB
    BQUHAwEwCwYDVR0PBAQDAgWgMHgGCSqGSIb3DQEJDwRrMGkwDgYIKoZIhvcNAwIC
    AgCAMA4GCCqGSIb3DQMEAgIAgDALBglghkaBZQMEASowCwYJYIZIAWUDBAEtMAsG
    CWCGSAFlAwQBAjALBglghkgBZQMEAQUwBwYFKw4DAgcwCgYIKoZIhvcNAwcwHQYD
    VR0OBBYEFG6iaA9QQGjCVHmRmYfo19rde7o2MB8GA1UdIwQYMBaAFJSjoUaQ2uE3
    hxQcy/5KKw8d0SrSMIHjBgNVHR8EgdswgdgwgdWggdKggc+GgcxsZGFwOi8vL0NO
    PXRlc3RlLVYtVEVTVEVBRDAxLUNBLANOPXYtdGVzdGVhZDAxLENOPUNEUCxDTj1Q
    dWJsaWMlMjBLZXklMjBTZXJ2aWNlcyxDTj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0
    aW9uLERDPXRlc3RlLERDPWNhcGVzLERDPWdvdixEQz1icj9jZXJ0aWZpY2F0ZVJl
    dm9jYXRpb25MaXN0P2Jhc2U/b2JqZWN0Q2xhc3M9Y1JMRGlzdHJpYnV0aW9uUG9p
    bnQwgdMGCCsGAQUFBwEBBIHGMIHDMIHABggrBgEFBQcwAoaBs2xkYXA6Ly8vQ049
    dGVzdGUtVi1URVNURUFEMDEtQ0EsQ029QUlBLENOPVB1YmxpYyUyMEtleSUyMFNl
    cnZpY2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9dGVzdGUsREM9
    Y2FwZXMsREM9Z292LERDPWJyP2NBQ0VydGlmaWNhdGU/YmFzZT9vYmplY3RDbGFz
    cz1jZXJ0aWZpY2F0aW9uQXV0aG9yaXR5MEoGA1UdEQRDMEGgHwYJKwYBBAGCNxkB
    oBIEEN+qavTZ7RhLotueYDzDmpSCHlatVEVTVEVBRDAyLnRlc3RlLmNhcGVzLmdv
    di5icjANBgkqhkiG9w0BAQUFAAOCAQEAkaF5BJ3sE7c9AikA5LFlrA5GdthigBWz
    Q8tUJFvBYOFJ+5ja7x3mOjUerzS7gTsSLCAux/JYTRdwu/1zAAgeWsbn2v0vmwV0
    6cd2ZHqKWIQjnH2tlWFofpQj6ZIbAK3KN364d+hO75iBqXXa63ceRPIBrJchtOQc
    6HVpuupJP6e30+MVTHwJ3RtK/3QcLQzxhYQQZ1jb4JPZHUYTxZhvldwcigMNghNi
    RjnZwvaHNvLbgvIaGjaKD9Z78U0O5hapnPZyyjZe7RVa23FZmIVkiR0m0Pk0E+G2
    /Ra4zk0NJyCAtNKN8ERWcmWrU+CTGCQJuNvmR0zQZTWnytMDMus7/Q==
    -----END CERTIFICATE-----


* **Fingerprint** - para verificar uma impressão digital (fingerprint) de um certificado no linux::

    openssl x509 -noout -in <CERTIFICADO>.cer -fingerprint

Exemplo de saída::

    SHA1 Fingerprint=89:50:B2:D1:3C:68:FC:A2:B2:A6:EE:22:20:EC:6B:4D:B8:5B:C2:70


Importar um certificado
~~~~~~~~~~~~~~~~~~~~~~~~

Veja como importar um certificado::

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


Windows
########
