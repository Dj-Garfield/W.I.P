# Installation Serveur DHCP Debian 10 + Routage
## Serveur avec 2 interfaces 

enp4s0 => WAN , Client DHCP depuis la BOX ( 192.168.1.0/24 )

enp0s25 Interface Physique pour le LAN
Le LAN est dans un VLAN ID : 200

enp0s25.200 => LAN Ip Fixe sur 172.17.1.0/24


## On declare les IFACES dans /etc/network/interfaces

Fichier interfaces :

#The loopback network interface
> auto lo
> iface lo inet loopback

> auto enp4s0
> iface enp4s0 inet dhcp

> auto enp0s25.200
> Allow-hotplug
> iface enp0s25.200 inet static
> address 172.17.1.1
> netmask 255.255.255.0
> up service isc-dhcp-server restart


## On configure le service DHCP Pour l'interface enp0s25.200

1/ dans /usr/share/isc-dhcp-server il faut preciser sur quelle IFACE ecoutera le service :

> /etc/default/isc-dhcp-server

## Ajouter l'interface supportant le service DHCP dans INTERFACESv4
> #On what interfaces should the DHCP server (dhcpd) serve DHCP requests?
> #Separate multiple interfaces with spaces, e.g. "eth0 eth1".
> INTERFACESv4="enp0s25.200"
> INTERFACESv6=""


2/ on configure le serveur proprement dit dans /etc/dhcp/dhcpd.conf

> cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.bak , pour garder la config originale

> option domain-name "eplkt.net";
> option domain-name-servers 192.168.1.254;       #Ip de la BOX ou d'un serveur DNS sur le Reseau
> default-lease-time 86400;
> max-lease-time 604800;
> ddns-update-style none;
> log-facility local7;
> authoritative;
> subnet 172.17.1.0 netmask 255.255.255.0 {
> range 172.17.1.10 172.17.1.150;                 #L'interface cote LAN a comme IP 172.17.1.1, il ne faut pas que le Range contienne cette adresse
> option subnet-mask 255.255.255.0;
> option broadcast-address 172.17.1.255;
> option routers 172.17.1.1;
> }

=======================================================
## voir les logs

tail -f /var/log/syslog

## faire un tri et voir juste les actions du serveur dhcp (le démon dhcpd) :

grep dhcpd /var/log/syslog

## connaitre la liste des machines auquel votre serveur DHCP a attribué une adresse (information détaillé Nom, adresse IP, adresse Mac) il suffit de lire le fichier.

cat /var/lib/dhcp/dhcpd.leases | more

ou

cat /var/lib/dhcp/dhcpd.leases 

# Routage entre les deux Interfaces
