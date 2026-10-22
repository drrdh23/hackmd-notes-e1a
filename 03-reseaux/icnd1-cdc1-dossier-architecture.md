# CDC  1 -Dossier d'architecture


**Client :** Léman Logistique SA — M. Favre, Directeur des opérations
**Ticket :** SAT-2026-0412 — Priorité haute
**Prestataire :** Satom IT & Learning Solutions
**Bloc interne imposé :** 192.168.10.0/24

---

## Présentation de l'infrastructure

![image](https://hackmd.io/_uploads/Byj0raocGe.png)

Léman Logistique possède trois sites sur l'arc lémanique :

- **Genève (siège)** : routeur **R1-GVA**. C'est le routeur central et le seul à avoir un accès à Internet (routeur de bordure, NAT/PAT).
- **Lausanne (entrepôt)** : routeur **R2-LSN**, avec le réseau LAN-LSN et le poste PC-LAUSANNE (VM Windows 10).
- **Nyon (entrepôt)** : routeur **R3-NYO**, avec le réseau LAN-NYO et le poste PC-NYON (VM Ubuntu LTS).

Les liaisons entre les équipements sont les suivantes :

- R1-GVA **Gi0/1** ↔ R2-LSN **Gi0/0** : liaison **WAN-1**
- R1-GVA **Gi0/2** ↔ R3-NYO **Gi0/0** : liaison **WAN-2**
- R1-GVA **Gi0/0** ↔ nœud NAT GNS3 : accès **Internet**
- R2-LSN **Gi0/1** ↔ LAN-LSN (via VMnet2) : passerelle de Lausanne
- R3-NYO **Gi0/1** ↔ LAN-NYO (via VMnet3) : passerelle de Nyon

L'infrastructure est maquettée dans GNS3 (routeurs Cisco IOSv). Les postes clients sont des VM VMware Workstation reliées à la maquette par des nœuds Cloud et les adaptateurs hôtes VMnet2 et VMnet3.

---

# PARTIE A — Volet théorique appliqué

## 1.1 Qualification LAN / WAN

### Rappel des notions

- Un **LAN** (Local Area Network) est un réseau local, limité à un seul lieu (un bâtiment, un entrepôt). Il est géré entièrement par l'entreprise et relie les postes de travail entre eux et à leur passerelle.
- Un **WAN** (Wide Area Network) est un réseau étendu, qui relie des sites éloignés géographiquement (plusieurs villes). Il passe en général par l'infrastructure d'un opérateur, que l'entreprise loue sans la posséder. Internet est lui aussi un WAN.

### Application à Léman Logistique

| Élément du schéma | Type | Justification |
|---|---|---|
| LAN-LSN (Lausanne) — 192.168.10.0/26 | **LAN** | Réseau interne à l'entrepôt de Lausanne. Il relie PC-LAUSANNE à sa passerelle R2-LSN (Gi0/1). Il est limité à un seul site et entièrement géré par Léman Logistique. |
| LAN-NYO (Nyon) — 192.168.10.64/27 | **LAN** | Réseau interne à l'entrepôt de Nyon. Il relie PC-NYON à sa passerelle R3-NYO (Gi0/1). Même logique que Lausanne. |
| WAN-1 (R1-GVA Gi0/1 ↔ R2-LSN Gi0/0) — 192.168.10.96/30 | **WAN** | Liaison point-à-point entre Genève et Lausanne (environ 60 km). Elle relie deux sites situés dans deux villes différentes, via une infrastructure louée à un opérateur. |
| WAN-2 (R1-GVA Gi0/2 ↔ R3-NYO Gi0/0) — 192.168.10.100/30 | **WAN** | Liaison point-à-point entre Genève et Nyon (environ 25 km). Mêmes caractéristiques que WAN-1. |
| Liaison R1-GVA Gi0/0 ↔ Internet | **WAN** | Accès au réseau public. Dans la maquette, il est simulé par le nœud NAT de GNS3 ; en déploiement réel, ce serait la liaison vers le fournisseur d'accès (FAI). |

### Critères utilisés pour justifier

1. **L'étendue géographique** : un LAN couvre un seul site, un WAN relie des sites situés dans des villes différentes.
2. **Le propriétaire de l'infrastructure** : le LAN est géré par Léman Logistique, le WAN repose sur un opérateur.
3. **Le rôle des routeurs** : ils se trouvent à la frontière entre LAN et WAN. R2-LSN et R3-NYO ont chacun une interface côté LAN (Gi0/1, la passerelle des postes) et une interface côté WAN (Gi0/0).

### Remarques

- Le siège de Genève n'a pas de LAN sur ce schéma. R1-GVA sert de **point central** : il relie les deux entrepôts entre eux et c'est le seul routeur qui donne accès à Internet.
- Les liaisons WAN sont en **point-à-point** (un seul routeur à chaque extrémité), ce qui explique le choix de masques /30 dans le plan d'adressage.

---

## 1.2 Types de flux

### Rappel des notions

- **Unicast** : un appareil envoie un message à **un seul destinataire** précis (communication de 1 vers 1).
- **Broadcast** : un appareil envoie un message à **tous les appareils du réseau local** en même temps (1 vers tous). Adresse IP 255.255.255.255, adresse MAC FF:FF:FF:FF:FF:FF. **Les routeurs ne transmettent pas le broadcast** : il reste bloqué dans le réseau local.
- **Multicast** : un appareil envoie un seul flux à **un groupe d'abonnés** (1 vers plusieurs choisis). Adresses de 224.0.0.0 à 239.255.255.255.

Pour choisir, on se pose une question : **à qui l'appareil parle-t-il ?**

### Application aux quatre flux

| Flux | Type | Justification |
|---|---|---|
| (a) Un PC demande une adresse par DHCP | **Broadcast** | Au démarrage, le PC n'a pas encore d'adresse IP et ne connaît pas l'adresse du serveur DHCP. Il ne peut donc pas lui écrire directement : il envoie un message *DHCP Discover* à tout le réseau local (IP de destination 255.255.255.255, MAC de destination FF:FF:FF:FF:FF:FF) en espérant qu'un serveur DHCP réponde. |
| (b) PC-LAUSANNE consulte un site web | **Unicast** | La communication se fait entre une seule source (PC-LAUSANNE) et une seule destination (le serveur web). Chaque paquet contient l'adresse IP précise du serveur. |
| (c) Diffusion vidéo des consignes de sécurité vers tous les entrepôts | **Multicast** | Un seul flux vidéo est envoyé vers une adresse de groupe, et seuls les postes abonnés le reçoivent. Le **broadcast ne conviendrait pas**, car R1-GVA, R2-LSN et R3-NYO ne le transmettent pas : la vidéo resterait bloquée dans le LAN d'origine et n'atteindrait jamais les autres entrepôts. L'**unicast ne conviendrait pas non plus**, car il faudrait envoyer une copie séparée à chaque poste, ce qui surchargerait inutilement les liaisons WAN-1 et WAN-2. |
| (d) Un PC cherche la MAC de sa passerelle | **Broadcast** (puis unicast) | Le PC connaît l'adresse IP de sa passerelle mais pas son adresse MAC, qui est nécessaire pour construire la trame. Il envoie donc une requête **ARP** à tout le LAN (« Qui a 192.168.10.1 ? ») avec la MAC de destination FF:FF:FF:FF:FF:FF. Seul R2-LSN répond, en **unicast**, avec la MAC de son interface Gi0/1. |

---

## 1.3 Tableau comparatif OSI / TCP-IP

### Rappel des notions

Le **modèle OSI** découpe la communication réseau en **7 couches**. Chaque couche a un rôle précis et ajoute ses propres informations au message : c'est l'**encapsulation**. Le nom du « paquet » de données à chaque couche s'appelle la **PDU** (Protocol Data Unit).

Le **modèle TCP/IP** décrit la même chose en **4 couches** seulement. Les couches 5, 6 et 7 de l'OSI sont regroupées dans la couche Application, et les couches 1 et 2 dans la couche Accès réseau. Le modèle OSI sert de référence théorique pour décrire et dépanner un réseau ; le modèle TCP/IP décrit ce qui fonctionne réellement sur notre maquette.

### Tableau

| Couche OSI | Couche TCP/IP | Rôle | PDU | Protocole (exemple) | Équipement du projet qui y opère |
|---|---|---|---|---|---|
| 7 — Application | Application | Fournit les services réseau aux applications de l'utilisateur. | Données | HTTP, DNS, SSH | Navigateur de PC-LAUSANNE ; client SSH (PuTTY) utilisé pour administrer R1-GVA, R2-LSN et R3-NYO |
| 6 — Présentation | Application | Met les données dans un format commun, gère le chiffrement et la compression. | Données | TLS, chiffrement SSH | Chiffrement de la session SSH v2 qui remplace Telnet sur les routeurs |
| 5 — Session | Application | Ouvre, maintient et ferme le dialogue entre deux applications. | Données | Gestion de session SSH | Session d'administration entre une VM et un routeur |
| 4 — Transport | Transport | Découpe les données, gère les numéros de port, la fiabilité (TCP) ou la rapidité (UDP). | Segment (TCP) / Datagramme (UDP) | TCP, UDP | PC-LAUSANNE et PC-NYON (ports source et destination) ; R1-GVA lors du PAT, qui réécrit les numéros de port |
| 3 — Réseau | Internet | Adressage logique (IP) et choix du meilleur chemin (routage). | Paquet | IP, ICMP | R1-GVA, R2-LSN, R3-NYO (routage statique entre les sites, NAT sur R1-GVA) |
| 2 — Liaison de données | Accès réseau | Adressage physique (MAC), construction des trames, détection d'erreurs. | Trame | Ethernet, ARP | Interfaces Gi0/0, Gi0/1, Gi0/2 des routeurs ; cartes réseau des VM ; adaptateurs VMnet2 et VMnet3 |
| 1 — Physique | Accès réseau | Transmission des bits sur le support. | Bits | Normes Ethernet (câble, signal) | Liens GNS3 entre les routeurs ; nœuds Cloud reliant la maquette aux VM |

### Explication du lien avec le projet

- Les **routeurs** R1-GVA, R2-LSN et R3-NYO travaillent principalement en **couche 3** : leur métier est de lire l'adresse IP de destination d'un paquet et de choisir par quelle interface l'envoyer.
- R1-GVA intervient aussi en **couche 4** quand il fait du **PAT** : il modifie les numéros de port pour distinguer les connexions des différents postes qui partagent la même adresse vers Internet.
- Le passage de **Telnet à SSH**, demandé par l'auditeur sécurité, concerne les couches hautes : SSH est un protocole applicatif (couche 7) qui **chiffre** les échanges (couche 6). Telnet, lui, envoie tout en clair, y compris les mots de passe.

---

## 1.4 Parcours d'un paquet de PC-LAUSANNE vers 8.8.8.8

### Rappel des notions

- L'**adresse IP** indique la **destination finale** du paquet. Elle reste la même pendant tout le trajet, sauf au passage du NAT.
- L'**adresse MAC** indique le **prochain équipement** sur le segment local. Elle change à chaque saut, car chaque routeur fabrique une nouvelle trame.
- Le **NAT/PAT** remplace une adresse IP privée par une adresse utilisable vers l'extérieur. Il est nécessaire car les adresses 192.168.0.0/16 sont **privées** (RFC 1918) et ne peuvent pas circuler sur Internet.

### Chemin

PC-LAUSANNE → R2-LSN → WAN-1 → R1-GVA → nœud NAT → Internet → 8.8.8.8

### Étape 1 — Sur PC-LAUSANNE : l'encapsulation

1. L'application crée les **données** (par exemple une requête `ping` ou DNS).
2. La couche Transport ajoute les numéros de port : on obtient un **segment**.
3. La couche Réseau ajoute l'en-tête IP, avec IP source = 192.168.10.2 et IP destination = 8.8.8.8 : on obtient un **paquet**.
4. Le PC compare 8.8.8.8 à son propre réseau 192.168.10.0/26 : la destination est **en dehors** de son réseau. Il doit donc passer par sa passerelle, 192.168.10.1.
5. Le PC trouve l'adresse MAC de sa passerelle grâce à **ARP** (voir 1.2 d).
6. La couche Liaison ajoute l'en-tête Ethernet, avec MAC source = PC-LAUSANNE et MAC destination = R2-LSN Gi0/1 : on obtient une **trame**.
7. La couche Physique transmet les **bits** vers R2-LSN.

### Étape 2 — Sur R2-LSN

1. La trame arrive sur l'interface **Gi0/1**.
2. R2-LSN retire l'en-tête Ethernet (désencapsulation) et lit l'IP de destination : 8.8.8.8.
3. Il consulte sa table de routage. Il n'a pas de route précise vers 8.8.8.8, donc il utilise sa **route par défaut**, qui pointe vers R1-GVA (192.168.10.97).
4. Il fabrique une **nouvelle trame** avec sa propre MAC (Gi0/0) en source et la MAC de R1-GVA (Gi0/1) en destination, puis l'envoie par **Gi0/0**.
5. Les adresses IP ne changent pas.

### Étape 3 — Sur R1-GVA : routage et NAT

1. La trame arrive sur **Gi0/1**, qui est une interface **NAT inside**.
2. R1-GVA lit l'IP de destination et utilise sa route par défaut vers Internet, qui sort par **Gi0/0**, une interface **NAT outside**.
3. Comme le paquet passe d'une interface *inside* à une interface *outside*, le **PAT** s'applique : l'IP source **192.168.10.2** est remplacée par l'adresse de **Gi0/0** (192.168.122.x), et le port source peut être modifié.
4. R1-GVA enregistre cette correspondance dans sa **table NAT**, pour savoir à qui renvoyer la réponse.
5. Il fabrique une nouvelle trame et l'envoie vers le nœud NAT.

### Tableau des adresses à chaque saut

| Tronçon | MAC source | MAC destination | IP source | IP destination |
|---|---|---|---|---|
| PC-LAUSANNE → R2-LSN (Gi0/1) | MAC de PC-LAUSANNE | MAC de R2-LSN Gi0/1 | 192.168.10.2 | 8.8.8.8 |
| R2-LSN (Gi0/0) → R1-GVA (Gi0/1) | MAC de R2-LSN Gi0/0 | MAC de R1-GVA Gi0/1 | 192.168.10.2 | 8.8.8.8 |
| R1-GVA (Gi0/0) → nœud NAT | MAC de R1-GVA Gi0/0 | MAC du nœud NAT | **192.168.122.x** (après NAT) | 8.8.8.8 |

### Ce qu'on observe

- Les **adresses MAC changent à chaque saut** : chaque routeur fabrique une nouvelle trame avec sa propre MAC en source et celle de l'équipement suivant en destination. Une adresse MAC n'a de sens que sur le segment local.
- Les **adresses IP ne changent pas** entre PC-LAUSANNE et R1-GVA. Elles changent **uniquement au NAT** sur R1-GVA, où l'adresse privée 192.168.10.2 devient l'adresse de Gi0/0.

### Remarque sur la maquette

L'interface Gi0/0 de R1-GVA reçoit son adresse par DHCP depuis le nœud NAT de GNS3 (réseau 192.168.122.0/24). Ce nœud joue le rôle du fournisseur d'accès : il effectue à son tour une traduction vers l'adresse réelle de l'ordinateur hôte avant la sortie vers Internet. En déploiement réel, Gi0/0 aurait une adresse publique fournie par le FAI.

### Le retour de la réponse

1. La réponse de 8.8.8.8 revient vers l'adresse de Gi0/0 de R1-GVA.
2. R1-GVA consulte sa **table NAT** et remet **192.168.10.2** comme adresse de destination.
3. Il envoie le paquet vers R2-LSN par Gi0/1, grâce à sa route statique vers 192.168.10.0/26.
4. R2-LSN le délivre à PC-LAUSANNE par Gi0/1.
5. À chaque saut, les adresses MAC sont de nouveau réécrites.

---

# PARTIE B — Plan d'adressage (VLSM)

## Rappel des notions

- Une **adresse IP** comporte 32 bits, écrits en 4 nombres de 0 à 255. Elle se compose d'une **partie réseau** (qui identifie le réseau) et d'une **partie hôte** (qui identifie l'appareil dans ce réseau).
- Le **masque** indique où s'arrête la partie réseau. Il s'écrit en décimal (255.255.255.192) ou en notation **CIDR** (/26 = 26 bits pour le réseau).
- Dans chaque réseau, **2 adresses sont réservées** :
  - la **première** est l'**adresse du réseau** (elle identifie le réseau lui-même) ;
  - la **dernière** est l'**adresse de broadcast** (elle sert à envoyer un message à tous les appareils du réseau).
- D'où la formule : **nombre d'hôtes utilisables = 2ⁿ − 2**, où n est le nombre de bits réservés aux hôtes.
- Le **VLSM** (Variable Length Subnet Mask) consiste à découper un bloc en sous-réseaux **de tailles différentes**, adaptées au besoin de chacun.

## Règles imposées par le client

1. Découper dans l'ordre décroissant des besoins, en partant de 192.168.10.0.
2. Passerelle d'un LAN = première adresse utilisable.
3. Adaptateur hôte VMware (VMnet) = dernière adresse utilisable du LAN.
4. Sur les liaisons WAN, R1-GVA prend la première adresse utilisable.

## Besoins classés du plus grand au plus petit

| Ordre | Segment | Besoin |
|---|---|---|
| 1 | LAN-LSN (Lausanne) | 50 hôtes |
| 2 | LAN-NYO (Nyon) | 20 hôtes |
| 3 | WAN-1 (R1-GVA ↔ R2-LSN) | 2 hôtes (point-à-point) |
| 4 | WAN-2 (R1-GVA ↔ R3-NYO) | 2 hôtes (point-à-point) |

## Tableau récapitulatif

| Segment | Réseau | Masque décimal | CIDR | 1ʳᵉ utilisable | Dernière utilisable | Broadcast | Nb d'hôtes |
|---|---|---|---|---|---|---|---|
| LAN-LSN (Lausanne) | 192.168.10.0 | 255.255.255.192 | /26 | 192.168.10.1 | 192.168.10.62 | 192.168.10.63 | 62 |
| LAN-NYO (Nyon) | 192.168.10.64 | 255.255.255.224 | /27 | 192.168.10.65 | 192.168.10.94 | 192.168.10.95 | 30 |
| WAN-1 (R1-GVA ↔ R2-LSN) | 192.168.10.96 | 255.255.255.252 | /30 | 192.168.10.97 | 192.168.10.98 | 192.168.10.99 | 2 |
| WAN-2 (R1-GVA ↔ R3-NYO) | 192.168.10.100 | 255.255.255.252 | /30 | 192.168.10.101 | 192.168.10.102 | 192.168.10.103 | 2 |
| Réserve (non attribuée) | 192.168.10.104 → 192.168.10.255 | — | — | — | — | — | 152 adresses |

## Attribution des adresses par équipement

| Équipement | Interface | Segment | Adresse IP | Règle appliquée |
|---|---|---|---|---|
| R1-GVA | Gi0/0 | Vers Internet (nœud NAT) | DHCP (192.168.122.0/24) | Adresse fournie par le nœud NAT |
| R1-GVA | Gi0/1 | WAN-1 | 192.168.10.97 /30 | R1 = 1ʳᵉ utilisable du WAN |
| R1-GVA | Gi0/2 | WAN-2 | 192.168.10.101 /30 | R1 = 1ʳᵉ utilisable du WAN |
| R2-LSN | Gi0/0 | WAN-1 | 192.168.10.98 /30 | Adresse restante du WAN-1 |
| R2-LSN | Gi0/1 | LAN-LSN | 192.168.10.1 /26 | Passerelle = 1ʳᵉ utilisable |
| R3-NYO | Gi0/0 | WAN-2 | 192.168.10.102 /30 | Adresse restante du WAN-2 |
| R3-NYO | Gi0/1 | LAN-NYO | 192.168.10.65 /27 | Passerelle = 1ʳᵉ utilisable |
| Hôte VMware | VMnet2 | LAN-LSN | 192.168.10.62 /26 | VMnet = dernière utilisable |
| Hôte VMware | VMnet3 | LAN-NYO | 192.168.10.94 /27 | VMnet = dernière utilisable |
| PC-LAUSANNE | Carte réseau | LAN-LSN | 192.168.10.2 /26 — GW 192.168.10.1 | Adresse libre de la plage |
| PC-NYON | Carte réseau | LAN-NYO | 192.168.10.66 /27 — GW 192.168.10.65 | Adresse libre de la plage |

## Détail des calculs

### Astuces utilisées

- **Taille du bloc** = 2ⁿ (n = bits hôtes).
- **CIDR** = 32 − n.
- **Dernier nombre du masque** = 256 − taille du bloc.
- **Broadcast** = adresse réseau + taille du bloc − 1.
- **Réseau suivant** = broadcast précédent + 1.

### LAN-LSN — 50 hôtes

- On cherche le plus petit n tel que 2ⁿ − 2 ≥ 50 :
  - n = 5 → 2⁵ − 2 = 30 → insuffisant
  - n = 6 → 2⁶ − 2 = 62 → **suffisant**
- CIDR : 32 − 6 = **/26**
- Masque : 256 − 64 = 192 → **255.255.255.192** (en binaire : 11000000 = 128 + 64)
- Taille du bloc : 2⁶ = **64**
- Adresse réseau : **192.168.10.0** (point de départ imposé)
- Broadcast : 0 + 64 − 1 = **192.168.10.63**
- Plage utilisable : **192.168.10.1 → 192.168.10.62**
- Passerelle (R2-LSN Gi0/1) : **192.168.10.1** — VMnet2 : **192.168.10.62**

### LAN-NYO — 20 hôtes

- On cherche le plus petit n tel que 2ⁿ − 2 ≥ 20 :
  - n = 4 → 2⁴ − 2 = 14 → insuffisant
  - n = 5 → 2⁵ − 2 = 30 → **suffisant**
- CIDR : 32 − 5 = **/27**
- Masque : 256 − 32 = 224 → **255.255.255.224** (en binaire : 11100000 = 128 + 64 + 32)
- Taille du bloc : 2⁵ = **32**
- Adresse réseau : broadcast de LAN-LSN + 1 = 63 + 1 = **192.168.10.64**
- Broadcast : 64 + 32 − 1 = **192.168.10.95**
- Plage utilisable : **192.168.10.65 → 192.168.10.94**
- Passerelle (R3-NYO Gi0/1) : **192.168.10.65** — VMnet3 : **192.168.10.94**

### WAN-1 — liaison point-à-point (2 hôtes)

- On cherche le plus petit n tel que 2ⁿ − 2 ≥ 2 :
  - n = 2 → 2² − 2 = 2 → **suffisant** (un routeur à chaque extrémité)
- CIDR : 32 − 2 = **/30**
- Masque : 256 − 4 = 252 → **255.255.255.252** (en binaire : 11111100)
- Taille du bloc : 2² = **4**
- Adresse réseau : broadcast de LAN-NYO + 1 = 95 + 1 = **192.168.10.96**
- Broadcast : 96 + 4 − 1 = **192.168.10.99**
- Plage utilisable : **192.168.10.97 → 192.168.10.98**
- R1-GVA Gi0/1 : **192.168.10.97** — R2-LSN Gi0/0 : **192.168.10.98**

### WAN-2 — liaison point-à-point (2 hôtes)

- Même calcul que WAN-1 : **/30**, **255.255.255.252**, bloc de **4**
- Adresse réseau : broadcast de WAN-1 + 1 = 99 + 1 = **192.168.10.100**
- Broadcast : 100 + 4 − 1 = **192.168.10.103**
- Plage utilisable : **192.168.10.101 → 192.168.10.102**
- R1-GVA Gi0/2 : **192.168.10.101** — R3-NYO Gi0/0 : **192.168.10.102**

## Vérifications

### Pas de chevauchement

Chaque réseau commence juste après le broadcast du précédent :

| Fin du réseau précédent | Début du réseau suivant |
|---|---|
| LAN-LSN se termine à .63 | LAN-NYO commence à .64 |
| LAN-NYO se termine à .95 | WAN-1 commence à .96 |
| WAN-1 se termine à .99 | WAN-2 commence à .100 |

De plus, chaque adresse réseau est un multiple de la taille de son bloc (0 pour un bloc de 64, 64 pour un bloc de 32, 96 et 100 pour des blocs de 4). Aucune adresse n'appartient donc à deux réseaux à la fois.

### Pas de gaspillage injustifié

- **LAN-LSN** : 62 adresses pour 50 hôtes. Un /27 (30 hôtes) serait trop petit. La marge de 12 adresses correspond à la croissance déjà incluse dans le besoin.
- **LAN-NYO** : 30 adresses pour 20 hôtes. Un /28 (14 hôtes) serait trop petit.
- **WAN-1 et WAN-2** : le /30 est le plus petit réseau qui offre 2 adresses utilisables, exactement le besoin d'une liaison point-à-point.
- **Total utilisé** : 64 + 32 + 4 + 4 = **104 adresses sur 256**. Les **152 adresses restantes** (192.168.10.104 → 192.168.10.255) sont gardées en réserve pour une extension future (nouveau site, nouvelle liaison WAN, LAN supplémentaire).

## Justification des choix

**Pourquoi découper le bloc en plusieurs réseaux ?**
Chaque site et chaque liaison entre routeurs doit avoir son propre réseau. Sans cela, les routeurs ne pourraient pas savoir vers quel site envoyer un paquet, puisque leur rôle est justement de faire passer le trafic d'un réseau à un autre.

**Pourquoi utiliser le VLSM ?**
Les besoins sont très différents : 50 hôtes à Lausanne, mais seulement 2 sur une liaison WAN. Si on donnait la même taille à tous les réseaux, on gaspillerait beaucoup d'adresses. Le VLSM permet d'adapter la taille de chaque réseau à son besoin réel.

**Pourquoi commencer par le plus grand besoin ?**
Un bloc doit commencer sur un multiple de sa taille (un bloc de 64 commence à 0, 64, 128 ou 192). En plaçant d'abord les plus grands blocs, les plus petits se placent à la suite sans laisser de trous inutilisables. C'est aussi une règle imposée par le client.

**Pourquoi retirer 2 adresses dans chaque réseau ?**
La première adresse identifie le réseau lui-même et la dernière sert au broadcast. Aucune des deux ne peut être attribuée à un appareil.

**Pourquoi /30 pour les liaisons WAN ?**
Une liaison point-à-point ne relie que deux routeurs. Il faut donc exactement 2 adresses utilisables, et le /30 (4 adresses dont 2 utilisables) est le plus petit réseau qui le permet.

**Pourquoi la passerelle sur la première adresse ?**
C'est une règle du client, et c'est une convention pratique : tous les postes savent facilement où se trouve leur passerelle (.1 à Lausanne, .65 à Nyon).

**Pourquoi le VMnet sur la dernière adresse ?**
Le VMnet est la carte réseau de l'ordinateur hôte qui fait le lien avec les VM. En le plaçant à la fin de la plage, il ne gêne pas les adresses attribuées aux postes, qui peuvent se suivre à partir de .2 (Lausanne) ou .66 (Nyon).

**Pourquoi garder 152 adresses en réserve ?**
Elles permettront d'ajouter un nouveau site ou une nouvelle liaison plus tard sans devoir redécouper tout le plan d'adressage.

---

# PARTIE C — Schéma réseau complété

Valeurs à reporter dans les cases vides du schéma fourni :

| Case du schéma | Valeur |
|---|---|
| WAN-1 · R1 ↔ R2 | 192.168.10.96 /30 |
| WAN-2 · R1 ↔ R3 | 192.168.10.100 /30 |
| LAN-LSN · 50 hôtes | 192.168.10.0 /26 |
| LAN-NYO · 20 hôtes | 192.168.10.64 /27 |
| PC-LAUSANNE — IP | 192.168.10.2 /26 |
| PC-LAUSANNE — GW | 192.168.10.1 |
| PC-NYON — IP | 192.168.10.66 /27 |
| PC-NYON — GW | 192.168.10.65 |