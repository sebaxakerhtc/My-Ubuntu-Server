<p> To not add sudo to many comands...
    
```
sudo -s
```
<p> Install items
    
```
apt install -y isc-dhcp-server tftpd-hpa targetcli-fb
```
<p> Create dir for iSCSI images
    
```
mkdir /var/lib/iscsi
```
<p> Look for the interface
    
```
ip address show
```
<p> Edit interface for DHCP
    
```
nano /etc/default/isc-dhcp-server
```
<p> Insert your interface
    
```
INTERFACESv4="enp0s3"
```

<p> Backing up original dhcpd.conf and insert code to new one
    
```
mv /etc/dhcp/dhcpd.conf{,_orig}
nano /etc/dhcp/dhcpd.conf
```
<p> Code to insert
    
```
option space PXE;
option PXE.mtftp-ip    code 1 = ip-address;
option PXE.mtftp-cport code 2 = unsigned integer 16;
option PXE.mtftp-sport code 3 = unsigned integer 16;
option PXE.mtftp-tmout code 4 = unsigned integer 8;
option PXE.mtftp-delay code 5 = unsigned integer 8;
option arch code 93 = unsigned integer 16; # RFC4578

use-host-decl-names on;
ddns-update-style interim;
ignore client-updates;
next-server 192.168.0.150;
authoritative;


subnet 192.168.0.0 netmask 255.255.255.0 {
    option subnet-mask 255.255.255.0;
    range dynamic-bootp 192.168.0.151 192.168.0.249;
    default-lease-time 300;
    max-lease-time 500;
    option domain-name-servers 192.168.0.1;
    option routers 192.168.0.1;
 
    class "UEFI-32-1" {
    match if substring(option vendor-class-identifier, 0, 20) = "PXEClient:Arch:00006";
    filename "i386-efi/ipxe.efi";
    }

    class "UEFI-32-2" {
    match if substring(option vendor-class-identifier, 0, 20) = "PXEClient:Arch:00002";
    filename "i386-efi/ipxe.efi";
    }

    class "UEFI-64-1" {
    match if substring(option vendor-class-identifier, 0, 20) = "PXEClient:Arch:00007";
    filename "ipxe.efi";
    }

    class "UEFI-64-2" {
    match if substring(option vendor-class-identifier, 0, 20) = "PXEClient:Arch:00008";
    filename "ipxe.efi";
    }

    class "UEFI-64-3" {
    match if substring(option vendor-class-identifier, 0, 20) = "PXEClient:Arch:00009";
    filename "ipxe.efi";
    }

    class "Legacy" {
    match if substring(option vendor-class-identifier, 0, 20) = "PXEClient:Arch:00000";
    filename "ipxe.kkpxe";
    }

}
```
<p> Restart DHCP server
    
```
service isc-dhcp-server restart
```
<p> Dummy interface for iSCSI to work from all the world
<p> Without it i can't add my static ip to Targetcli
<p> It can't add ip address, which it don't see

```
nano /etc/netplan/01-network-manager-all.yaml
```
<p> Code to insert
    
```
network:
  ethernets:
    enp0s3:
      dhcp4: true
  bridges:
    iscsi0:
      interfaces: []
      addresses: [your.static.ip.address/24]
```
<p> Restart netplan
    
```
sudo netplan apply
```
    
<p> Look for your new interface

```
ip address show
```

# Create iSCSI
<p> Sample code
    
```
targetcli
clearconfig confirm=true
cd /backstores/fileio
create install-image /var/lib/iscsi/install-image.img 5G
cd /iscsi
create iqn.2018-06.io.github.sebaxakerhtc:install-image
cd iqn.2018-06.io.github.sebaxakerhtc:install-image/tpg1/luns
create /backstores/fileio/install-image
cd /iscsi/iqn.2018-06.io.github.sebaxakerhtc:install-image/tpg1/portals
delete 0.0.0.0 3260
create 192.168.0.150 3260
create your.static.ip.address 3260
cd /iscsi/iqn.2018-06.io.github.sebaxakerhtc:install-image/tpg1/acls
create iqn.2018-06.io.github.sebaxakerhtc:iSCSI-name-of-client
cd /iscsi/iqn.2018-06.io.github.sebaxakerhtc:install-image/tpg1
set parameter AuthMethod=None
set attribute authentication=0 demo_mode_write_protect=0
exit
y
```

<p> Open ports
    
```
sudo ufw allow from any to any port 3260,3261 proto tcp
sudo ufw allow from any to any port 3260,3261 proto udp
```
<p> Remove auth

```
********/tpg1
set parameter AuthMethod=None
set attribute authentication=0 demo_mode_write_protect=0 generate_node_acls=1 cache_dynamic_acls=1
```
<p> ON or OFF "Write Protection"
    
```
/etc/rtslib-fb-target/saveconfig.json
```
<p> look for:
write_protect: true or false
