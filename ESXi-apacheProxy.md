# Working config for Apache2 Proxy for ESXi-SSL
<p> 
<p> Apache config port 80

```
<VirtualHost *:80>
        ServerName {VMWARE-DOMAIN-NAME}
        ServerAdmin admin@vmware.com
        Redirect permanent / https://{VMWARE-DOMAIN-NAME}
</VirtualHost>
```
<p> 
<p> Apache config port 443

```
<IfModule mod_ssl.c>
<VirtualHost *:443>
        ServerName {VMWARE-DOMAIN-NAME}
        ServerAdmin admin@vmware.com

SSLEngine On
SSLProxyEngine On
ProxyPreserveHost On
SSLProxyCheckPeerCN off
SSLProxyCheckPeerName off
SSLProxyCheckPeerExpire off
SSLProxyVerify none
ProxyPass /sdk/ https://{LOCAL-IP-ADDRESS}/sdk/
ProxyPassReverse /sdk/ https://{LOCAL-IP-ADDRESS}/sdk/
ProxyPass /ticket/ wss://{LOCAL-IP-ADDRESS}/ticket/
ProxyPassReverse /ticket/ wss://{LOCAL-IP-ADDRESS}/ticket/
ProxyPass / https://{LOCAL-IP-ADDRESS}/
ProxyPassReverse / https://{LOCAL-IP-ADDRESS}/

SSLCertificateFile /etc/letsencrypt/live/{VMWARE-DOMAIN-NAME}/fullchain.pem
SSLCertificateKeyFile /etc/letsencrypt/live/{VMWARE-DOMAIN-NAME}/privkey.pem
Include /etc/letsencrypt/options-ssl-apache.conf
</VirtualHost>
</IfModule>
```

<p> 
<p> Enable Apache2 module
  
```
a2enmod proxy_wstunnel
```

<p> 
<p> Copy certs to vmware
  
```
ssh {USER}@{VMWARE-ADDRESS} "cp /etc/vmware/ssl/castore.pem /etc/vmware/ssl/castore.pem.bak"
ssh {USER}@{VMWARE-ADDRESS} "cp /etc/vmware/ssl/rui.crt /etc/vmware/ssl/rui.crt.bak"
ssh {USER}@{VMWARE-ADDRESS} "cp /etc/vmware/ssl/rui.key /etc/vmware/ssl/rui.key.bak"
sudo scp /etc/letsencrypt/live/{CERTS-DIR}/fullchain.pem {USER}@{VMWARE-ADDRESS}:/etc/vmware/ssl/castore.pem
sudo scp /etc/letsencrypt/live/{CERTS-DIR}/cert.pem {USER}@{VMWARE-ADDRESS}:/etc/vmware/ssl/rui.crt
sudo scp /etc/letsencrypt/live/{CERTS-DIR}/privkey.pem {USER}@{VMWARE-ADDRESS}:/etc/vmware/ssl/rui.key
ssh {USER}@{VMWARE-ADDRESS} "services.sh restart"
```
