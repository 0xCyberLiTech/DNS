# Installer un serveur de DNS sous Debian 11 & Debian 12.

Nous utiliserons la brique Bind9. Mon objectif est de référencer quelques serveurs avec des enregistrements de type A, et de spécifier un redirecteur.

On va voir ici comment mettre cela en place de la façon la plus simple qui soit. La base de la base, sans prise de tête. Mais la base c’est important ! En commençant par la, vous pourrez ensuite implémenter les divers paramètres dont vous aurez besoin plus tard.

- Je suis sur une Debian 12, installation minimale sens interface graphyque.
- J’ai 6 hôtes sur mon réseau.
- Je n’ai pas de contrôleur de domaine, je souhaite juste référencer mes hôtes et leurs IP.
- Je n’ai pas non plus de serveur mail.
- Je souhaite que mon DNS redirige les requêtes qu’il ne sait pas résoudre.
- Mon domaine sera cyberlitech.lan

Configurer le réseau en statique :

Configurez d’abord l’interface réseau de votre serveur en statique.

Le nom d’hôte de mon serveur est NS1.
L’IP de mon serveur est 192.168.50.200/24

## Installer Bind9
```
apt-get install bind9
```
Les fichiers de conf :

named.conf

Par défaut il ne contient que des includes vers les fichiers suivants :

/etc/bind/named.conf.options          # Le fichier de configuration.
/etc/bind/named.conf.local            # Le fichier avec nos zones.
/etc/bind/named.conf.default-zones    # Le fichier avec les zones par défaut.

On va donc configurer Bind dans le fichier named.conf.options :

named.conf.options
Exemple, avec quelques annotations. A vous d’adapter le vôtre :
...
options {
      directory « /var/cache/bind »;

      forwarders { 8.8.8.8; };                           # Entrez l’IP d’un serveur DNS public qui saura résoudre ce qui n’appartient pas à votre réseau. Ici il s’agit d’un DNS de Google.
      auth-nxdomain no;                                  # Le serveur n’enregistrera pas les réponses négatives (« je ne sais pas résoudre cela ») dans son cache.*
      listen-on-v6 { none; };                            # Si votre serveur n’a pas de carte réseau en IPv6, mettez cette directive.
      listen-on { 127.0.0.1 ; 192.168.50.200 ;};         # Entrez ici les IP des interfaces réseau du serveur DNS qui peuvent recevoir des requête DNS.
      allow-transfer {none;};                            # Je n’ai pas de DNS secondaire donc pas de transfert de zones à activer.
      allow-query { 192.168.50.0/24; localhost; };       # autoriser les requêtes provenant seulement du réseau spécifié et du loopback.
      allow-recursion { 192.168.50.0/24; localhost; };   # Même principe mais pour les recherches récursives**.
      version none;                                      # Ne pas diffuser la version de Bind.
}
```
Enregistrez le fichier.

*NXDOMAIN est un message que le DNS vous renvoie s’il ne sait pas résoudre une requête. Par exemple, vous lui demandez quel est l’IP de cyberlitech.fr, votre serveur cherche et ne trouve nulle part, il va vous envoyer l’erreur NXDOMAIN, et si l’option est active, enregistrer cette erreur dans son cache. Cela implique que si cyberlitech.fr devient trouvable plus tard, votre DNS vous renverra quand même NXDOMAIN sans chercher plus loin puisque c’est la réponse qui se trouve dans son cache ! Donc c’est nul, on le désactive ^^

**Les requêtes récursives demandent à votre DNS de se renseigner auprès des autres serveurs DNS s’il ne sait pas résoudre votre requête. C’est votre serveur DNS qui vous enverra la résolution, ou une erreur s’il ne parvient pas à trouver votre réponse. Votre DNS va chercher une réponse récursivement dans sa hiérarchie de DNS, puis vous répondre.

La requête récursive vient en opposition à la requête itérative qui fonctionne différemment. Si votre DNS ne contient pas la résolution demandée, il vous envoie l’IP du DNS supérieur et c’est « vous » qui questionnez le DNS supérieur.

Le mode récursif semble plus intéressant car votre DNS enregistrera à mesure ses nouvelles connaissances dans son cache. Ce ne sera pas le cas en itératif puisque en fin de compte quand vous obtenez votre réponse ce n’est plus avec votre DNS que vous êtes en train de communiquer. On autorise donc les recherches récursives ici.

named.conf.local
On va maintenant créer la zone associée au domaine, ainsi que la zone de recherche inversée. 

Voici mon fichier named.conf.local, adaptez à votre sauce :
```
zone « cyberlitech.lan » {                        # Entrez votre nom de domaine. 
      type master;                                # Le serveur a autorité sur ce domaine, même s’il n’y a pas de slave dans cet exemple.
      file « /var/cache/bind/db.cyberlitech.lan« ;  # On défini le fichier de zone (qui contiendra les enregistrements).
      forwarders {};                              # On ne redirige pas les requêtes concernant le domaine cyberlitech.lan.
      allow-update {none;};                       # Dans cet exemple on ne souhaite pas que le DNS soit mis à jour dynamiquement. Mais si vous avez un DHCP ou autre renseignez-vous là-dessus.
};
```
# Voila pour la zone principale. En dessous on met la zone inversée. Si vous ne comprenez pas ce que c’est, je vous invite à commencer plutôt par un cours sur DNS :
```
zone « 50.168.192.in-addr.arpa » {
      type master;
      file « /var/cache/bind/db.cyberlitech.lan.inv » ;
      forwarders{};
      allow-update{none;};
};
```
Enregistrez le fichier.

On va ensuite configurer nos zones de recherche :

Création des fichiers de zones.

Les fichiers auxquels se rapporte named.conf.local n’existent pas. Il faut les créer et on va commencer à inscrire des enregistrement DNS. 

Fichier de résolution de noms db.cyberlitech.lan
```
nano /var/cache/bind/db.cyberlitech.lan
```
Pour info la casse n’est pas prise en compte sur les noms d’hôtes.

```
$TTL    3600 
@      IN     SOA    dns01.cyberlitech.lan   root.cyberlitech.lan   (  
2021113001   
3600  
600
86400
600 )

