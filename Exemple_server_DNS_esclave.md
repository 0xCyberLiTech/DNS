<a name="Exemple_server_DNS_esclave.md"></a>
![Apache_logo](./images/Apache_logo.png)

| Cat | Introduction : |
|------|------|
| - A. | [Mise en place d'un serveur DNS (esclave).](#balise_01) |
| - B. | [Transfert de Zone Master > Slave.](#balise_02) |
| - C. | [Configuration Serveur Esclave.](#balise_03) |
| - D. | [Configuration DNS (resolv.conf).](#balise_04) |
| - E. | [Tests DNS Esclave.](#balise_05) |

<a name="balise_01"></a>
# - A. Mise en place d'un serveur DNS (esclave) sur Debian 11 ou Debian 12.

Schéma de principe pour la réalisation de notre maquette de labo.

Affectation des taches de chaques serveurs :

- (srv-linux-01) - Serveur (Nagios Core + NRPE + Smokeping),     192.168.50.200.
- (srv-linux-02) - Serveur (Test),                               192.168.50.201.
- (srv-linux-03) - serveur (DNS maître),                         192.168.50.203.
- (srv-linux-04) - serveur (DNS esclave),                        192.168.50.204.

![Apache_logo](./images/schema_nagios_01.png)

# DNS serveur slave (Esclave).

Maintenant que nous avons configuré un serveur maître, nous allons configurer un serveur esclave  pour assurer une disponibilité du service en cas de panne du serveur maître.

Nous installons BIND9 sur le serveur srv-linux-04.cyberlitech.lan sans configuration particulière pour le moment :
```
apt-get install bind9 bind9-doc resolvconf ufw
```
<a name="balise_02"></a>
## - B. Transfert de Zone Master > Slave.

Nous allons configurer le serveur Maître (srv-linux-03.cyberlitech.lan) pour autoriser le transfert de zone vers le serveur esclave.

Retourner sur le serveur Maître et éditez le fichier /etc/bind/named.conf.local.

Rajouter ces deux lignes suivantes dans la zone cyberlitech.lan et dans la zone inverse cyberlitech.lan.
```
notify yes;
allow-transfer { 192.168.50.204; };
```
```
nano /etc/bind/named.conf.local

//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";

// zone cyberlitech.lan
zone "cyberlitech.lan" IN {
           type master;
           file "/etc/bind/cyberlitech.fw.zone";
           notify yes;
           allow-transfer { 192.168.50.204; };
           };

// zone inverse cyberlitech.lan
zone "50.168.192.in-addr.arpa" {
           type master;
           file "/etc/bind/cyberlitech.rev.zone";
           notify yes;
           allow-transfer { 192.168.50.204; };
           };

```
Enregistrer : Ctrl+o et entrée. Quitter : Ctrl+x.

Enregistrement A Slave (cyberlitech.fw.zone).

Finissons la configuration sur le serveur maître avant de passer au serveur esclave.

Éditez le fichier de zone /etc/bind/cyberlitech.fw.zone et ajoutez l’adresse du serveur esclave :
```
nano /etc/bind/cyberlitech.fw.zone

;
; BIND data file for local loopback interface
;
$TTL             604800
@                IN      SOA     srv-linux-03.cyberlitech.lan. root.cyberlitech.lan. (
                                   20181226         ; Serial
                                     604800         ; Refresh
                                      86400         ; Retry
                                    2419200         ; Expire
                                     604800 )       ; Negative Cache TTL

; Serveur DNS

@                IN      NS      srv-linux-03.cyberlitech.lan.
@                IN      NS      srv-linux-04.cyberlitech.lan.
@                IN      A       192.168.50.203
@                IN      A       192.168.50.204

; Resolve DNS

srv-linux-03     IN      A       192.168.50.203
srv-linux-04     IN      A       192.168.50.204

; Machine du domaine

srv-linux-01     IN      A       192.168.50.200
srv-linux-02     IN      A       192.168.50.201

```
Enregistrer : Ctrl+o et entrée. Quitter : Ctrl+x

Enregistrement PTR Slave (cyberlitech.rev.zone).

Éditez le fichier de zone /etc/bind/cyberlitech.rev.zone et ajoutez l’adresse du serveur esclave (reverse) :
```
nano /etc/bind/cyberlitech.rev.zone

;
; BIND reverse data file for local loopback interface
;
$TTL            604800
@               IN     SOA      srv-linux-03.cyberlitech.lan. root.cyberlitech.lan. (
                                  20181226      ; Serial
                                    604800      ; Refresh
                                     86400      ; Retry
                                   2419200      ; Expire
                                    604800 )    ; Negative Cache TTL

; Serveur DNS

@               IN      NS      srv-linux-03.cyberlitech.lan.
@               IN      NS      srv-linux-04.cyberlitech.lan.
@               IN      PTR     cyberlitech.lan.

; Resolve DNS

srv-linux-03    IN      A       192.168.50.203
srv-linux-04    IN      A       192.168.50.204

; Machine du domaine

203             IN      PTR     srv-linux-03.cyberlitech.lan.
204             IN      PTR     srv-linux-04.cyberlitech.lan.
200             IN      PTR     srv-linux-01.cyberlitech.lan.
201             IN      PTR     srv-linux-02.cyberlitech.lan.
```
Enregistrer : Ctrl+o et entrée. Quitter : Ctrl+x

Relancer le service bind9 :
```
systemctl restart bind9
```
RAS

<a name="balise_03"></a>
## - C. Configuration Serveur Esclave.

Nous allons maintenant configurer le serveur esclave pour qu’il puisse récupérer les zones du serveur maître.

Allez maintenant sur le serveur esclave srv-linux-04.cyberlitech.lan et éditez le fichier /etc/bind/named.conf.local.
```
nano /etc/bind/named.conf.local

//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";

zone "cyberlitech.lan" {
           type slave;
           file "/var/cache/bind/cyberlitech.fw.zone";
           masters { 192.168.50.203; };
           };

zone "50.168.192.in-addr.arpa" {
           type slave;
           file "/var/cache/bind/cyberlitech.rev.zone";
           masters { 192.168.50.203; };
           };
```
Enregistrer : Ctrl+o et entrée. Quitter : Ctrl+x.

Relancer le service bind9 :
```
systemctl restart bind9
```
RAS
<a name="balise_04"></a>
## - D. Configuration DNS (resolv.conf).

Configuration DNS (resolv.conf).

Comme avec le serveur maître, modifier le fichier resolv.conf. Attention : si ce fichier se met à jour automatiquement (dynamique) par resolvconf, ne pas le modifier manuellement. C’est le cas également sur le serveur esclave:

srv-linux-03 - Serveur maître DNS - 192.168.50.203.

```
cat /etc/resolv.conf

# Dynamic resolv.conf(5) file for glibc resolver(3) generated by resolvconf(8)
#     DO NOT EDIT THIS FILE BY HAND -- YOUR CHANGES WILL BE OVERWRITTEN
# 127.0.0.53 is the systemd-resolved stub resolver.
# run "resolvectl status" to see details about the actual nameservers.

nameserver 127.0.0.1
search cyberlitech.lan
```
srv-linux-04 - Serveur esclave DNS - 192.168.50.204.
```
cat /etc/resolv.conf

# Dynamic resolv.conf(5) file for glibc resolver(3) generated by resolvconf(8)
#     DO NOT EDIT THIS FILE BY HAND -- YOUR CHANGES WILL BE OVERWRITTEN
# 127.0.0.53 is the systemd-resolved stub resolver.
# run "resolvectl status" to see details about the actual nameservers.

nameserver 192.168.50.203
search cyberlitech.lan
```
Vous devez alors ajouter le domaine et le serveur DNS depuis le fichier /etc/network/interfaces.
```
nano /etc/network/interfaces

# The primary network interface
#allow-hotplug enp0s3
#iface enp0s3 inet dhcp

# the primary network interface
allow-hotplug enp0s3
iface enp0s3 inet static
        address 192.168.50.204/24
        gateway 192.168.50.1
        dns-domain cyberlitech.lan
        dns-nameservers 192.168.50.203
```
Vous devez alors ajouter le domaine depuis le fichier /etc/hosts.
```
nano /etc/network/hosts
127.0.0.1       localhost.localdomain           localhost
192.168.50.204  srv-linux-04.cyberlitech.lan    sr-linux-04
```
Enregistrer : Ctrl+o et entrée. Quitter : Ctrl+x

Relancez le service réseau :
```
systemctl restart networking
```
<a name="balise_05"></a>
## - E. Tests DNS Esclave.

On peut vérifier de la même manière que sur le serveur esclave :
```
hostname
srv-linux-04
```
Test (nslookup srv-linux-01) depuis le serveur srv-linux-04.
```
nslookup srv-linux-01

nslookup srv-linux-01
Server:         192.168.50.203
Address:        192.168.50.203#53

Name:   srv-linux-01.cyberlitech.lan
Address: 192.168.50.200
```
Toujours depuis le serveur srv-linux-04 on test le reverse, nslookup 192.168.50.200.
```
nslookup 192.168.50.200

nslookup srv-linux-01
Server:         192.168.50.203
Address:        192.168.50.203#53

Name:   srv-linux-01.cyberlitech.lan
Address: 192.168.50.200
```
Toujours depuis le serveur srv-linux-04 on test dig -x 192.168.50.200.
```
dig -x 192.168.50.200
```
## Configuration du client windows sans DHCP, donc IP static.
```
Le DNS primaire - 192.168.50.203
Le DNS secondaire - 192.168.50.203
```
Pour tester mon serveur DNS, depuis cette machine même (Client Windows), je ping le domaine cyberlitech.lan
```
ping cyberlitech.lan
```
On peut constater que c’est le serveur maître qui répond à ma requête.

Maintenant, je vais arrêter le service bind9 sur le serveur maître (srv-linux-03.cyberlitech.lan)
```
systemctl stop bind9
```
```
ping cyberlitech.lan
```
Le serveur maître n’est plus joignable (srv-linux-03.cyberlitech.lan). 
C’est donc le serveur esclave (srv-linux-04.cyberlitech.lan) qui reprend le relais en attendant.

## Configuration Client Linux.

C’est le même principe sous Linux. 

Le client Debian est en mode DHCP, mais vous pouvez mettre une adresse IP statique à votre serveur . Voir l’exemple ci-dessous.

Exemple pour :

- srv-linux-01
```
cat /etc/network/interfaces

# the primary network interface
allow-hotplug enp0s3
iface enp0s3 inet static
        address 192.168.50.200/24
        gateway 192.168.50.1
        dns-domain cyberlitech.lan
        dns-nameservers 192.168.50.203 192.168.50.204
```
```
nano /etc/network/hosts

127.0.0.1       localhost.localdomain           localhost
192.168.50.200  srv-linux-01.cyberlitech.lan    sr-linux-01
```
```
nano /etc/resolv/conf

domain cyberlitech.lan
search cyberlitech.lan
nameserver 192.168.50.203
nameserver 192.168.50.204
```
- srv-linux-02
```
cat /etc/network/interfaces

# the primary network interface
allow-hotplug enp0s3
iface enp0s3 inet static
        address 192.168.50.201/24
        gateway 192.168.50.1
        dns-domain cyberlitech.lan
        dns-nameservers 192.168.50.203 192.168.50.204
```
```
nano /etc/network/hosts

127.0.0.1       localhost.localdomain           localhost
192.168.50.201  srv-linux-02.cyberlitech.lan    sr-linux-02
```
```
nano /etc/resolv/conf

domain cyberlitech.lan
search cyberlitech.lan
nameserver 192.168.50.203
nameserver 192.168.50.204
```
- Si vous être donc en IP statique, il ne faut pas oublier de rajouter les serveurs DNS dans /etc/resolv.conf. 
- Si vous êtes en DHCP, la machine Debian va tout récupérer automatiquement :

Allez, je ping le domaine cyberlitech.lan
```
ping cyberlitech.lan -c 2
```
On constat que c’est le serveur esclave qui répond à ma requête.

J’arrête le service bind9 sur le serveur esclave :
```
systemctl stop bind9
```
Je relance le ping sur le poste client :
```
ping cyberlitech.lan -C 2
```
Le serveur maître reprend le relais.
