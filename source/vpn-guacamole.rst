VPN com Guacamole
===============================
Veja como instalar e configurar o software Guacamole.

Visão Geral
-----------
Em resumo, a :ref:`instalação` e a :ref:`configuração` da solução será:

* **Sistema Operacional**: CentoOS 7
* **Servidor Web**: Apache (2.4.6)
* **Servidor de Aplicação**: Tomcat (7.0.54)
* **Banco de Dados**: MariaDB (5.5.47)
* **Compilador**:gcc
* **Softwares**: Guacamole (0.9.9) e Extensões (banco de dados e ldap)



.. _instalação:

Instalação
-----------

Desabilitar serviços que não são necessários::

    # Firewall
    systemctl disable firewalld
    systemctl stop firewalld
    # Postfix
    systemctl disable postfix
    systemctl stop postix
    # SELinux
    setenforce 0

Instalar e pacotes::

    yum install tomcat
    systemctl start tomcat
    systemctl enable tomcat

    yum install httpd
    systemctl start httpd
    systemctl enable httpd

. _Guacamole e Componentes:

A documentação[2] e os downloads[3] do softwares são:

Componentes do Guacamole - dependências de pacotes[4]

* **Repositórios necessários**::

    yum -y install epel-release wget
    wget -O /etc/yum.repos.d/home:felfert.repo http://download.opensuse.org/repositories/home:/felfert/Fedora_19/home:felfert.repo

* **Pacotes**::

    yum -y install cairo-devel freerdp-devel gcc java-1.8.0-openjdk.x86_64 libguac libguac-client-rdp libguac-client-ssh libguac-client-vnc libjpeg-turbo-devel libpng-devel libssh2-devel libtelnet-devel libvncserver-devel libvorbis-devel libwebp-devel openssl-devel pango-devel pulseaudio-libs-devel terminus-fonts tomcat tomcat-admin-webapps tomcat-webapps uuid-devel

* **Download Guacamole - v0.9.9** - realizar download do Guacamole a ser compilado::

    wget -O guacamole-0.9.9.war http://downloads.sourceforge.net/project/guacamole/current/binary/guacamole-0.9.9.war
    wget -O guacamole-server-0.9.9.tar.gz http://sourceforge.net/projects/guacamole/files/current/source/guacamole-server-0.9.9.tar.gz

* **Download das Extensões** - realizar download das extensões para utilização com banco de dados e sistema de diretório::

    wget http://sourceforge.net/projects/guacamole/files/current/extensions/guacamole-auth-jdbc-0.9.9.tar.gz
    wget http://dev.mysql.com/get/Downloads/Connector/j/mysql-connector-java-5.1.38.tar.gz
    wget https://sourceforge.net/projects/guacamole/files/current/extensions/guacamole-auth-ldap-0.9.9.tar.gz

* **Descompactar Guacamole e extensões** - extensões para integração com banco de dados MySQL e LDAP::

    tar -xzf guacamole-server-0.9.9.tar.gz
    tar -xzf guacamole-auth-jdbc-0.9.9.tar.gz
    tar -xzf mysql-connector-java-5.1.38.tar.gz
    tar -xzf guacamole-auth-ldap-0.9.9.tar.gz

* **Criar Diretório Guacamole** - será o local de instalação::

    mkdir /etc/guacamole
    mkdir /etc/guacamole/lib
    mkdir /etc/guacamole/extensions

* **Compilando e Instalando o Guacamole** - ao compilar o guacamole, será criado o binário guacd::

    cd guacamole-server-0.9.9
    ./configure --with-init-dir=/etc/init.d
    make
    make install
    ldconfig


* **Configurar inicialização automática do guacd**::

    chkconfig guacd on

* **Disponibilizando Arquivos** - após instalação dos componentes e extensões, disponibiliza-se para as devidas pastas::

    cp guacamole-0.9.9.war /etc/guacamole
    ln -s /etc/guacamole/guacamole-0.9.9.war /var/lib/tomcat/webapps/
    mv /usr/lib64/freerdp/guacdr.so /tmp
    ln -s /usr/local/lib/freerdp/* /usr/lib64/freerdp/.

    cp mysql-connector-java-5.1.38/mysql-connector-java-5.1.38-bin.jar /etc/guacamole/lib/
    cp guacamole-auth-jdbc-0.9.9/mysql/guacamole-auth-jdbc-mysql-0.9.9.jar /etc/guacamole/extensions/
    cp guacamole-auth-ldap-0.9.9/guacamole-auth-ldap-0.9.9.jar /etc/guacamole/extensions/

* **Link para Tomcat**::

    mkdir -p /usr/share/tomcat/.guacamole/{extensions,lib}
    ln -s /etc/guacamole/extensions/guacamole-auth-jdbc-mysql-0.9.9.jar /usr/share/tomcat/.guacamole/extensions/
    ln -s /etc/guacamole/lib/mysql-connector-java-5.1.38-bin.jar /usr/share/tomcat/.guacamole/lib/
    ln -s /etc/guacamole/guacamole.properties /usr/share/tomcat/.guacamole/

.. _banco_de_dados:

Banco de Dados
~~~~~~~~~~~~~~~~~~~

Com base na documentação[5], a utilização do banco de dados.

.. note:: Armazene as senhas em local seguro!


* **Propriedades do Guacamole** - essas são as configurações iniciais para subir o serviço.[6]::

    echo "# Configurações do Banco MySQL" >> /etc/guacamole/guacamole.properties
    echo "mysql-hostname: localhost" >> /etc/guacamole/guacamole.properties
    echo "mysql-port: 3306" >> /etc/guacamole/guacamole.properties
    echo "mysql-database: guacamole_db" >> /etc/guacamole/guacamole.properties
    echo "mysql-username: guacamole_user" >> /etc/guacamole/guacamole.properties
    echo "mysql-password: <SENHA>" >> /etc/guacamole/guacamole.properties

* **Instalar banco MariaDB** - caso necessário (utillizado na homologação) foi instalado um banco de dados local [7]::

    yum install install mariadb-server
    systemctl start mariadb
    systemctl enable mariadb

* **Melhorar segurança do banco**::
    mysql_secure_installation

* **Criar conta e banco de dados** - criando banco e importando esquemas::

    mysql -u root -p
    create database guacamole_db;
    create user 'guacamole_user'@'localhost' identified by 'SENHA';
    GRANT SELECT,INSERT,UPDATE,DELETE ON guacamole_db.* TO 'guacamole_user'@'localhost';
    flush privileges;
    quit

*  **Criando schema** - script com definições do esquema de dados::

    cat guacamole-auth-jdbc-0.9.9/mysql/schema/*.sql | mysql -u root -p guacamole_db

* **Finalizando a Instalação** 0 atualizar pacotes::

    yum update


Ao iniciar os serviços, o sistema estará disponível em ``<IP_DO_SERVIDOR>:8080/guacamole-0.9.9/`` (usuário/senha padrão:``guacadmin``).

.. note:: Troque a senha do guacadmin!

Proceda agora com a :ref:`configuração`.

.. _configuração:

Configuração
~~~~~~~~~~~~~~~~~~~

Após a instalação do serviço, a configuração foi feita com base na documentação do fabricante[1], fórum e mais informações [2]
