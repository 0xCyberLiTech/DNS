# Installer un serveur de DNS sous Debian 11 & Debian 12.

Serveur DNS

Le service DNS est essentiel aux réseaux et surtout à Internet. Il est utilisé par meme si les utilisateurs ne le réalisent pas. En effet, les échanges sur les réseaux se font à partir d'adresses IP de la forme 216.239.37.99

Chaque utilisateur est connecté par une adresse IP, et lorsque l'on se connecte à un site, on envoie des mails on sollicite en fait une IP. Pourquoi les utilisateurs ne voient pas les IP ? Car pour accéder à Internet, un PC a besoin de l'IP d'un serveur DNS. Ce serveur DNS va faire pour lui les résolutions DNS, c'est-à-dire convertir les noms en IP (et les IP en nom si necessaire). Par exemple, lorsque vous ouvrez un navigateur et pointez sur http://www.google.fr, votre PC envoie une requête à votre serveur DNS qui lui dit www.google.com = 216.239.37.99 et votre navigateur sollicite en réalité http://216.239.37.99

Lorsque vous vous abonnez à un FAI, celui-ci vous fournit les IP des serveurs DNS de votre FAI. Par exemple pour wanadoo, il s'agit de 193.252.19.3 et 193.252.19.4 ou pour nerim 62.4.16.70 et 62.4.17.69 

Si vous avez un réseau informatique, il est intéressant d'installer son propre serveur DNS. Celui-ci gérera non seulement les résolutions DNS du web mais aussi les resolutions de votre réseau local, c'est-à-dire convertir les noms de vos machines et leur IP (locales si vous n'avez pas acheté d'IP fixes). Celui-ci agira donc comme serveur DNS de cache. Les machines du réseau n'iront donc plus chercher les services DNS chez votre FAI mais ils iront sur votre serveur.

On peut aussi configurer le serveur DNS en tant que que serveur DNS primaire pour gérer un (ou plusieurs) noms de domaines.

