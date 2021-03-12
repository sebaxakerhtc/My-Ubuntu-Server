# Apache

<p> Install apache2 and create folders
  
```  
sudo apt install -y nano apache2

sudo systemctl enable apache2

sudo systemctl start apache2

sudo mkdir /var/www/html/sebaxakerhtc.github.io

sudo mkdir /var/www/html/sebaxakerhtc.github.io/public_html

sudo chmod -R 755 /var/www/html

sudo nano /var/www/html/sebaxakerhtc.github.io/public_html/index.html
```
<p> Insert code

```
<html>
<head>
<title>It's just a Test, but...</title>
</head>
<body>
<h1>IIIhhaa! Virtual host is working!</h1>
</body>
</html>
```
<p> Create config

```
sudo nano /etc/apache2/sites-available/sebaxakerhtc.github.io.conf
```
<p> Insert code and replace with your data

```
<VirtualHost *:80>
ServerName sebaxakerhtc.github.io
ServerAlias www.sebaxakerhtc.github.io
ServerAdmin sebaxakerhtc@localhost
DocumentRoot /var/www/html/sebaxakerhtc.github.io/public_html
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

<VirtualHost *:443>
ServerName sebaxakerhtc.github.io
ServerAlias www.sebaxakerhtc.github.io
ServerAdmin sebaxakerhtc@localhost
DocumentRoot /var/www/html/sebaxakerhtc.github.io/public_html
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
<p> Enable site and add it to your hosts

```
sudo a2ensite sebaxakerhtc.github.io.conf

sudo systemctl restart apache2

sudo nano /etc/hosts
```
<p> Insert

```
127.0.0.1	sebaxakerhtc.github.io
```
<p> Enable modules and restart apache

```
sudo a2enmod ssl

sudo a2ensite default-ssl

sudo systemctl restart apache2

```
# PHP
<p> Install and test
  
```
sudo apt install -y php

sudo nano /var/www/html/sebaxakerhtc.github.io/public_html/index.php

<?php phpinfo(); ?>

sudo systemctl restart apache2

sudo nano /etc/php/7.2/apache2/php.ini

display_errors = On
```

# MySQL
```

sudo apt install -y mariadb-server mariadb-client

sudo mysql_secure_installation
```
# phpmyadmin
```
sudo apt install -y phpmyadmin
sudo -i
echo "update user set plugin='' where User='root'; flush privileges;" | mysql -u root -p mysql
sudo ln -s /usr/share/phpmyadmin /var/www/html/
```
