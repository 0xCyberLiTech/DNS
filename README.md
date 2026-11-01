<!-- MASTER-HEADER-START -->
<div align="center">

<a href="https://0xcyberlitech.com/">
  <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/headers/header-dns.svg" alt="ATELIER DNS // RÉSOLUTION & BIND9" width="100%" />
</a>

<p align="center">
  <a href="https://0xcyberlitech.com/"><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-portal.svg" alt="Portail 0xCyberLiTech.com" width="24%" /></a>
  <a href="https://0xcyberlitech.com/soc-rd.html"><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-soc-rd.svg" alt="Livre Blanc SOC Cockpit" width="24%" /></a>
  <a href="https://0xcyberlitech.com/jarvis-rd.html"><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-jarvis-rd.svg" alt="Livre Blanc JARVIS IA" width="24%" /></a>
  <a href="https://github.com/0xCyberLiTech?tab=repositories"><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-github.svg" alt="Catalogue des 23 Dépôts" width="24%" /></a>
</p>
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/divider-hud.svg" alt="" width="100%" />
</div>
<!-- MASTER-HEADER-END -->

<!-- DEBIAN-TRUTH-START -->
> [!IMPORTANT]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-shield.svg" width="18" height="18" align="absmiddle" alt="" /> ** Matrice Officielle Debian — `Debian 12 (Bookworm)` · `Debian 13 (Trixie)` · `Debian 14 (Forky)`**
> - **Sources Officielles Debian.org (`bind9`) :** `Bookworm (12)` : **`1:9.18.49-1~deb12u1`** &nbsp;·&nbsp; `Trixie (13 — Stable)` : **`1:9.20.26-1~deb13u1`** &nbsp;·&nbsp; `Forky (14 — Next-Gen)` : **`1:9.20.30-1`**
> - **Références Officielles :** [Paquet Debian Trixie (`bind9`)](https://packages.debian.org/trixie/bind9) &nbsp;·&nbsp; [Paquet Debian Forky (`bind9`)](https://packages.debian.org/forky/bind9) &nbsp;·&nbsp; [Documentation Officielle Debian](https://wiki.debian.org/Bind9)
> - **Note d'Ingénierie (Divergence 13 vs 14) :** Sous Debian 13 (Trixie) et Debian 14 (Forky), le démon BIND9 se pilote via l'unité systemd **`named.service`** (`systemctl restart named`) et les logs se lisent via `journalctl -u named`.

<details>
<summary><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-doc.svg" width="18" height="18" align="absmiddle" alt="" /> <b>Afficher les Encarts Ligne de Commande : Debian 13 (Trixie — Stable) vs Debian 14 (Forky — Next-Gen)</b></summary>

#### <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-ok.svg" width="42" height="16" align="absmiddle" alt="[OK]" /> Profil Production Stable — Commandes pour `Debian 13 (Trixie)`
```bash
# Debian 13 (Trixie — BIND 9.20+) : Verification de syntaxe & statut du service nomme 'named'
sudo named-checkconf && sudo systemctl restart named
dig @127.0.0.1 localhost +dnssec
```

#### <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-rd.svg" width="46" height="16" align="absmiddle" alt="[R&D]" /> Profil Anticipation Next-Gen — Commandes pour `Debian 14 (Forky)`
```bash
# Debian 14 (Forky) — Unité systemd 'named.service' (l'alias bind9.service est deprecie) :
sudo systemctl status named.service
sudo journalctl -u named.service -f --no-hostname
```

</details>
<!-- DEBIAN-TRUTH-END -->

<!-- CIEL-PEDAGOGY-START -->
> [!TIP]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-target.svg" width="18" height="18" align="absmiddle" alt="" /> **Guide de Lecture &amp; Progression par Niveau (DNS)**
> - **Rôle de la brique :** Le service **DNS (BIND9 / Unbound)** assure la résolution bidirectionnelle entre les noms de domaine pleinement qualifiés (**FQDN**) et les adresses IP (`A` en IPv4, `AAAA` en IPv6, `PTR` en résolution inverse), constituant le socle critique de tout réseau d'entreprise.
> - **Parcours recommandé :** **Niveau 1 (Fondamentaux &amp; Installation initiale)** pour déployer une base saine &rarr; **Niveau 2 (Configuration &amp; Intégration)** &rarr; **Niveau 3 (Haute Disponibilité, Durcissement &amp; Exploitation)**.

<details>
<summary><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-puzzle.svg" width="18" height="18" align="absmiddle" alt="" /> <b>Synthèse d'Architecture, Standards Officiels &amp; Diagnostic (Niveau 1 : Fondamentaux · Niveau 2 : Repères · Niveau 3 : Diagnostic)</b></summary>

- **Niveau 1 — Fondamentaux &amp; Principe d'Architecture :** Le service **DNS (BIND9 / Unbound)** assure la résolution bidirectionnelle entre les noms de domaine pleinement qualifiés (**FQDN**) et les adresses IP (`A` en IPv4, `AAAA` en IPv6, `PTR` en résolution inverse), constituant le socle critique de tout réseau d'entreprise.
- **Niveau 2 — Repères Officiels (Ports IANA, Fichiers Debian &amp; Commandes de Validation) :** **Ports IANA :** `UDP 53` (Résolution standard) & `TCP 53` (Transferts de zone AXFR/IXFR & DNSSEC) · `TCP 853` (DNS-over-TLS) &nbsp;|&nbsp; **Fichiers Debian :** `/etc/bind/named.conf.options`, `/etc/bind/named.conf.local` &nbsp;|&nbsp; **Validation officielle :** `sudo named-checkconf && sudo named-checkzone <zone> <fichier>`.
- **Niveau 3 — Diagnostic d'Incidents &amp; Ingénierie Avancée :** **Diagnostic & Architecture :** Toute modification d'un fichier de zone impose d'incrémenter le numéro **`Serial`** de l'enregistrement `SOA` avant d'exécuter `sudo rndc reload`. En architecture sécurisée, séparez toujours le **résolveur récursif** (interne) du **serveur faisant autorité** (exposé) pour prévenir l'amplification DNS.
- **Sources de Vérité Officielles Éditeurs &amp; Standards :** [Documentation Officielle ISC BIND 9](https://bind9.readthedocs.io/) &nbsp;·&nbsp; [RFC 1035 (IETF DNS)](https://datatracker.ietf.org/doc/html/rfc1035) &nbsp;·&nbsp; [Wiki Officiel Debian BIND9](https://wiki.debian.org/Bind9)
- **Livres Blancs &amp; 67 Podcasts Audio R&amp;D :** [Portail 0xCyberLiTech](https://0xcyberlitech.com/) &nbsp;·&nbsp; [Livre Blanc SOC Cockpit v5.5](https://0xcyberlitech.com/soc-rd.html) &nbsp;·&nbsp; [Livre Blanc JARVIS Mark-VII](https://0xcyberlitech.com/jarvis-rd.html) &nbsp;·&nbsp; [Blueprint Logiciel &amp; Matériel RTX 5080](https://0xcyberlitech.com/blueprint-jarvis.html)

</details>
<!-- CIEL-PEDAGOGY-END -->


## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 01 // SOMMAIRE DES DOSSIERS TECHNIQUES — ATELIER DNS // RÉSOLUTION & BIND9
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

<div align="center">

  | Chapitre | Description                                                           | Accès Rapide                                                                                                                                                                               |
  |----------|-----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  | **01**   | Qu'est-ce que le DNS ?                                                | [<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-explore.svg" alt="Ouvrir le dossier" />](#balise_01)                                                                     |
  | **02**   | Comment fonctionne le DNS ?                                           | [<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-explore.svg" alt="Ouvrir le dossier" />](#balise_02)                                                                     |
  | **03**   | Les 4 types de serveurs DNS impliqués dans une requête.               | [<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-explore.svg" alt="Ouvrir le dossier" />](#balise_03)                                                                     |
  | **04**   | Différence entre DNS de référence et résolveur récursif.              | [<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-explore.svg" alt="Ouvrir le dossier" />](#balise_04)                                                                     |
  | **05**   | Installation & configuration d'un serveur DNS Maître sous DEBIAN 12.  | [<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-explore.svg" alt="Ouvrir le dossier" />](Installation_&_configuration_d_un_serveur_DNS_Maître_BIND9_sous_Debian_12.md)    |
  | **05**   | Installation & configuration d'un serveur DNS Maître sous DEBIAN 13.  | [<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-explore.svg" alt="Ouvrir le dossier" />](Installation_&_configuration_d_un_serveur_DNS_Maître_BIND9_sous_Debian_13.md)    |
  | **07**   | Installation & configuration d'un serveur DNS Esclave sous DEBIAN 12. | [<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-explore.svg" alt="Ouvrir le dossier" />](Installation_&_configuration_d_un_serveur_DNS_Esclave_BIND9_sous_Debian_12.md)  |
  | **08**   | Installation & configuration d'un serveur DNS Esclave sous DEBIAN 13. | [<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-explore.svg" alt="Ouvrir le dossier" />](Installation_&_configuration_d_un_serveur_DNS_Esclave_BIND9_sous_Debian_13.md)  |

</div>

<a name="balise_01"></a>
## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 02 // Qu'est-ce que le DNS, à quoi sert-il ?
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

Le DNS (Domain Name System, système de nom de domaine) est en quelque sorte le répertoire téléphonique d'Internet. Les internautes accèdent aux informations en ligne via des noms de domaine (par exemple, nytimes.com ou espn.com), tandis que les navigateurs interagissent par le biais d'adresses IP (Internet Protocol, protocole Internet). Le DNS traduit les noms de domaine en adresses IP afin que les navigateurs puissent charger les ressources web.

Chaque appareil connecté à Internet dispose d'une adresse IP unique que les autres appareils utilisent afin de le trouver. Grâce aux serveurs DNS, les internautes n'ont pas à mémoriser les adresses IP (par exemple, 192.168.1.1 en IPv4) ni les adresses IP alphanumériques plus récentes et plus complexes (par exemple, 2410:cb02:2048:1::c455:c7v5 en IPv6).

<a name="balise_02"></a>
## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 03 // Comment fonctionne le DNS ?
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

Le processus de résolution DNS implique la conversion d'un nom d'hôte (par exemple, www.exemple.com) en adresse IP « au format informatique » (par exemple, 192.168.1.1). Chaque appareil relié à Internet se voit attribuer une telle adresse. Cette dernière permet de trouver l'appareil approprié sur Internet, de la même manière qu'une adresse dans une rue permet de trouver un domicile. Lorsqu'un utilisateur souhaite charger une page web, une traduction doit intervenir entre l'adresse que l'utilisateur saisit dans son navigateur (exemple.com) et l'adresse utilisable par la machine, nécessaire pour localiser la page web exemple.com.

Afin de comprendre le processus à l'œuvre derrière la résolution DNS, il est important de connaitre les différents composants physiques par lesquels une requête DNS doit passer. Du point de vue du navigateur, la recherche DNS s'effectue « en arrière-plan » et ne nécessite aucune interaction de l'ordinateur de l'utilisateur en dehors de la demande initiale.

<a name="balise_03"></a>
## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 04 // Quatre serveurs DNS sont impliqués dans le chargement d'une page web.
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

- Récurseur DNS : le récurseur peut être considéré comme un bibliothécaire à qui l'on demande d'aller chercher un livre particulier quelque part dans une bibliothèque. Il s'agit d'un serveur conçu pour recevoir les requêtes des ordinateurs client par l'intermédiaire d'applications, telles que les navigateurs web. Généralement, le récurseur se charge ensuite d'effectuer les demandes supplémentaires nécessaires à la résolution de la requête DNS du client.

- Serveur de noms racine : le serveur racine constitue la première étape de la traduction (résolution) des noms d'hôtes lisibles par l'humain en adresses IP. Il s'agit en quelque sorte du catalogue d'une bibliothèque : il renvoie vers les différents rayonnages de livres et sert généralement de référence pour trouver d'autres emplacements plus spécifiques.

- Serveur de noms TLD : le serveur de domaine de premier niveau (TLD, top level domain) peut être considéré comme un rayonnage spécifique au sein d'une bibliothèque. Étape suivante dans la recherche d'une adresse IP spécifique, ce serveur de noms héberge la dernière partie d'un nom d'hôte (dans « exemple.com », le serveur TLD est ainsi « .com »).

- Serveur de noms de référence : ce dernier serveur de noms peut être considéré comme un dictionnaire situé sur un rayonnage. Il rend possible la traduction d'un nom spécifique sous forme d'une définition. Le serveur de noms de référence constitue la dernière étape d'une requête effectuée au serveur de noms. Si le serveur de noms de référence a accès à l'enregistrement demandé, il renvoie l'adresse IP du nom d'hôte demandé au récurseur DNS (le bibliothécaire) qui a effectué la requête initiale.

<a name="balise_04"></a>
## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 05 // Quelle est la différence entre un serveur DNS de référence et un résolveur DNS récursif ?
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

Si les deux concepts font référence à des serveurs (groupe de serveurs) faisant partie intégrante de l'infrastructure DNS, ils présentent chacun un rôle différent et ne se situent pas au même emplacement dans le pipeline d'une requête DNS. Pour comprendre la différence entre les deux, il suffit de se souvenir que le résolveur récursif se trouve au début de la requête DNS et le serveur de noms de référence à la fin.

- Résolveur DNS récursif :

Le résolveur récursif désigne l'ordinateur qui répond à la requête récursive d'un client et prend le temps de suivre l'enregistrement DNS. Il lance pour cela une série de requêtes jusqu'à atteindre le serveur de noms DNS de référence pour l'enregistrement demandé (la requête expire ou renvoie une erreur si aucun enregistrement n'est trouvé). Heureusement, les résolveurs DNS récursifs n'ont pas toujours à effectuer plusieurs requêtes pour rechercher les enregistrements permettant de répondre à un client. En tant que processus de persistance des données, la mise en cache permet de court-circuiter les requêtes nécessaires en fournissant l'enregistrement de la ressource demandée plus tôt au sein de la recherche DNS.

![dns_record_request_sequence_recursive_resolver.png](./images/dns_record_request_sequence_recursive_resolver.png)

- Serveur DNS de référence :

En clair, le terme serveur DNS de référence désigne un serveur détenant réellement les enregistrements de ressources DNS et responsable de ces derniers. Situé tout en bas de la chaîne de recherche DNS, ce serveur répond en renvoyant l'enregistrement de la ressource recherchée. Ce faisant, il permet finalement au navigateur web effectuant la requête d'atteindre l'adresse IP nécessaire pour accéder à un site web ou à d'autres ressources web. Un serveur de noms de référence peut satisfaire les requêtes à partir de ses propres données sans avoir à interroger une autre source, car il s'agit de la source unique de vérité pour certains enregistrements DNS.

![dns_record_request_sequence_authoritative_nameserver.png](./images/dns_record_request_sequence_authoritative_nameserver.png)

Source : https://www.cloudflare.com/fr-fr/learning/dns/what-is-dns/

<!-- MASTER-FOOTER-START -->
<div align="center">
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/divider-hud.svg" alt="" width="100%" />

<a href="https://0xcyberlitech.com/">
  <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/footer-master.svg" alt="0xCyberLiTech — Architecture Cyberdéfense, Ingénierie Déterministe & IA Souveraine" width="100%" />
</a>

<p align="center"><a href="https://0xcyberlitech.com/"><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/badge-antigravity.svg" alt="Architecturé par Marc & Antigravity (Google DeepMind)" width="520" /></a></p>

<sub><b>Architecturé &amp; Forgé par Marc &amp; Antigravity (Google DeepMind)</b> &nbsp;·&nbsp; <a href="https://0xcyberlitech.com/"><b>0xcyberlitech.com</b></a> &nbsp;·&nbsp; <a href="https://0xcyberlitech.com/soc-rd.html">Architecture SOC Cockpit v5.5</a> &nbsp;·&nbsp; <a href="https://0xcyberlitech.com/jarvis-rd.html">Architecture JARVIS Mark-VII</a> &nbsp;·&nbsp; <a href="https://0xcyberlitech.com/blueprint-jarvis.html">Blueprint Matériel RTX 5080</a></sub>

</div>
<!-- MASTER-FOOTER-END -->
