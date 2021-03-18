# I was in trouble with botman after moving it from one server another. The solution is easy, but very dificult to find it. It works.
<p> 
<p> This commands totaly solve my problem

```
sudo a2enmod rewrite
cd /var/www/html/botman_project
sudo chown -R $USER:www-data storage
sudo chown -R $USER:www-data bootstrap/cache
chmod -R 775 storage
chmod -R 775 bootstrap/cache
```