;

@              IN     NS     dns01

@              IN     MX     1     mail01      # Juste pour l’exemple si présence de serveur mail sur le domaine.
ns1            IN     A      192.168.50.200
srv-linux-02   IN     A      192.168.50.201
srv-linux-03   IN     A      192.168.50.202
srv-linux-04   IN     A      192.168.50.203
srv-linux-05   IN     A      192.168.50.204
srv-linux-06   IN     A      192.168.50.205

mail01         IN     A    192.168.50.0        # Juste pour l’exemple si présence de serveur mail sur le domaine.

dns     IN     CNAME    ns1
```
Les nombres entre parenthèses sont des directives de délais concernant les rafraichissements des DNS esclaves. Dans cet exemple ça ne nous intéresse pas puisque nous n’avons qu’un seul DNS, mais c’est important à savoir). Le TTL indique combien de temps doit s’écouler entre les mises à jour d’infos entre serveurs DNS. Notre serveur sera vérifié toutes les heures (3600 secondes). Encore une fois ce n’est pas important pour notre exemple.

L’enregistrement NS indique le DNS qui fait autorité.

L’enregistrement MX pointe un serveur mail. Je n’en ai pas mais je le mets pour l’exemple. On peut en mettre plusieurs et #définir une priorité en le précédent d’un chiffre (ici 1 qui est la plus #haute priorité).

La valeur IN signifie Internet. Il en existe d’autres mais c’est assez spécifique j’ai l’impression. Nous on laissera IN.

L’enregistrement CNAME permet de créer un alias. Un nom qui pointe vers un autre nom.

Et enfin les enregistrements de type A pour référencer les hôtes de mon réseau.
 
Enregistrez le fichier.

Fichier de résolution inverse db.cyberlitech.lan.inv
Et enfin on créer le fichier de résolution inversée.
```
@      IN     SOA    dns01.cyberlitech.lan.   root.cyberlitech.lan.   (  
2021113001   
3600  
600
86400
600 )
;

       IN     NS     ns1.cyberlitech.lan.

2     IN     PTR     srv-linux-01.cyberlitech.lan.
3     IN     PTR     srv-linux-02.cyberlitech.lan.
4     IN     PTR     srv-linux-03.cyberlitech.lan.
5     IN     PTR     srv-linux-04.cyberlitech.lan.
6     IN     PTR     srv-linux-05.cyberlitech.lan.
7     IN     PTR     ns1.cyberlitech.lan.
```

Le chiffre qui se trouve en début de ligne est le dernier bloc de l’IP de chaque hôte.
N’oubliez pas de mettre un point à la fin de chaque nom de domaine !
Enregistrez le fichier.

Redémarrer le service et le tester.

On va redémarrer le service, s’assurer qu’il se lance au démarrage de Debian, et tester les fonctions du DNS en local.

Vous pouvez également tester la bonne fonction du DNS depuis une autre machine du réseau. Pour cela pensez à déclarer l’IP du serveur DNS dans ses paramètres réseau.

Le service associé à Bind, sous Debian en tous cas, se nomme named.

->Vous avez envie que le service démarre en même tant que l’OS :
```
systemctl enable named
```
-> Après toutes nos bidouilles, il faut redémarrer le service :
```
systemctl restart named
```
Si vous avez une erreur de démarrage c’est que vous avez fait une erreur de syntaxe dans un de vos fichiers.
->Testez la zone de recherche directe :
```
host -t A srv-linux-02
```
Doit vous afficher l’IP de l’hôte srv-linux-02.
->Testez la zone de recherche inversée :
```
host -t ptr 172.16.0.3
```
Doit vous afficher le nom associé à l’ip 172.16.0.3.
En théorie, si vous ne vous êtes pas planté sur un point ou une accolade, vous avez maintenant un serveur DNS basique fonctionnel. A vous d’y implémenter les options nécessaire le cas échéant.
