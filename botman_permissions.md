# I was in trouble with botman after moving it from one server another. The solution is easy, but very dificult to find it. It works.
<p> 
<p> This commands totaly solve my problem

```
sudo a2enmod rewrite
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
