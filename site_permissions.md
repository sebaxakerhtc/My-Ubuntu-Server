# I was in trouble with one joomla site after moving it from one server to another. The solution is easy, but very dificult to find it. It works.
<p> 
<p> This commands totaly solve my problem

```
find /var/www/html/yourdomain.com -type d -exec chmod 755 {} +
find /var/www/html/yourdomain.com -type f -exec chmod 644 {} +
cd /var/www/html/yourdomain.com
chown -R www-data .
```
