# TP ICND1 – Interconnexion des sites et sécurisation des accès

## À faire avant de rendre (à supprimer ensuite)

Ce rapport décrit uniquement ce qui a réellement été fait sur ta maquette. Les points ci-dessous manquent encore ou sont à vérifier dans ton cahier des charges : coche-les, fais les captures, puis supprime cette section.



## Informations

| | |
| --- | --- |
| Étudiante | Rrudhani Dea |
| Formation | CFC Informaticien – Geneva Institute of Technology |
| Client | Léman Logistique SA – Ticket SAT-2026-0412 |
| Prestataire | Satom IT & Learning Solutions |
| Dates d'intervention | 01.10.2026 et 02.10.2026 |

## Contexte et maquette

Léman Logistique SA a trois sites : le siège de Genève (R1-GNV) et deux entrepôts, à Lausanne (R2-LSN) et à Nyon (R3-NYO). L'intervention relie les trois sites par routage statique, donne l'accès Internet par NAT sur R1, raccorde un poste client par site et sécurise l'administration des routeurs.

La maquette tourne dans GNS3 avec trois routeurs Cisco c7200 (IOS 15.2) émulés par Dynamips. Les deux postes clients sont des VM VMware Workstation, reliées à GNS3 par des nœuds Cloud :

- PC-LSN : VM Windows 11, sur VMnet1
- PC-NYO : VM Ubuntu, sur VMnet2
- Accès Internet : nœud Cloud sur VMnet8 (NAT de VMware)

