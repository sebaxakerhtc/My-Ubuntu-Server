# FTP server for www users

#### Install ftp-server
```
sudo apt update && sudo apt install vsftpd
```
#### Creating a site dirs
```
sudo mkdir -p /var/www/site1.ru/public_html
sudo mkdir -p /var/www/site2.ru/public_html
```
#### Adding users
```
sudo adduser site1-user
sudo adduser site2-user
```
#### Set site dir as home
```
sudo usermod -d /var/www/site1.ru site1-user
sudo usermod -d /var/www/site2.ru site2-user
```
#### Set write permissions to public_html
```
sudo chown site1-user:site1-user /var/www/site1.ru/public_html
sudo chown site1-user:site2-user /var/www/site2.ru/public_html
```
#### Deny SSH for www-ftp users
```
sudo nano /etc/ssh/sshd_config
```
##### /etc/ssh/sshd_config:
```
.
..
...
DenyUsers site1-user site2-user
```
#### Restart sshd
```
sudo service sshd restart
```
#### vsftpd config
```
sudo mv /etc/vsftpd.conf /etc/vsftpd.conf.bak
sudo nano /etc/vsftpd.conf
```
#### /etc/vsftpd.conf:
```
listen=NO
listen_ipv6=YES
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
use_localtime=YES
xferlog_enable=YES
connect_from_port_20=YES
chroot_local_user=YES
secure_chroot_dir=/var/run/vsftpd/empty
pam_service_name=vsftpd
force_dot_files=YES
pasv_min_port=40000
pasv_max_port=50000
```
#### Restart vsftpd
```
sudo systemctl restart vsftpd
```