Les adresses des serveurs DNS sont stockées dans le fichier /etc/resolv.conf qui contient une ou plusieurs adresses IP (il peut être utile d'avoir un serveur DNS secondaire prêt à pallier le premier en cas de pépin).

But de ce document

Ce document me sert de mémo pour installer un serveur Bind 9 sur une Debian. 
Ce document a été testé sur Debian Sarge Debian Etch Debian Lenny et Ubuntu 8.04

Pré-requis

Avoir installé au préalable un des OS cité si dessus.

Présentation rapide d’un système DNS

L’architecture de réseau TCP/IP sur lequel est basé Internet et la plupart des réseaux locaux actuels, utilisent des adresses IP numériques du type 192.168.0.1. Mais pour faciliter la lecture de ces adresses par l’homme, un système permet de transformer ces adresses en adresses plus lisibles comme www.cyberlitech.lan

Pour effectuer cette opération, il est nécessaire d’utiliser des serveurs DNS. Un serveur DNS fera donc la correspondance entre les adresses IP et les noms des domaines.

Un serveur DNS s’occupe en général d’un domaine limité et s’occupe de transmettre les questions à d’autres serveurs s’il ne connaît pas la répons

Principe de fonctionnement de la recherche de noms

Lorsque qu’une demande de résolution de nom est demandée, Linux commence par regarder le fichier « /etc/host.conf : »

# The "order" line is only used by old versions of the C library. 
order hosts,bind 
multi on

La première ligne de ce fichier indique qu’il faut commencer la recherche en regardant la table hosts locale et ensuite il faut interroger le serveur DNS.

La table hosts locale est enregistrée dans le fichier « /etc/hosts » elle contient une table de correspondance entre des adresses IP et des noms, elle ressemble à :
```
127.0.0.1           localhost.localdomain                 localhost 
192.168.0.253       srv-linux-01.cyberlitech.lan    srv-linux-01 
```
La première ligne est obligatoire pour que le système fonctionne même quand le réseau est désactivé. L’adresse IP 127.0.0.1 est toujours associée au nom localhost.

Les lignes suivantes peuvent être ajoutées manuellement pour faire la correspondance entre des adresses IP et des noms. C’est ce qui est fait en l’absence de serveur DNS.

Si le résultat n’est pas trouvé dans la table hosts,le système recherche le serveur DNS indiqué dans le fichier « /etc/resolv.conf» :
```
search cyberlitech.lan 
nameserver 192.168.0.253
nameserver 194.2.0.50
```
La première ligne indique quel domaine il faut ajouter au noms si celui-ci n’est pas indiqué lors d’une demande de résolution de nom. 

Exemple : 

ping monserveur.mondomaine.com -> Aucun domaine ne sera ajouté lors de la résolution du nom, car le domaine est fourni.

ping monserveur -> Le domaine mondomaine.com, sera ajouté avant d’effectuer la demande de résolution du nom (La recherche du nom, portera donc sur monserveur.mondomaine.com)

La deuxième ligne indique le serveur DNS principal.

Et c’est donc le serveur DNS qui sera chargé de donner le résultat s’il connaît la réponse ou de transmettre la question à un autre serveur DNS.

Si le serveur principal n’est pas disponible, le serveur DNS indiqué sur la ligne suivante sera utilisé.

Pourquoi installer un serveur DNS 

Pour au moins deux raisons :

Éviter de tenir à jour la table hosts de chaque poste client d’un réseau. 
Avoir un cache DNS qui accélère la recherche des noms. 
Sur un réseau locale, un serveur DNS permet d’accélérer le trafic sur le réseau car de nombreux services ont besoins d’un serveur DNS bien configuré pour fonctionner correctement (WEB, POP, SMTP,..)

Sous Debian, il faut installer le paquet suivant :
```
apt-get install bind9
```
Fichier de Configuration Principal (/etc/bind/named.conf)
```
key "rndc-key" { 
       algorithm hmac-md5; 
       secret "kKf6n5FNQaRGBM4NSUROUA=="; 
}; 

controls { 
       inet 127.0.0.1 port 953 
               allow { 127.0.0.1; } keys { "rndc-key"; }; 
}; 

zone "." { 
	type hint; 
	file "/etc/bind/db.root"; 
}; 

zone "localhost" { 
	type master; 
	file "/etc/bind/db.local"; 
}; 

zone "127.in-addr.arpa" { 
	type master; 
	file "/etc/bind/db.127"; 
}; 

zone "0.in-addr.arpa" { 
	type master; 
	file "/etc/bind/db.0"; 
}; 

zone "255.in-addr.arpa" { 
	type master; 
	file "/etc/bind/db.255"; 
}; 

// add entries for other zones below here 

include "/etc/bind/named.conf.local"; 
include "/etc/bind/named.conf.options";
```
Fichier de Configuration (/etc/bind/named.conf.local)
```
// 
// Do any local configuration here 
// 

// Consider adding the 1918 zones here, if they are not used in your 
// organization 
//include "/etc/bind/zones.rfc1918"; 

zone "cyberlitech.lan" { 
	type master; 
	notify no ; 
	file "/etc/bind/db.cyberlitech.lan"; 
// allow-update { key rndc-key; }; 
allow-update { 192.168.0.253; }; 
}; 

zone "0.168.192.in-addr.arpa" { 
	type master; 
	notify no ; 
	file "/etc/bind/db.cyberlitech.lan.reversed"; 
// allow-update { key rndc-key; }; 
allow-update { 192.168.0.253; }; 
}; 

// Anti Spam Orange 

zone "orange.fr" { 
	type forward; 
	forward only; 
	forwarders { 
	80.10.246.3; 
	81.253.149.1; 
	}; 
}; 

zone "246.10.80.in-addr.arpa" { 
	type forward; 
	forward only; 
	forwarders { 
	80.10.246.3; 
	81.253.149.1; 
	}; 
}; 
```
Fichier de Configuration (/etc/bind/named.conf.options)
```
options { 
	directory "/var/cache/bind"; 

	// If there is a firewall between you and nameservers you want 
	// to talk to, you might need to uncomment the query-source 
	// directive below.  Previous versions of BIND always asked 
	// questions using port 53, but BIND 8.1 and later use an unprivileged 
	// port by default. 

	// query-source address * port 53; 

	// If your ISP provided one or more IP addresses for stable 
	// nameservers, you probably want to use them as forwarders.  
	// Uncomment the following block, and insert the addresses replacing 
	// the all-0's placeholder. 

	// forwarders { 
	// 	0.0.0.0; 
	// }; 

	forwarders { 
	 	80.10.246.3; 
		81.253.149.1; 
	 }; 

	auth-nxdomain no;    # conform to RFC1035 
	listen-on-v6 { any; }; 
};
```
Fichier de Configuration (/etc/bind/db.cyberlitech.lan)
```
@	IN	SOA	srv-linux-01.cyberlitech.lan. postmaster.srv-linux-01.cyberlitech.lan. ( 
			1999112002 ; numéro de série 
			28800 ; rafraichissement 
			14400 ; nouvel essais 
			604800 ; expiration 
			86400 ) ; temps de vie minimum 
@	IN 	NS	srv-linux-01 
@	IN	NS	srv-linux-01.cyberlitech.lan. 
@	IN	MX	10 srv-linux-01 
@	IN	MX	20 srv-linux-01.cyberlitech.lan.	 

; serveurs de noms en local 
@	IN A	127.0.0.1		; localhost 
@	IN A	192.168.0.253	; serveur principal 

; adresses IP des serveurs 
locahost		IN A	127.0.0.1 
srv-linux-01		IN A	192.168.0.253	 

; adresses IP des machines du reseau 
PC-01		IN A	192.168.0.101 
PC-02		IN A	192.168.0.102 
PC-03		IN A	192.168.0.103 
PC-04		IN A	192.168.0.104 
PC-05		IN A	192.168.0.105 

; aliases 
www 		IN CNAME 	srv-linux-01.cyberlitech.lan. 
ssh 		IN CNAME 	srv-linux-01.cyberlitech.lan. 
pop 		IN CNAME 	srv-linux-01.cyberlitech.lan. 
smtp 		IN CNAME 	srv-linux-01.cyberlitech.lan. 
smtps 	        IN CNAME 	srv-linux-01.cyberlitech.lan. 
pops 		IN CNAME 	srv-linux-01.cyberlitech.lan. 
imap 		IN CNAME 	srv-linux-01.cyberlitech.lan. 
ftp 		IN CNAME 	srv-linux-01.cyberlitech.lan. 
webmail 	IN CNAME 	srv-linux-01.cyberlitech.lan. 
web 		IN CNAME 	srv-linux-01.cyberlitech.lan. 
stats 	        IN CNAME 	srv-linux-01.cyberlitech.lan. 
```
Fichier de Configuration (/etc/bind/db.cyberlitech.lan.reversed)
```
@	IN	SOA	srv-linux-01.cyberlitech.lan. postmaster.srv-linux-01.cyberlitech.lan. ( 
			1999112002 ; numéro de série 
			28800 ; rafraichissement 
			14400 ; nouvel essais 
			604800 ; expiration 
			86400 ) ; temps de vie minimum	 
; serveurs de noms 
	IN	NS	srv-linux-01.cyberlitech.lan. 

; adresses IP inverses 
253	IN PTR	srv-linux-01.cyberlitech.lan.	 
101	IN PTR	PC-01.cyberlitech.lan. 
102	IN PTR	PC-02.cyberlitech.lan. 
103	IN PTR	PC-03.cyberlitech.lan. 
104	IN PTR	PC-04.cyberlitech.lan. 
105	IN PTR	PC-05.cyberlitech.lan.
```
Fichier de Configuration (/etc/bind/rndc.conf)
```
# Start of rndc.conf 
key "rndc-key" { 
	algorithm hmac-md5; 
	secret "kKf6n5FNQaRGBM4NSUROUA=="; 
}; 

options { 
	default-key "rndc-key"; 
	default-server 127.0.0.1; 
	default-port 953; 
}; 

# End of rndc.conf 

# Use with the following in named.conf, adjusting the allow list as needed: 
# key "rndc-key" { 
# 	algorithm hmac-md5; 
# 	secret "kKf6n5FNQaRGBM4NSUROUA=="; 
# }; 
# 
# controls { 
# 	inet 127.0.0.1 port 953 
# 		allow { 127.0.0.1; } keys { "rndc-key"; }; 
# }; 
# End of named.conf
```
Fichier de Configuration (/etc/bind/rndc.key)
```
key "rndc-key" { 
	algorithm hmac-md5; 
	secret "kKf6n5FNQaRGBM4NSUROUA=="; 
};
```
Fichier de Configuration (/etc/bind/db.root)

```
.                        3600000  IN  NS    A.ROOT-SERVERS.NET. 
A.ROOT-SERVERS.NET.      3600000      A     198.41.0.4 
.                        3600000      NS    B.ROOT-SERVERS.NET. 
B.ROOT-SERVERS.NET.      3600000      A     128.9.0.107 
.                        3600000      NS    C.ROOT-SERVERS.NET. 
C.ROOT-SERVERS.NET.      3600000      A     192.33.4.12 
.                        3600000      NS    D.ROOT-SERVERS.NET. 
D.ROOT-SERVERS.NET.      3600000      A     128.8.10.90 
.                        3600000      NS    E.ROOT-SERVERS.NET. 
E.ROOT-SERVERS.NET.      3600000      A     192.203.230.10 
.                        3600000      NS    F.ROOT-SERVERS.NET. 
F.ROOT-SERVERS.NET.      3600000      A     192.5.5.241 
.                        3600000      NS    G.ROOT-SERVERS.NET. 
G.ROOT-SERVERS.NET.      3600000      A     192.112.36.4 
.                        3600000      NS    H.ROOT-SERVERS.NET. 
H.ROOT-SERVERS.NET.      3600000      A     128.63.2.53 
.                        3600000      NS    I.ROOT-SERVERS.NET. 
I.ROOT-SERVERS.NET.      3600000      A     192.36.148.17 
.                        3600000      NS    J.ROOT-SERVERS.NET. 
J.ROOT-SERVERS.NET.      3600000      A     198.41.0.10 
.                        3600000      NS    K.ROOT-SERVERS.NET. 
K.ROOT-SERVERS.NET.      3600000      A     193.0.14.129 
.                        3600000      NS    L.ROOT-SERVERS.NET. 
L.ROOT-SERVERS.NET.      3600000      A     198.32.64.12 
.                        3600000      NS    M.ROOT-SERVERS.NET. 
M.ROOT-SERVERS.NET.      3600000      A     202.12.27.33
```
