Logs
====


Tomcat
~~~~~~


Arquivo ``etc/rsyslog.d/tomcat.conf``::

    programname,contains,"server" /var/log/tomcat/catalina.out
    programname,contains,"server" ~

Reiniciar serviço::

    systemctl restart rsyslog
