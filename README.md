<div align="center">

  <br></br>
  
  <a href="https://github.com/0xCyberLiTech">
    <img src="https://readme-typing-svg.herokuapp.com?font=JetBrains+Mono&size=50&duration=6000&pause=1000000000&color=FF0048&center=true&vCenter=true&width=1100&lines=%3EDNS_" alt="Titre dynamique DNS" />
  </a>
  
  <br></br>
  
  <h2>Laboratoire numérique pour la cybersécurité, Linux & IT</h2>


  <p align="center">
    <a href="https://0xcyberlitech.github.io/">
      <img src="https://img.shields.io/badge/Portfolio-0xCyberLiTech-181717?logo=github&style=flat-square" alt="Portfolio" />
    </a>
    <a href="https://github.com/0xCyberLiTech">
      <img src="https://img.shields.io/badge/Profil-GitHub-181717?logo=github&style=flat-square" alt="Profil GitHub" />
    </a>
    <a href="https://github.com/0xCyberLiTech/DNS/releases/latest">
      <img src="https://img.shields.io/github/v/release/0xCyberLiTech/DNS?label=version" alt="Dernière version" />
    </a>
    <a href="https://github.com/0xCyberLiTech/DNS/blob/main/CHANGELOG.md">
      <img src="https://img.shields.io/badge/📄%20CHANGELOG-DNS-blue" alt="Changelog" />
    </a>
    <a href="https://github.com/0xCyberLiTech?tab=repositories">
      <img src="https://img.shields.io/badge/Dépôts-publics-blue?style=flat-square" alt="Dépôts publics" />
    </a>
    <a href="https://github.com/0xCyberLiTech/DNS/graphs/contributors">
      <img src="https://img.shields.io/badge/👥%20Contributeurs-cliquez%20ici-007ec6?style=flat-square" alt="Contributeurs" />
    </a>
  </p>

</div>

<!--
Optimisation SEO : mots-clés cybersécurité, Linux, administration système, sécurité informatique, tutoriels, guides, expertise, formation, supervision, Docker, OpenVAS, firewall, proxy, DNS, SSH, Debian, IT, réseau, cryptographie, open source, ressources techniques, étudiants, professionnels, passionnés.
-->

<div align="center">
  <img src="https://img.icons8.com/fluency/96/000000/cyber-security.png" alt="CyberSec" width="80"/>
</div>

<div align="center">
  <p>
    <strong>Cybersécurité</strong> <img src="https://img.icons8.com/color/24/000000/lock--v1.png"/> • <strong>Linux Debian</strong> <img src="https://img.icons8.com/color/24/000000/linux.png"/> • <strong>Sécurité informatique</strong> <img src="https://img.icons8.com/color/24/000000/shield-security.png"/>
  </p>
</div>

---

## 🚀 À propos & Objectifs

Ce projet propose des solutions innovantes et accessibles en cybersécurité, avec une approche centrée sur la simplicité d’utilisation et l’efficacité. Il vise à accompagner les utilisateurs dans la protection de leurs données et systèmes, tout en favorisant l’apprentissage et le partage des connaissances.

Le contenu est structuré, accessible et optimisé SEO pour répondre aux besoins de :
- 🎓 Étudiants : approfondir les connaissances
- 👨‍💻 Professionnels IT : outils et pratiques
- 🖥️ Administrateurs système : sécuriser l’infrastructure
- 🛡️ Experts cybersécurité : ressources techniques
- 🚀 Passionnés du numérique : explorer les bonnes pratiques

---

<div align="center" style="margin-bottom: 10px;">

### 🚀 **Sommaire :**

🟢 **Actif** – Dépôt totalement accessible  
🟠 **Partiel** – Dépôt partiellement accessible  
🔴 **Inactif** – Dépôt inaccessible ou indisponible

</div>

---

<div align="center">

**Catégories des projets :**

