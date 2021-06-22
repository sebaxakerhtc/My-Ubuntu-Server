# Example botman installation on ubuntu

### Composer
```
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
sudo chmod +x /usr/local/bin/composer
```
### Botman
```
sudo apt update
sudo apt install php libapache2-mod-php php-mbstring php-xmlrpc php-soap php-gd php-xml php-cli php-zip php-bcmath php-tokenizer php-json php-pear php-curl
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
php artisan serve
```
