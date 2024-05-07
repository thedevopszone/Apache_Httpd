# Apache Httpd

## Ubuntu

```
apt update

apt install net-tools

apt install apache2

ufw allow 80/tcp
ufw allow 443/tcp
ufw allow 22/tcp
sudo ufw allow https
ufw enable

cd /etc/apache2/sites-available/

vi 000-default.conf
---
#<VirtualHost *:80>
#    ServerName grafana-server.home54.de
#
#    ProxyPreserveHost On
#    ProxyPass / http://localhost:3000/
#    ProxyPassReverse / http://localhost:3000/

#    ErrorLog ${APACHE_LOG_DIR}/grafana_error.log
#    CustomLog ${APACHE_LOG_DIR}/grafana_access.log combined
#</VirtualHost>



<VirtualHost *:443>
    ServerName solr.home54.de
#    ServerName solr-e.home54.de
#    ServerName grafana-server.home54.de

    SSLEngine on
#    SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt
#    SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key

    SSLCertificateFile /etc/ssl/home54.pem
    SSLCertificateKeyFile /etc/ssl/home54.key

    ProxyPreserveHost On
    ProxyPass / http://localhost:8983/
    ProxyPassReverse / http://localhost:8983/

    ErrorLog ${APACHE_LOG_DIR}/grafana_error.log
    CustomLog ${APACHE_LOG_DIR}/grafana_access.log combined
</VirtualHost>
---

sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod ssl

sudo systemctl restart apache2
sudo systemctl status apache2

cd /etc/ssl

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt

apt install systemd-resolve
resolvectl flush-caches

```


## Rhel

```
dnf install httpd

sudo yum install mod_ssl

sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd

```

```
<VirtualHost *:80>
    ServerName iam-mon-e.dg-nexolution.de
    Redirect permanent / https://iam-mon-e.dg-nexolution.de/
</VirtualHost>

<VirtualHost *:443>
    ServerName iam-mon-e.dg-nexolution.de

    ProxyPreserveHost On
    ProxyRequests On
    SSLEngine On
    SSLProxyEngine On

    ProxyPass / http://localhost:3000/
    ProxyPassReverse / http://localhost:3000/

    SSLCertificateFile    /etc/ssl/wildcard-dg-nexolution.crt
    SSLCertificateKeyFile /etc/ssl/wildcard-dg-nexolution.key
    SSLCACertificateFile  /etc/ssl/wildcard-dg-nexolution.cc

#    ErrorLog ${APACHE_LOG_DIR}/grafana_error.log
#    CustomLog ${APACHE_LOG_DIR}/grafana_access.log combined
</VirtualHost>


```