| Chapitre | Description | Accès Rapide |
|:---:|:---|:---:|
| **01** | Qu'est-ce que le DNS ? | [<img src="https://img.shields.io/badge/EXPLORER-brightgreen?style=for-the-badge&logo=github&logoColor=white">](#balise_01) |
| **02** | Comment fonctionne le DNS ? | [<img src="https://img.shields.io/badge/EXPLORER-brightgreen?style=for-the-badge&logo=github&logoColor=white">](#balise_02) |
| **03** | Les 4 types de serveurs DNS impliqués dans une requête. | [<img src="https://img.shields.io/badge/EXPLORER-brightgreen?style=for-the-badge&logo=github&logoColor=white">](#balise_03) |
| **04** | Différence entre DNS de référence et résolveur récursif. | [<img src="https://img.shields.io/badge/EXPLORER-brightgreen?style=for-the-badge&logo=github&logoColor=white">](#balise_04) |
| **05** | Exemple de configuration d'un serveur DNS Maître. | [<img src="https://img.shields.io/badge/EXPLORER-brightgreen?style=for-the-badge&logo=github&logoColor=white">](Exemple_server_DNS_maître.md) |
| **06** | Exemple de configuration d'un serveur DNS Esclave. | [<img src="https://img.shields.io/badge/EXPLORER-brightgreen?style=for-the-badge&logo=github&logoColor=white">](Exemple_server_DNS_esclave.md) |

</div>

<a name="balise_01"></a>
## Qu'est-ce que le DNS ?

Le DNS (Domain Name System, système de nom de domaine) est en quelque sorte le répertoire téléphonique d'Internet. Les internautes accèdent aux informations en ligne via des noms de domaine (par exemple, nytimes.com ou espn.com), tandis que les navigateurs interagissent par le biais d'adresses IP (Internet Protocol, protocole Internet). Le DNS traduit les noms de domaine en adresses IP afin que les navigateurs puissent charger les ressources web.

Chaque appareil connecté à Internet dispose d'une adresse IP unique que les autres appareils utilisent afin de le trouver. Grâce aux serveurs DNS, les internautes n'ont pas à mémoriser les adresses IP (par exemple, 192.168.1.1 en IPv4) ni les adresses IP alphanumériques plus récentes et plus complexes (par exemple, 2410:cb02:2048:1::c455:c7v5 en IPv6).

<a name="balise_02"></a>
## Comment fonctionne le DNS ?

Le processus de résolution DNS implique la conversion d'un nom d'hôte (par exemple, www.exemple.com) en adresse IP « au format informatique » (par exemple, 192.168.1.1). Chaque appareil relié à Internet se voit attribuer une telle adresse. Cette dernière permet de trouver l'appareil approprié sur Internet, de la même manière qu'une adresse dans une rue permet de trouver un domicile. Lorsqu'un utilisateur souhaite charger une page web, une traduction doit intervenir entre l'adresse que l'utilisateur saisit dans son navigateur (exemple.com) et l'adresse utilisable par la machine, nécessaire pour localiser la page web exemple.com.

Afin de comprendre le processus à l'œuvre derrière la résolution DNS, il est important de connaitre les différents composants physiques par lesquels une requête DNS doit passer. Du point de vue du navigateur, la recherche DNS s'effectue « en arrière-plan » et ne nécessite aucune interaction de l'ordinateur de l'utilisateur en dehors de la demande initiale.

<a name="balise_03"></a>
## Quatre serveurs DNS sont impliqués dans le chargement d'une page web.

- Récurseur DNS : le récurseur peut être considéré comme un bibliothécaire à qui l'on demande d'aller chercher un livre particulier quelque part dans une bibliothèque. Il s'agit d'un serveur conçu pour recevoir les requêtes des ordinateurs client par l'intermédiaire d'applications, telles que les navigateurs web. Généralement, le récurseur se charge ensuite d'effectuer les demandes supplémentaires nécessaires à la résolution de la requête DNS du client.

- Serveur de noms racine : le serveur racine constitue la première étape de la traduction (résolution) des noms d'hôtes lisibles par l'humain en adresses IP. Il s'agit en quelque sorte du catalogue d'une bibliothèque : il renvoie vers les différents rayonnages de livres et sert généralement de référence pour trouver d'autres emplacements plus spécifiques.

- Serveur de noms TLD : le serveur de domaine de premier niveau (TLD, top level domain) peut être considéré comme un rayonnage spécifique au sein d'une bibliothèque. Étape suivante dans la recherche d'une adresse IP spécifique, ce serveur de noms héberge la dernière partie d'un nom d'hôte (dans « exemple.com », le serveur TLD est ainsi « .com »).

- Serveur de noms de référence : ce dernier serveur de noms peut être considéré comme un dictionnaire situé sur un rayonnage. Il rend possible la traduction d'un nom spécifique sous forme d'une définition. Le serveur de noms de référence constitue la dernière étape d'une requête effectuée au serveur de noms. Si le serveur de noms de référence a accès à l'enregistrement demandé, il renvoie l'adresse IP du nom d'hôte demandé au récurseur DNS (le bibliothécaire) qui a effectué la requête initiale.

<a name="balise_04"></a>
## Quelle est la différence entre un serveur DNS de référence et un résolveur DNS récursif ?

Si les deux concepts font référence à des serveurs (groupe de serveurs) faisant partie intégrante de l'infrastructure DNS, ils présentent chacun un rôle différent et ne se situent pas au même emplacement dans le pipeline d'une requête DNS. Pour comprendre la différence entre les deux, il suffit de se souvenir que le résolveur récursif se trouve au début de la requête DNS et le serveur de noms de référence à la fin.

- Résolveur DNS récursif :

Le résolveur récursif désigne l'ordinateur qui répond à la requête récursive d'un client et prend le temps de suivre l'enregistrement DNS. Il lance pour cela une série de requêtes jusqu'à atteindre le serveur de noms DNS de référence pour l'enregistrement demandé (la requête expire ou renvoie une erreur si aucun enregistrement n'est trouvé). Heureusement, les résolveurs DNS récursifs n'ont pas toujours à effectuer plusieurs requêtes pour rechercher les enregistrements permettant de répondre à un client. En tant que processus de persistance des données, la mise en cache permet de court-circuiter les requêtes nécessaires en fournissant l'enregistrement de la ressource demandée plus tôt au sein de la recherche DNS.

![dns_record_request_sequence_recursive_resolver.png](./images/dns_record_request_sequence_recursive_resolver.png)

- Serveur DNS de référence :

En clair, le terme serveur DNS de référence désigne un serveur détenant réellement les enregistrements de ressources DNS et responsable de ces derniers. Situé tout en bas de la chaîne de recherche DNS, ce serveur répond en renvoyant l'enregistrement de la ressource recherchée. Ce faisant, il permet finalement au navigateur web effectuant la requête d'atteindre l'adresse IP nécessaire pour accéder à un site web ou à d'autres ressources web. Un serveur de noms de référence peut satisfaire les requêtes à partir de ses propres données sans avoir à interroger une autre source, car il s'agit de la source unique de vérité pour certains enregistrements DNS.

![dns_record_request_sequence_authoritative_nameserver.png](./images/dns_record_request_sequence_authoritative_nameserver.png)

Source : https://www.cloudflare.com/fr-fr/learning/dns/what-is-dns/

---

<div align="center">
  <a href="https://github.com/0xCyberLiTech" target="_blank" rel="noopener">
    <img src="https://skillicons.dev/icons?i=linux,debian,bash,docker,nginx,git,vim,python,markdown" alt="Skills" width="440">
  </a>
</div>

<div align="center">
  <b>🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour des tutoriels accessibles à tous. 🔒</b>
</div>
