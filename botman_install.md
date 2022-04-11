# Example botman installation on ubuntu

### Install necessary packages
```
sudo apt update
sudo apt install -y php curl libapache2-mod-php php-mbstring php-xmlrpc php-soap php-gd php-xml php-cli php-zip php-bcmath php-tokenizer php-json php-pear php-curl php-dom
```
### Install composer
```
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
sudo chmod +x /usr/local/bin/composer
```
### Install botman
```
composer global require "botman/installer"
echo 'export PATH="$PATH:$HOME/.config/composer/vendor/bin"' >> ~/.bashrc
source ~/.bashrc
```
### Create project
```
botman new testbot
cd testbot
composer up
cp .env.example .env
php artisan key:gen
```
# Enjoy!

### Publish your bot
<p> Copy project to www
  
`sudo cp -r testbot /var/www/`

<p> Enable rwrite mod
  
`sudo a2enmod rewrite`
  
<p> Resolve privilegies
  
```
cd /var/www/html/testbot
sudo chown -R $USER:www-data storage
sudo chown -R $USER:www-data bootstrap/cache
chmod -R 775 storage
chmod -R 775 bootstrap/cache
```
<p> Apache2 config
        
```
<VirtualHost *:80>
        ServerName bot.example.com
        ServerAdmin admin@example.com
        DocumentRoot /var/www/html/testbot/public
        <Directory /var/www/html/testbot>
            AllowOverride All
        </Directory>
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