![image](https://hackmd.io/_uploads/rykUPQT5Mg.png)



## Plan d'adressage

Le bloc imposé 192.168.10.0/24 est découpé en VLSM, du plus grand besoin au plus petit, selon le corrigé du formateur.

| Segment | Réseau | Masque | Équipements |
| --- | --- | --- | --- |
| LAN-LSN (50 hôtes) | 192.168.10.0 /26 | 255.255.255.192 | R2-LSN .1, PC-LSN .10, carte VMnet1 de l'hôte .62 |
| LAN-NYO (20 hôtes) | 192.168.10.64 /27 | 255.255.255.224 | R3-NYO .65, PC-NYO .70, carte VMnet2 de l'hôte .94 |
| WAN-1 | 192.168.10.96 /30 | 255.255.255.252 | R1-GNV .97, R2-LSN .98 |
| WAN-2 | 192.168.10.100 /30 | 255.255.255.252 | R1-GNV .101, R3-NYO .102 |
| Internet | 192.168.209.0 /24 | 255.255.255.0 | R1-GNV en DHCP (VMnet8), adresse reçue 192.168.209.148 |

Calcul : 50 hôtes → 2^6 − 2 = 62 (/26) ; 20 hôtes → 2^5 − 2 = 30 (/27) ; liaison point à point → 2^2 − 2 = 2 (/30). Les adresses .104 à .255 restent libres pour de futurs sites.

### Interfaces des routeurs

Le schéma du client est dessiné pour des routeurs IOSv. Sur le c7200, les noms d'interfaces diffèrent ; voici le câblage réel de la maquette.

| Routeur | Rôle | Interface c7200 | Adresse |
| --- | --- | --- | --- |
| R1-GNV | WAN-2 vers R3-NYO | Gi0/0 | 192.168.10.101 /30 |
| R1-GNV | WAN-1 vers R2-LSN | Gi1/0 | 192.168.10.97 /30 |
| R1-GNV | Accès Internet (VMnet8) | Gi2/0 | DHCP |
| R2-LSN | WAN-1 vers R1-GNV | Gi0/0 | 192.168.10.98 /30 |
| R2-LSN | Passerelle LAN-LSN | Gi1/0 | 192.168.10.1 /26 |
| R3-NYO | WAN-2 vers R1-GNV | Gi0/0 | 192.168.10.102 /30 |
| R3-NYO | Passerelle LAN-NYO | Gi1/0 | 192.168.10.65 /27 |

## Partie 1 – Interconnexion des sites et intégration VMware

Les trois sites communiquent par routage statique, sortent sur Internet par le NAT de R1-GNV, et chaque poste client joint les deux routeurs distants, l'autre poste et Internet.

### Préparation de VMware Workstation

VMnet1 et VMnet2 sont des commutateurs virtuels : chacun relie une VM à l'interface LAN d'un routeur. Ils portent donc le même réseau que le LAN, et le DHCP de VMware est coupé pour que les VM ne reçoivent pas d'adresse hors plan.

| Réglage | VMnet1 (LAN-LSN) | VMnet2 (LAN-NYO) |
| --- | --- | --- |
| Type | Host-only | Host-only |
| DHCP de VMware | désactivé | désactivé |
| Subnet IP | 192.168.10.0 | 192.168.10.64 |
| Subnet mask | 255.255.255.192 | 255.255.255.224 |

VMnet8 reste en NAT avec son DHCP : il fournit l'accès Internet à R1-GNV, sur le réseau 192.168.209.0/24, sans chevauchement avec le bloc du client.

![Capture d'écran 2026-10-02 103801](https://hackmd.io/_uploads/H1EVOXaqze.png)
![Capture d'écran 2026-10-02 103754](https://hackmd.io/_uploads/rkV4OQTcGx.png)

### Cartes VMnet de l'hôte Windows

VMware donne d'office à la carte de l'hôte la première adresse du réseau (.1 et .65), qui est celle des passerelles R2-LSN et R3-NYO. Pour éviter le conflit, chaque carte prend la dernière adresse utilisable de son LAN, sans passerelle.

| Carte de l'hôte | Adresse | Masque |
| --- | --- | --- |
| VMware Network Adapter VMnet1 | 192.168.10.62 | 255.255.255.192 |
| VMware Network Adapter VMnet2 | 192.168.10.94 | 255.255.255.224 |

![Capture d'écran 2026-10-02 104617](https://hackmd.io/_uploads/B1r5u7aqzx.png)
![Capture d'écran 2026-10-02 104555](https://hackmd.io/_uploads/HJr9Ompczg.png)



### Nœuds Cloud dans GNS3

| Nœud Cloud | Carte liée | Branché sur |
| --- | --- | --- |
| PC-LSN | VMware Network Adapter VMnet1 | R2-LSN Gi1/0 |
| PC-NYO | VMware Network Adapter VMnet2 | R3-NYO Gi1/0 |
| INTERNET | VMware Network Adapter VMnet8 | R1-GNV Gi2/0 |

![image](https://hackmd.io/_uploads/r16Xo7a5fe.png)

### Routage statique

Un routeur ne connaît que ses réseaux directement connectés. R1-GNV, au centre, reçoit une route vers chaque LAN. R2-LSN et R3-NYO n'ont qu'une sortie, vers R1-GNV : une route par défaut leur suffit.

R1-GNV :

```
ip route 192.168.10.0 255.255.255.192 192.168.10.98
ip route 192.168.10.64 255.255.255.224 192.168.10.102
interface GigabitEthernet2/0
 ip address dhcp
```

R2-LSN :

```
ip route 0.0.0.0 0.0.0.0 192.168.10.97
```

R3-NYO :

```
ip route 0.0.0.0 0.0.0.0 192.168.10.101
```

La route par défaut de R1-GNV n'est pas saisie : elle est apprise par DHCP sur Gi2/0 (S* 0.0.0.0/0 [254/0] via 192.168.209.2, distance 254).

![image](https://hackmd.io/_uploads/r1-dsQacGg.png)


![image](https://hackmd.io/_uploads/ByrIj7Tczx.png)

![image](https://hackmd.io/_uploads/S1utjmT5zx.png)

### NAT sur R1-GNV

Les adresses 192.168.10.x sont privées : R1-GNV les traduit en son adresse Gi2/0 pour sortir sur Internet (PAT, plusieurs postes partagent une adresse grâce aux numéros de port).

```
interface GigabitEthernet0/0
 ip nat inside
interface GigabitEthernet1/0
 ip nat inside
interface GigabitEthernet2/0
 ip nat outside
access-list 1 permit 192.168.10.0 0.0.0.255
ip nat inside source list 1 interface GigabitEthernet2/0 overload
```

Exemple de traduction relevé après un ping de R3-NYO vers 8.8.8.8 :

| Inside local | Inside global | Destination |
| --- | --- | --- |
| 192.168.10.102 | 192.168.209.148 | 8.8.8.8 |

![image](https://hackmd.io/_uploads/Syuso7T9Mg.png)

### Postes clients

| Paramètre | PC-LSN (Windows 11) | PC-NYO (Ubuntu) |
| --- | --- | --- |
| Carte VMware | Custom : VMnet1 | Custom : VMnet2 |
| Adresse IP | 192.168.10.10 /26 | 192.168.10.70 /27 |
| Passerelle | 192.168.10.1 (R2-LSN) | 192.168.10.65 (R3-NYO) |
| DNS | 8.8.8.8 et 1.1.1.1 | 8.8.8.8 et 1.1.1.1 |

Sur Ubuntu, l'adresse est fixée dans un fichier netplan unique, /etc/netplan/01-tp.yaml, appliqué avec sudo netplan apply (voir incident 7). Sur Windows, elle est saisie dans les propriétés IPv4 de la carte Ethernet0. Le pare-feu Windows a reçu une règle autorisant le ping entrant, pour que PC-NYO puisse joindre PC-LSN.

📸 11 – PC-LSN : ipconfig![Capture d'écran 2026-10-02 120752](https://hackmd.io/_uploads/Bk3P3Qaqzl.png)


![Capture d'écran 2026-10-02 105617](https://hackmd.io/_uploads/HkL7hmTqfx.png)


### Tests

| Depuis | Vers | Résultat | TTL |
| --- | --- | --- | --- |
| R2-LSN | R1-GNV (192.168.10.97) | Bien | |
| R3-NYO | R1-GNV (192.168.10.101) | Bien  | |
| R3-NYO | Internet (8.8.8.8) | Bien  | |
| PC-LSN | Passerelle R2-LSN (192.168.10.1) | Bien | 255 |
| PC-LSN | PC-NYO (192.168.10.70) | Bien | 61 |
| PC-LSN | google.com |Bien | 126 |
| PC-NYO | Passerelle R3-NYO (192.168.10.65) | Bien  | 255 |
| PC-NYO | 8.8.8.8 | Bien  | 126 |
| PC-NYO | google.com |Bien | 126 |
| PC-NYO | PC-LSN (192.168.10.10) |Bien  | |

Le TTL de 61 entre les deux PC montre le chemin : Ubuntu part de 64, et le paquet traverse R3-NYO, R1-GNV et R2-LSN (64 − 3 = 61). Un routeur Cisco répond avec 255 quand il est directement joint.

📸 13 – PC-LSN : pings vers la passerelle, PC-NYO, 8.8.8.8 et google.com![Capture d'écran 2026-10-02 140946](https://hackmd.io/_uploads/HJeMaQaqMx.png)
![Capture d'écran 2026-10-02 121646](https://hackmd.io/_uploads/rkezpXTcMl.png)
![Capture d'écran 2026-10-02 120814](https://hackmd.io/_uploads/Bkefa7p9Mg.png)

![Capture d'écran 2026-10-02 151416](https://hackmd.io/_uploads/HkbBy4a5Mg.png)


![image](https://hackmd.io/_uploads/HyydRQpcMe.png)

![Capture d'écran 2026-10-02 151730](https://hackmd.io/_uploads/r1MdyN65zl.png)


## Partie 2 – Mise en service et sécurisation des accès

Les trois routeurs acceptent une administration à distance par SSH v2 avec un compte local, testée depuis les deux postes clients. Telnet est encore ouvert pour la démonstration et doit être fermé à la fin (voir la section À faire).

### Configuration de base (2.1 → 2.8) –

Commandes à appliquer sur chaque routeur, en adaptant le nom et en reprenant les valeurs imposées par le CDC 2 :

```
configure terminal
banner motd #Acces reserve au personnel autorise de Leman Logistique SA#
service password-encryption
no ip domain-lookup
line con 0
 password [mot de passe console du CDC]
 login
 logging synchronous
 exec-timeout 5 0
end
write memory
```

Déjà en place sur les 3 routeurs : nom d'hôte, enable secret, no ip domain-lookup, adresses et descriptions, sauvegarde avec write memory.![Capture d'écran 2026-10-02 135254](https://hackmd.io/_uploads/ry-4g4Tqfl.png)
![Capture d'écran 2026-10-02 132801](https://hackmd.io/_uploads/Bk-VeNaqMx.png)
![image](https://hackmd.io/_uploads/r1q8gN69fl.png)




### Accès SSH et Telnet

Mêmes commandes sur les trois routeurs :

```
ip domain-name leman.local
enable secret <REDACTED>
username admin privilege 15 secret <REDACTED>
crypto key generate rsa modulus 2048
ip ssh version 2
line vty 0 4
 login local
 transport input ssh telnet
```

La clé RSA ne peut être générée qu'une fois le nom d'hôte et le domaine définis : elle est nommée d'après eux (par exemple R2-LSN.leman.local). login local oblige à s'authentifier avec un compte nominatif, au lieu d'un mot de passe commun aux lignes VTY.

![image](https://hackmd.io/_uploads/ByHKxVp5zx.png)
![image](https://hackmd.io/_uploads/HJb1-Np9fx.png)
![image](https://hackmd.io/_uploads/B16kb4aczx.png)

### Côté postes clients

IOS 15.2 ne propose que d'anciens algorithmes (échange de clés SHA1, chiffrement CBC, intégrité HMAC-SHA1), désactivés par défaut dans les clients OpenSSH récents. Ils ont été autorisés uniquement pour les adresses de la maquette, dans ~/.ssh/config sur Ubuntu et %USERPROFILE%\.ssh\config sur Windows :

```
Host 192.168.10.*
    KexAlgorithms +diffie-hellman-group14-sha1
    HostKeyAlgorithms +ssh-rsa
    Ciphers +aes128-cbc
    MACs +hmac-sha1
```

La ligne MACs n'a été nécessaire que sur Windows. En production, il vaudrait mieux mettre à jour l'IOS que d'accepter ces algorithmes.

Sur Windows, le client Telnet a été activé avec dism /online /Enable-Feature /FeatureName:TelnetClient.

### Tests d'accès

| Depuis | Vers | SSH | Telnet |
| --- | --- | --- | --- |
| PC-NYO (Ubuntu) | R1-GNV, R2-LSN, R3-NYO | ✅ | ✅ |
| PC-LSN (Windows) | R2-LSN | ✅ | [à compléter] |
| PC-LSN (Windows) | R1-GNV, R3-NYO | [à compléter] | [à compléter] |

![image](https://hackmd.io/_uploads/SkjlfEa5fe.png)

![image](https://hackmd.io/_uploads/HyrOzV65fg.png)
![image](https://hackmd.io/_uploads/rk25GE69Ge.png)
![image](https://hackmd.io/_uploads/SJ2kXV69fl.png)
![image](https://hackmd.io/_uploads/rkxf74acMg.png)

![image](https://hackmd.io/_uploads/BJCXXVTqfx.png)


📸 22 – Windows : connexions SSH vers les 3 routeurs
![image](https://hackmd.io/_uploads/S1R9XVT9Gx.png)
![image](https://hackmd.io/_uploads/BJGaX4a9Me.png)
![image](https://hackmd.io/_uploads/HJX1EETqzg.png)


![image](https://hackmd.io/_uploads/HJ9jNN6cMx.png)
![Capture d'écran 2026-10-02 154042](https://hackmd.io/_uploads/B1HnN4acfe.png)
![image](https://hackmd.io/_uploads/HJuJB465Mx.png)

### Démonstration Wireshark – 

Capture sur le lien WAN-1 (R1-GNV Gi1/0 ↔ R2-LSN Gi0/0), clic droit sur le lien dans GNS3 → Start capture, puis une session Telnet et une session SSH vers R2-LSN. Dans Wireshark : clic droit sur un paquet → Suivre → Flux TCP.


![image](https://hackmd.io/_uploads/Hy9BI4p5Mg.png)

![image](https://hackmd.io/_uploads/HkMN_E69Gg.png)

![image](https://hackmd.io/_uploads/HyRmtV6cGe.png)
![image](https://hackmd.io/_uploads/rJHHF4a9zx.png)
![Capture d'écran 2026-10-02 160118](https://hackmd.io/_uploads/Sk3HtNp9Mg.png)

### Comparatif Telnet / SSH

| Critère | Telnet | SSH v2 |
| --- | --- | --- |
| Port | TCP 23 | TCP 22 |
| Chiffrement | Aucun, tout passe en clair | Oui, après l'échange de clés (ici AES-128-CBC, HMAC-SHA1, clé RSA 2048 bits) |
| Identité du routeur | Non vérifiée | Le routeur présente sa clé RSA, le client mémorise son empreinte |
| Visible dans Wireshark | Identifiants, mot de passe et commandes | Bannières de version et échange de clés, le reste est illisible |

Recommandation pour Léman Logistique SA : n'administrer les routeurs qu'en SSH v2 (transport input ssh), limiter les adresses autorisées par une ACL sur les lignes VTY (access-class), et créer un compte par administrateur.

## Récapitulatif des incidents

Le plus instructif a été le n° 8 : le ping passait mais pas le DNS, et le debug de R1-GNV a montré que le NAT rejetait l'UDP.

| N° | Équipement | Symptôme | Cause | Résolution |
| --- | --- | --- | --- | --- |
| 1 | GNS3 | Dynamips error : unable to rename VM instance | Renommage d'un routeur allumé | Routeur arrêté avant le renommage, puis hostname dans IOS |
| 2 | R1, R2, R3 | Anciennes adresses 192.168.11.x et anciennes routes | Configuration d'un essai précédent restée en mémoire | Nouvelles adresses appliquées, anciennes routes supprimées avec no ip route |
| 3 | R1, R2, R3 | Ancienne configuration revenue après write erase | Stop/Start dans GNS3 recharge la configuration gardée dans le projet | Configuration corrigée en console, write memory puis sauvegarde du projet (Ctrl+S) |
| 4 | R1-GNV | % Invalid input sur ip address | Commande tapée en mode privilégié au lieu du mode interface | configure terminal puis interface avant la commande |
| 5 | R2-LSN | Adresses de R3 appliquées sur R2 | Bloc de configuration collé dans la mauvaise console | Bonnes adresses réappliquées, route erronée supprimée |
| 6 | R1, R2, R3 | Configuration vide (Method unset) après redémarrage du PC | Projet GNS3 non sauvegardé | Routeurs reconfigurés, write memory et Ctrl+S |
| 7 | PC-NYO | nmcli con up bloqué puis Échec de connexion | Activation en attente d'IPv6 et trois fichiers netplan contradictoires | Anciens fichiers mis de côté, un seul fichier /etc/netplan/01-tp.yaml, IPv6 désactivé |
| 8 | R1-GNV | Ping vers 8.8.8.8 OK, mais DNS en host unreachable depuis PC-NYO | debug ip nat : translation failed (A) pour l'UDP ; tracepath montrait !H renvoyé par 192.168.10.101 | Règle ip nat inside source réappliquée une fois l'adresse DHCP reçue sur Gi2/0 |
| 9 | R3-NYO | NAT inutile sur R3 | Reste d'un essai précédent (ip nat et access-list 1) | Supprimé : seul R1-GNV fait du NAT |
| 10 | PC-NYO, PC-LSN | SSH : no matching key exchange / cipher / MAC | Algorithmes anciens d'IOS 15.2 refusés par OpenSSH | Fichier .ssh/config limité aux adresses 192.168.10.* |
| 11 | R1, R2 | % Please define a domain-name first, puis SSH Connection refused | Clé RSA absente faute de nom de domaine | ip domain-name puis crypto key generate rsa modulus 2048 |
| 12 | R2-LSN | SSH : Permission denied, même avec un compte de test | Lignes VTY sans login local | line vty 0 4, login local, transport input ssh telnet |
| 13 | Maquette | Pings avec 20 à 50 % de pertes et 150 ms | Dynamips sans Idle-PC, processeur de l'hôte saturé | Valeur Idle-PC calculée dans GNS3 |


## Bilan

Le réseau fonctionne de bout en bout ; il reste la configuration de base et la démonstration Wireshark de la partie 2.

| Demandé | Résultat | Captures |
| --- | --- | --- |
| Plan d'adressage VLSM appliqué | ✅ | 01 |
| VMnet1, VMnet2 et cartes de l'hôte | ✅ | 02 à 05 |
| Trois nœuds Cloud (VMnet1, VMnet2, VMnet8) | ✅ | 06 |
| Routage statique sur les 3 routeurs | ✅ | 07 à 09 |
| Accès Internet et NAT sur R1-GNV | ✅ | 10 |
| Postes clients configurés et testés | ✅ | 11 à 15 |
| Configuration de base 2.1 à 2.8 | [à réaliser] | 16 à 18 |
| SSH v2 sur les 3 routeurs | ✅ | 19 à 23 |
| Démonstration Telnet / SSH avec Wireshark | [à réaliser] | 24, 25 |
| Telnet refusé à la fin | [à réaliser] | 26 |

### Fichiers joints

| Fichier | Contenu |
| --- | --- |
| R1-GNV.txt, R2-LSN.txt, R3-NYO.txt | show running-config final des trois routeurs |
| Capture_Telnet.pcapng | Capture Wireshark de la session Telnet |
| Capture_SSH.pcapng | Capture Wireshark de la session SSH |
| 01-tp.yaml | Configuration réseau de PC-NYO |