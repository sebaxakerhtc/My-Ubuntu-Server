# Read for all but write for me

# install samba
```
sudo apt install samba
```
# backing up old config
```
sudo mv /etc/samba/smb.conf /etc/samba/smb.conf.backup
```
# create new config
```
sudo nano /etc/samba/smb.conf
```
# insert code
```
[global]
workgroup = WORKGROUP
server string = %h server (Samba, Ubuntu)
map to guest = bad user
usershare allow guests = yes
# allow run *.exe files from SMB
acl allow execute always = True

[pcserviceburgas.com]
path = /raid10/files
browsable = yes
writable = yes
guest ok = yes
# hide folder
veto files = hidenfoldername
```
# create folder
```
sudo mkdir -p /raid10/files
```
# give it to me ;)
```
sudo chown -R sebaxakerhtc /raid10/files
```
# restart services
```
sudo systemctl restart smbd nmbd
```
# samba dir writable for all
```
sudo adduser --system mainhero
sudo mkdir -p /failopomojka
sudo chown -R mainhero /failopomojka

sudo nano /etc/samba/smb.conf

[failopomojka]
path = /failopomojka
browsable = yes
writable = yes
guest ok = yes
public = yes
veto files = lost+found
create mask = 0644
directory mask = 0755
force user = mainhero

sudo systemctl restart smbd nmbd
```
# add user for samba only
```
sudo adduser --no-create-home --disabled-password --disabled-login smbuser-1
sudo smbpasswd -a smbuser-1
```
