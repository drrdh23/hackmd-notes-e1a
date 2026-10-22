# TP Final 
## Cahier des charges 2 — POC multi-hyperviseurs : Proxmox

# CDC 2 – POC multi-hyperviseurs : ESXi + Proxmox

**Client :** Léman Transports & Logistique SA
**Réalisé par :** Rrudhani Deaa (Proxmox) et [nom de ta copine] (ESXi)
**Date :** 25.09.2026

---

## 1. Objectif

Le client hésite entre VMware ESXi et Proxmox VE. Il veut une preuve que les deux plateformes peuvent **cohabiter sur le même réseau** : une VM Windows sur ESXi, une VM sur Proxmox, et qu'elles **communiquent entre elles**.

| Partie | Responsable |
|---|---|
| ESX-LAB01 + VM01-WIN | lilou|
| PVE-LAB01 + VM02-WIN |Deaa |
| Tests et analyse | Ensemble |

---

## 2. Environnement réel et écarts par rapport à la consigne

### 2.1 Environnement

La consigne proposait une virtualisation imbriquée dans VMware Workstation, ou des serveurs physiques s'ils étaient disponibles. Nous avons installé les deux hyperviseurs **directement sur des PC physiques de la salle** (bare-metal), à partir de clés USB d'installation. C'est plus proche d'une vraie production : les hyperviseurs utilisent directement le matériel.

### 2.2 Plan d'adressage adapté

Le réseau de la salle n'est pas en 192.168.50.0/24 mais en **10.40.10.0/24**. Comme le permet la consigne, nous avons adapté le plan en gardant les mêmes derniers octets pour les machines (.11, .21, .22). J'ai choisi pour dernier triplet 129.


**Règle respectée :** même réseau, même masque, même passerelle et même DNS pour toutes les machines, mais une adresse différente pour chacune. Deux machines avec la même IP entreraient en conflit.

### 2.3 Autres écarts (validés par le professeur)

| Consigne | Réalisé | Raison |
|---|---|---|
| VM02 sous Ubuntu Server | VM02 sous **Windows Server 2022** | Validé par le professeur |
| IP par Netplan | IP par les **paramètres IPv4 de Windows** | Netplan n'existe que sous Linux |
| qemu-guest-agent par apt | Agent installé avec **virtio-win-guest-tools** | Équivalent Windows |
| VM ID 102 | VM ID **[100 ou 102]** |100 |

---

## 3. Lexique

| Terme | Définition |
|---|---|
| **Hyperviseur de type 1** | Logiciel installé directement sur le matériel, qui fait tourner des VMs (ESXi, Proxmox) |
| **Bare-metal** | Installation directe sur un PC physique, sans système en dessous |
| **vmbr0** | Pont réseau (switch virtuel) de Proxmox qui relie les VMs à la carte réseau physique |
| **vSwitch0 / Port Group** | Switch virtuel d'ESXi et groupe de ports sur lequel on branche les VMs |
| **VirtIO / VMXNET3** | Cartes réseau virtuelles optimisées de Proxmox et de VMware. Windows n'a pas leurs pilotes par défaut |
| **Agent invité** | Programme installé dans la VM qui permet à l'hyperviseur de communiquer avec elle (QEMU Guest Agent, VMware Tools) |
| **Snapshot** | Photo de l'état d'une VM à un instant donné, pour pouvoir y revenir |
| **ICMP** | Protocole utilisé par la commande ping |
| **ARP** | Protocole qui trouve l'adresse MAC d'une machine à partir de son adresse IP |
| **Passerelle** | Routeur qui permet de sortir du réseau local (vers Internet ou un autre réseau) |

---

# PARTIE A – Proxmox VE (PVE-LAB01)
## Etape A1 - Téléchargement de l'iso Proxmox VE

1. Dans un premier temps j'ai taper sur la bre de recherche firefox Proxmox VE iso download : https://www.proxmox.com/en/downloads/proxmox-virtual-environment/iso
2. Puis j'ai télécharger ![image](https://hackmd.io/_uploads/HkvCsUI5Gx.png) et placer dans mon bureau 
3. Réalisation de la clé bootable , il faut faire attention à ne pas avoir de fichiers important sur la clé car ils disparaitront par la suite (minimum 8 Go).
4. A l'aide de l'application RUFUS ![image](https://hackmd.io/_uploads/BJsI2IIcfl.png)
5. Séléctionner le fichier iso télécharger , Format par defaut MBR  BIOS/UEFI 1 passe FAT 32![Capture d'écran 2026-09-25 114035](https://hackmd.io/_uploads/SypF2U8cMg.png)

6. Appuyer sur démarrer et attendre que la clé se boot attendre le message "Prêt" et fermer ![Capture d'écran 2026-09-25 114226](https://hackmd.io/_uploads/H1pY2U8cMl.png)


 


## Étape A2 – Installer Proxmox VE

- **Quand :** en premier, sur le PC physique.
- **Que faire :** installer Proxmox VE à partir d'une clé USB.
- **Comment :**
  1. Démarrer le PC sur la clé USB (menu de démarrage : F9 sur HP).
  2. Choisir **Install Proxmox VE (Terminal UI)**.
  3. Accepter la licence, choisir le disque interne. ![image](https://hackmd.io/_uploads/BJ4008U9ze.png)

  4. Pays **Switzerland**, fuseau **Europe/Zurich**, clavier **US**(a choisir selon le clavier que l'on a) (Nous avons rectifier par ls suite il s'agit pas d'un clavier swiss german mais d'un clavier US..).
  5. Mot de passe root et adresse e-mail. ![image](https://hackmd.io/_uploads/rJKsJvLqGe.png)

  6. Réseau :

     | Champ | Valeur |
     |---|---|
     | Management interface | La carte connectée au réseau |
     | Hostname (FQDN) | pve-lab01.ltl.local |
     | IP | 10.40.10.129/24 |
     | Gateway | 10.40.10.1 |
     | DNS | 8.8.8.8 | (correction du dns serveur par la suite )

![Capture d'écran 2026-09-27 112004](https://hackmd.io/_uploads/S1QblvL5zl.png)


  7. Lancer l'installation, **retirer la clé USB** avant le redémarrage.
- **Pourquoi :**
  - Proxmox est un hyperviseur de **type 1** : il s'installe directement sur le matériel, ce qui donne les meilleures performances.
  - Le **FQDN** (nom complet avec domaine) est obligatoire : Proxmox l'utilise pour s'identifier et générer ses certificats.
  - On retire la clé avant de redémarrer, sinon le PC relance l'installation.
- **Résultat attendu :** au redémarrage, un écran noir affiche `https://10.40.10.129:8006/`.
![image](https://hackmd.io/_uploads/rkGogDUqfe.png)



## Étape A3 – Accéder à l'interface web

- **Quand :** juste après l'installation.
- **Que faire :** se connecter à l'interface d'administration depuis un autre ordinateur.
- **Comment :**
  1. Depuis un portable sur le même réseau, ouvrir **https://10.40.10.129:8006**.![Capture d'écran 2026-09-25 161846](https://hackmd.io/_uploads/S1BAePIczx.png)

  2. Accepter l'avertissement de sécurité (**Paramètres avancés → Continuer**).
  3. Se connecter avec **root**, Realm **Linux PAM**.
  4. Cliquer sur **OK** au message « No valid subscription ».
- **Pourquoi :**
  - L'avertissement apparaît parce que Proxmox utilise un **certificat auto-signé** (créé par lui-même). La connexion est quand même chiffrée.
  - Le port **8006** est le port de l'interface web de Proxmox.
  - Le message d'abonnement est normal : nous utilisons la version gratuite.
- **Résultat attendu :** l'interface Proxmox s'affiche avec le nœud pve-lab01.

![image](https://hackmd.io/_uploads/HJRU-vUcMx.png)



## Étape A4 – Changer les dépôts et mettre à jour

- **Quand :** avant toute création de VM.
- **Que faire :** désactiver le dépôt enterprise, activer le dépôt no-subscription, mettre à jour le système.
- **Comment :**
  1. **pve-lab01 → Updates → Repositories**.
  2. Sélectionner la ligne **pve-enterprise** → **Disable** (idem pour **ceph enterprise**).
  3. **Add → No-Subscription → Add**.
  4. **Updates → Refresh**, puis **Upgrade** et confirmer avec `y`.
- **Pourquoi :**
  - Le dépôt **enterprise** est réservé aux clients qui paient un abonnement. Sans abonnement, les mises à jour échouent.
  - Le dépôt **no-subscription** est gratuit et contient les mêmes mises à jour, testées un peu moins longtemps.
  - La mise à jour corrige les failles de sécurité et les bugs connus.
- **Résultat attendu :** la mise à jour se termine sans erreur (TASK OK).


## Étape A5 – Vérifier le pont vmbr0

- **Quand :** après la mise à jour.
- **Que faire :** vérifier que le switch virtuel est bien relié à la carte réseau physique.
- **Comment :** **pve-lab01 → System → Network**, ligne **vmbr0**.
- **Pourquoi :** vmbr0 est un **switch virtuel**. Les VMs s'y branchent, et lui est branché sur la carte physique du PC. C'est ce qui permet aux VMs d'être sur le même réseau que les autres machines de la salle.
- **Résultat attendu :**

  | Champ | Valeur |
  |---|---|
  | Type | Linux Bridge |
  | Ports/Slaves | [carte physique, ex. nic0] |
  | CIDR | 10.40.10.145/24 |
  | Gateway | 10.40.10.1 |

![image](https://hackmd.io/_uploads/r1IVmb45zx.png)

## Étape A6 – Téléverser les ISOs

- **Quand :** avant de créer la VM.
- **Que faire :** mettre dans Proxmox l'ISO de Windows Server 2022 et l'ISO des pilotes **virtio-win**.
- **Comment :**
  - Windows Server : **local (pve-lab01) → ISO Images → Upload**.
  - virtio-win : **local (pve-lab01) → ISO Images → Download from URL** avec l'adresse
    `https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso`
    puis **Query URL** et **Download**.
- **Pourquoi :**
  - Proxmox a besoin des ISOs dans son stockage pour les « insérer » dans le lecteur CD virtuel des VMs.
  - L'ISO **virtio-win** contient les pilotes de la carte réseau VirtIO et l'agent QEMU pour Windows, que Windows n'a pas par défaut.
  - **Download from URL** permet à Proxmox de télécharger directement le fichier, sans passer par l'ordinateur portable.
- **Résultat attendu :** les deux ISOs apparaissent dans **ISO Images**.

![image](https://hackmd.io/_uploads/H1wLQb4qMl.png)


## Étape A7 – Créer la VM VM02-WIN

- **Quand :** une fois les ISOs disponibles 














- **Que faire :** créer la machine virtuelle.
- **Comment :** bouton **Create VM** : 
  | Onglet | Paramètre | Valeur |
  |---|---|---|
  | General | VM ID / Name | [100 ou 102] / VM02-WIN |
  | OS | ISO / Type | Windows Server 2022 / Microsoft Windows |
  | System | Qemu Agent | Coché |
  | Disks | Bus / Taille | SATA / [32 ou 50] Go |
  | CPU | Sockets / Cores | 1 / 2 |
  | Memory | RAM | 2048 Mo |
  | Network | Bridge / Model | vmbr0 / VirtIO |
![Capture d'écran 2026-09-21 144019](https://hackmd.io/_uploads/SyZz6tw8qMg.png)
![Capture d'écran 2026-09-25 131252](https://hackmd.io/_uploads/BkbM6FwLcGx.png)
![Capture d'écran 2026-09-25 131256](https://hackmd.io/_uploads/BylGTFPU5Mg.png)
![Capture d'écran 2026-09-25 131300](https://hackmd.io/_uploads/Hy-fTYwU9Me.png)
![Capture d'écran 2026-09-25 131317](https://hackmd.io/_uploads/H1efaFwL5ze.png)
![Capture d'écran 2026-09-25 133813](https://hackmd.io/_uploads/HJfTFwU5ze.png)
![Capture d'écran 2026-09-25 133816](https://hackmd.io/_uploads/H1zatw8qGx.png)
![Capture d'écran 2026-09-25 133833](https://hackmd.io/_uploads/SJxGaYDI5fg.png)
![Capture d'écran 2026-09-25 133846](https://hackmd.io/_uploads/r1M6YvUqMl.png)
![Capture d'écran 2026-09-25 133936](https://hackmd.io/_uploads/SyMTtvLqGg.png)

 

  Puis **Hardware → Add → CD/DVD Drive** : second lecteur avec l'ISO **virtio-win**.
- **Pourquoi :**
  - **1 socket, 2 cores** = 2 vCPU. Windows Server est licencié par processeur, donc 1 socket est préférable.
  - **Disque SATA** : Windows reconnaît ce type de disque sans pilote supplémentaire.
  - **Carte VirtIO** : carte réseau paravirtualisée, plus performante qu'une carte émulée. Son pilote est installé après Windows.
  - **Qemu Agent coché** : prépare la communication entre Proxmox et la VM.
  - **Second lecteur CD** : l'un contient Windows, l'autre les pilotes.
- **Résultat attendu :** la VM apparaît dans la liste, avec deux lecteurs CD dans **Hardware**.

![image](https://hackmd.io/_uploads/HJfFQb49Gl.png)


## Étape A8 – Installer Windows Server 2022

- **Quand :** après la création de la VM.
- **Que faire :** installer le système d'exploitation.
- **Comment :**
  1. **Start → Console**.
  2. Au message **« Press any key to boot from CD or DVD »**, appuyer tout de suite sur une touche.                                                                                          ![Capture d'écran 2026-09-25 134118](https://hackmd.io/_uploads/BkG6tv8qzx.png)
  3. Clavier **French (Switzerland)** → **Install now**.
  4. Édition : **Windows Server 2022 Standard Evaluation (Desktop Experience)**.
  5. Accepter la licence → **Custom** (et non Upgrade) → **Drive 0**.![Capture d'écran 2026-09-25 134511](https://hackmd.io/_uploads/HkfaKPUczg.png)
![Capture d'écran 2026-09-25 134610](https://hackmd.io/_uploads/SJfaYDI9fg.png)
  6. Définir le mot de passe **Administrator**.
  7. Pour se connecter : envoyer **Ctrl+Alt+Del** depuis le menu latéral de la console (bouton **Extra keys**).
- **Pourquoi :**
  - **Desktop Experience** : version avec interface graphique. Sans cette mention, Windows s'installe uniquement en ligne de commande.
  - **Evaluation** : version d'essai gratuite de 180 jours, adaptée à un lab.
  - **Custom** : installation propre sur un disque vide. **Upgrade** sert uniquement à mettre à jour un Windows existant.![Capture d'écran 2026-09-25 135108](https://hackmd.io/_uploads/BkfTYPU5fx.png)
  - Le **Ctrl+Alt+Del** doit passer par la console, sinon il est intercepté par l'ordinateur local. le dernier carré( faire cette manipulation car sinon c'est le pc portable qui interceptera la manip si cela est fait avec le clavier)
![Capture d'écran 2026-09-25 135317](https://hackmd.io/_uploads/B1bpYDL9fl.png)



## Étape A9 – Installer les pilotes VirtIO et l'agent QEMU

- **Quand :** juste après l'installation de Windows.
- **Que faire :** installer le pilote de la carte réseau et l'agent invité.
- **Comment :**
  1. Dans Windows : **Explorateur de fichiers → This PC** → lecteur **virtio-win**.
  2. Lancer **virtio-win-guest-tools.exe** → **I agree** → **Install** → **Close**.
- **Pourquoi :**
  - Sans ce pilote, Windows ne reconnaît pas la carte VirtIO et n'a **aucun réseau**.
  - L'**agent QEMU** permet à Proxmox d'afficher l'IP de la VM, de l'éteindre proprement et de faire des sauvegardes cohérentes.
  - Le programme installe les deux en une seule fois.


![Capture d'écran 2026-09-25 162054](https://hackmd.io/_uploads/HywAQWNqfg.png)
![Capture d'écran 2026-09-25 141228](https://hackmd.io/_uploads/HJGpKDL5Ml.png)
![Capture d'écran 2026-09-25 142034](https://hackmd.io/_uploads/S1zpKwIcfe.png)
## Étape A10 – Configurer l'IP fixe

- **Quand :** après l'installation du pilote réseau.
- **Que faire :** donner à la VM l'adresse prévue dans le plan d'adressage.
- **Comment :**
  1. Clic droit sur **Démarrer → Network Connections → Change adapter options**.
  2. Clic droit sur **Ethernet → Properties → Internet Protocol Version 4 (TCP/IPv4)**.
  3. Saisir :

     | Champ | Valeur |
     |---|---|
     | IP address | 10.40.10.145 |
     | Subnet mask | 255.255.255.0 |
     | Default gateway | 10.40.10.1 |
     | Preferred DNS | 8.8.8.8 |

  4. Vérifier dans **cmd** avec `ipconfig`.
- **Pourquoi :** un serveur doit avoir une **adresse fixe**. Avec une adresse automatique (DHCP), elle pourrait changer et les autres machines ne le retrouveraient plus. Cette étape remplace Netplan, puisque la VM est sous Windows.
- **Résultat attendu :** `ipconfig` affiche 10.40.10.22.

![image](https://hackmd.io/_uploads/SkgIVZE5zl.png)


## Étape A11 – Renommer la machine

- **Quand :** après la configuration IP.
- **Que faire :** donner le nom VM02-WIN à la machine.
- **Comment :** **Settings → System → About → Rename this PC** → `VM02-WIN` → **Restart now**.
- **Pourquoi :** Windows génère un nom aléatoire à l'installation (du type WIN-XXXX). Un nom clair permet d'identifier la machine sur le réseau et dans la documentation.
- **Résultat attendu :** la page **About** affiche VM02-WIN.

**Figure A10-1 – Nom de la machine**
> 📸 *À prendre : console, Settings → System → About, après le redémarrage. On doit voir : Device name VM02-WIN. → Colle l'image ici, puis supprime ce bloc.*

## Étape A12 – Autoriser le ping entrant

- **Quand :** avant les tests.
- **Que faire :** autoriser les requêtes ping sans désactiver le pare-feu.
- **Comment :** **Windows Defender Firewall with Advanced Security → Inbound Rules** → **File and Printer Sharing (Echo Request - ICMPv4-In)** → clic droit → **Enable Rule**.
- **Pourquoi :** Windows bloque par défaut les pings entrants. En activant uniquement cette règle, on autorise le ping sans ouvrir le reste : le pare-feu continue de protéger la machine. Désactiver entièrement le pare-feu serait une mauvaise pratique de sécurité.
![Capture d'écran 2026-09-16 180804](https://hackmd.io/_uploads/Syq8Wu8cMx.png)
![image](https://hackmd.io/_uploads/ryfYfuL5Mx.png)
![image](https://hackmd.io/_uploads/HJ1qfOL9zg.png)






## Étape A13 – Vérifier l'agent dans Proxmox

- **Quand :** après l'installation de l'agent et la configuration IP.
- **Que faire :** vérifier que Proxmox communique avec la VM.
- **Comment :**
  1. **VM → Options → QEMU Guest Agent** : **Enabled**.
  2. **Shutdown** puis **Start** de la VM depuis Proxmox.
  3. **VM → Summary**, section **IPs**.
- **Pourquoi :** l'agent n'est pris en compte qu'après un arrêt complet de la VM (un redémarrage depuis Windows ne suffit pas). Si Proxmox affiche l'IP, c'est qu'il communique avec l'agent : c'est la preuve qu'il fonctionne.
- **Résultat attendu :** **Summary** affiche 10.40.10.22.


## Étape A14 – Snapshot

- **Quand :** une fois la VM entièrement configurée.
- **Que faire :** prendre un snapshot nommé POC-post-install.
- **Comment :**
  1. **Hardware** : retirer les deux ISOs (**Do not use any media**).
  2. **Snapshots → Take Snapshot** → Name : `POC-post-install`.
- **Pourquoi :**
  - Le snapshot permet de **revenir à cet état propre** en quelques secondes si une manipulation échoue pendant les tests.
  - On retire les ISOs avant, car elles ne sont plus utiles et un média monté peut gêner certaines opérations.
  - Un snapshot **n'est pas une sauvegarde** : il dépend du disque de la VM et disparaît avec lui.
- **Résultat attendu :** le snapshot apparaît dans la liste.


---

# PARTIE B – VMware ESXi (ESX-LAB01)

## Étape B1 – Installer ESXi 8

- **Quand :** en parallèle de la partie Proxmox, sur un autre PC physique.
- **Que faire :** installer ESXi 8.0.3 à partir d'une clé USB.
- **Comment :**
  1. Démarrer sur la clé (menu de démarrage, ou clé placée avant « Windows Boot Manager » dans le BIOS).
  2. **Entrée** (Welcome) → **F11** (licence) → choisir le disque interne.
  3. Clavier **Swiss French**, mot de passe root.
  4. **F11** pour installer.
  5. **Retirer la clé USB**, puis **Entrée** pour redémarrer.
- **Pourquoi :** ESXi est aussi un hyperviseur de **type 1**. La clé doit être retirée avant le redémarrage, sinon le PC relance l'installation.
- **Résultat attendu :** l'écran jaune et gris d'ESXi s'affiche.

## Étape B2 – Configurer le réseau de gestion

- **Quand :** au premier démarrage d'ESXi.
- **Que faire :** donner une IP fixe à l'ESXi, sur le même réseau que Proxmox.
- **Comment :** **F2** → root → **Configure Management Network** :
  1. **Network Adapters** : cocher la carte **Connected**.
  2. **IPv4 Configuration** : **Set static IPv4 address** → IP `10.40.10.11`, masque `255.255.255.0`, passerelle [passerelle].
  3. **DNS Configuration** : DNS `8.8.8.8`, hostname `esx-lab01`.
  4. **Échap → Y** pour appliquer.
  5. **Test Management Network** : ping de la passerelle, du DNS et de **10.40.10.129** (Proxmox).
- **Pourquoi :** pour que les VMs des deux hyperviseurs communiquent, **les deux hyperviseurs doivent être dans le même réseau**, avec le même masque et la même passerelle. Le test de ping vers 10.40.10.129 vérifie directement que ESXi voit Proxmox.
- **Résultat attendu :** l'écran jaune affiche `https://10.40.10.11/`, et les tests affichent **OK**.

## Étape B3 – Configuration dans le client web

- **Quand :** une fois l'IP configurée.
- **Que faire :** accéder au client web, configurer le NTP, le datastore et vérifier le réseau.
- **Comment :**
  1. Ouvrir **https://10.40.10.11/ui**, se connecter en root.
  2. **NTP** : Host → Manage → System → Time & date → Edit NTP Settings → `ch.pool.ntp.org`, puis démarrer le service **ntpd**.
  3. **Datastore** : Storage → Datastores → [création de DS-LAB01 sur le second disque / renommage de datastore1 en DS-LAB01].
  4. **Réseau** : Networking → vérifier **vSwitch0** (uplink vmnic0) et le Port Group **VM Network**.
- **Pourquoi :**
  - **NTP** : une heure exacte est indispensable pour les journaux, les certificats et les futurs clusters.
  - **Datastore** : espace de stockage où ESXi range les fichiers des VMs, au format **VMFS**.
  - **vSwitch0 / VM Network** : équivalent de vmbr0 chez Proxmox. Le vSwitch est relié à la carte physique, et le Port Group est la « prise » sur laquelle on branche les VMs.
- **Résultat attendu :** NTP actif, DS-LAB01 visible, vSwitch0 relié à vmnic0.

## Étape B4 – Créer la VM VM01-UBU

- **Quand :** après la configuration de l'ESXi.
- **Que faire :** créer la VM qui tournera sur ESXi. Nous avons choisi **Ubuntu 64 bits** au lieu de Windows.
- **Comment :** **Virtual Machines → Create / Register VM → Create a new virtual machine** :

  | Paramètre | Valeur |
  |---|---|
  | Name | VM01-UBU |
  | Guest OS family | Linux |
  | Guest OS version | Ubuntu Linux (64-bit) |
  | Storage | DS-LAB01 |
  | CPU | 2 |
  | Memory | 4 GB |
  | Hard disk 1 | 60 GB, **Thin provisioned** |
  | Network Adapter 1 | VM Network, type **VMXNET3** |
  | CD/DVD Drive 1 | Datastore ISO file → ISO Ubuntu |

- **Pourquoi :**
  - **Guest OS : Ubuntu Linux (64-bit)** : ESXi adapte le matériel virtuel proposé au système choisi.
  - **Thin** : le disque ne consomme de l'espace qu'au fur et à mesure qu'on écrit dedans, au lieu de réserver 60 Go d'un coup.
  - **VMXNET3** : carte réseau optimisée de VMware, l'équivalent de VirtIO. Contrairement à Windows, **Ubuntu intègre déjà son pilote** : le réseau fonctionne dès l'installation.
- **Résultat attendu :** la VM apparaît dans la liste des machines virtuelles.

## Étape B5 – Installer Ubuntu

- **Quand :** après la création de la VM.
- **Que faire :** installer le système d'exploitation.
- **Comment :**
  1. **Power on**, puis ouvrir la console.
  2. Suivre l'installation : langue, clavier **French (Switzerland)**, disque entier.
  3. Nom de la machine : `vm01-ubu`, création d'un utilisateur et de son mot de passe.
  4. Redémarrer à la fin de l'installation.
- **Pourquoi :** Ubuntu est une distribution Linux gratuite et très répandue. La version **64 bits** est nécessaire pour utiliser plus de 4 Go de RAM et correspond à l'architecture x86-64 du serveur.
- **Résultat attendu :** l'écran de connexion d'Ubuntu s'affiche avec le nom vm01-ubu.

## Étape B6 – Installer les VMware Tools (open-vm-tools)

- **Quand :** juste après l'installation d'Ubuntu.
- **Que faire :** installer l'agent invité de VMware.
- **Comment :** dans un terminal :
```bash
  sudo apt update
  sudo apt install open-vm-tools -y
```
  (Ubuntu l'installe souvent automatiquement quand il détecte ESXi : la commande confirme alors qu'il est déjà présent.)
- **Pourquoi :** **open-vm-tools** est la version libre des VMware Tools pour Linux, recommandée par VMware. C'est l'équivalent de l'agent QEMU chez Proxmox : ESXi peut afficher l'IP de la VM et l'éteindre proprement.
- **Résultat attendu :** dans le client web, la VM affiche **VMware Tools : Running**.

## Étape B7 – Configurer l'IP fixe avec Netplan

- **Quand :** après l'installation d'Ubuntu.
- **Que faire :** donner à la VM l'adresse du plan d'adressage.
- **Comment :**
  1. Trouver le nom de la carte avec `ip a` (sur ESXi avec VMXNET3, souvent **ens160**).
  2. Éditer le fichier : `sudo nano /etc/netplan/50-cloud-init.yaml`
```yaml
     network:
       version: 2
       ethernets:
         ens160:
           dhcp4: false
           addresses:
             - 10.40.10.21/24
           routes:
             - to: default
               via: [passerelle]
           nameservers:
             addresses:
               - 8.8.8.8
```
  3. Appliquer : `sudo netplan apply`, puis vérifier avec `ip a`.
- **Pourquoi :**
  - Un serveur doit avoir une **adresse fixe**, sinon les autres machines ne le retrouvent plus si l'adresse change.
  - **Netplan** est l'outil de configuration réseau d'Ubuntu. Le fichier est au format YAML, très strict : uniquement des espaces, jamais de tabulations.
- **Résultat attendu :** `ip a` affiche 10.40.10.21/24 sur ens160.

## Étape B8 – Pare-feu et ping entrant

- **Quand :** avant les tests.
- **Que faire :** vérifier que la VM accepte les pings.
- **Comment :** `sudo ufw status`
- **Pourquoi :** sous Ubuntu, le pare-feu **UFW** est **désactivé par défaut**, et même quand il est activé, il autorise le ping (ICMP). Contrairement à Windows, il n'y a donc **aucune règle à ajouter**. C'est une différence importante entre les deux systèmes.
- **Résultat attendu :** `Status: inactive`, ou ICMP autorisé si UFW est actif.

## Étape B9 – Snapshot

- **Quand :** une fois la VM configurée.
- **Que faire :** prendre un snapshot nommé POC-post-install.
- **Comment :** clic droit sur **VM01-UBU → Snapshots → Take snapshot** → Name : `POC-post-install`.
- **Pourquoi :** le snapshot permet de revenir à cet état propre en quelques secondes si une manipulation échoue. Il ne remplace pas une sauvegarde : il dépend du disque de la VM.
- **Résultat attendu :** POC-post-install apparaît dans **Manage snapshots**.

---

# PARTIE C – Tests de validation (recette)

## Étape C1 – Tests prévus

| # | Source | Destination | Commande | Résultat attendu | Résultat obtenu |
|---|---|---|---|---|---|
| T1 | VM01-UBU | VM02-WIN (10.40.10.22) | `ping -c 4 10.40.10.22` | 4/4 réponses | Non réalisé |
| T2 | VM02-WIN | VM01-UBU (10.40.10.21) | `ping -n 4 10.40.10.21` | 4/4 réponses | Non réalisé |
| T3 | VM01-UBU | PVE-LAB01 (10.40.10.129) | `ping -c 4 10.40.10.129` | 4/4 réponses | Non réalisé |
| T4 | VM02-WIN | ESX-LAB01 (10.40.10.11) | `ping -n 4 10.40.10.11` | 4/4 réponses | [Réalisé / non réalisé] |
| T5 | VM01-UBU | — | `ip neigh` | MAC de VM02 visible | Non réalisé |
| T6 | VM02-WIN | — | `arp -a` | MAC de VM01 visible | Non réalisé |
| T7 | VM01-UBU | VM02-WIN | `traceroute 10.40.10.22` | 1 saut | Non réalisé |

- **Ce que chaque test aurait dû prouver :**
  - **T1 et T2** : les deux VMs communiquent dans les deux sens.
  - **T3 et T4** : chaque VM atteint l'hyperviseur « d'en face », donc les deux plateformes sont sur le même réseau.
  - **T5 et T6** : chaque VM connaît l'adresse MAC de l'autre, preuve qu'elles se sont trouvées directement par **ARP**.
  - **T7** : un seul saut signifie qu'aucun routeur n'est traversé : les deux VMs sont sur le **même segment réseau**.

## Étape C2 – Pourquoi les tests n'ont pas pu être réalisés

- **Problème :** le mot de passe de l'utilisateur de VM01-UBU a été perdu.
- **Conséquence :** sans ce mot de passe, il était impossible de se connecter à la VM pour lancer les commandes.
- **Solution possible :** réinitialiser le mot de passe (démarrage en mode de récupération) ou réinstaller la VM. Elle n'avait plus le temps de le faire pendant la séance, et le faire après la séance, chacune de son côté, était impossible : nos PC n'étaient plus reliés au réseau de la salle par câble Ethernet, donc les VMs ne pouvaient plus communiquer.

## Étape C3 – Test de remplacement

- **Que faire :** vérifier quand même que ma VM communique avec une machine hébergée sur un autre hyperviseur.
- **Comment :** ping vers le serveur d'un camarade (ART Solaj).

  | Source | Destination | Commande | Résultat |
  |---|---|---|---|
  | [VM02-WIN] | Serveur d'ART Solaj (10.40.10.250) | `ping -n 4 [10.40.10.250]` | Réponses reçues : le ping a fonctionné |
![image](https://hackmd.io/_uploads/r1LxpdLczx.png)

- **Absence de capture :** ce serveur était hébergé sur le grand serveur de la classe, sur lequel ce groupe travaillait. Ils étaient en train de refaire leurs configurations pour leur propre TP : nous n'avons donc pas pu refaire le test pour en prendre une capture.
- **Ce que ce test prouve :** ma VM, hébergée sur Proxmox, peut communiquer avec une machine hébergée sur un autre hyperviseur du même réseau. C'est le principe demandé par le POC : des plateformes différentes qui cohabitent sur le même réseau.
- **Ce qu'il ne prouve pas :** il ne remplace pas complètement la recette T1 à T7, puisque la communication avec VM01-UBU sur ESXi n'a pas été testée.

## Étape C4 – Ce qu'il aurait fallu faire

### C4.1 – Réinitialiser le mot de passe de VM01-UBU

- **Quand :** dès que le mot de passe perdu a été constaté, avant de lancer les tests.
- **Que faire :** redonner un mot de passe à l'utilisateur de VM01-UBU sans réinstaller la VM.
- **Comment :**
  1. Redémarrer la VM depuis le client web ESXi (**Actions → Power → Reset**).
  2. Au démarrage, maintenir **Maj** (ou appuyer sur **Échap**) pour afficher le menu **GRUB**.
  3. Choisir **Advanced options for Ubuntu**, puis la ligne **(recovery mode)**.
  4. Dans le menu de récupération, choisir **root – Drop to root shell prompt**.
  5. Rendre le disque modifiable, puis changer le mot de passe :
```bash
     mount -o remount,rw /
     passwd [nom_utilisateur]
```
  6. Taper deux fois le nouveau mot de passe, puis redémarrer avec `reboot`.
- **Pourquoi :** le mode recovery démarre Ubuntu avec un accès **root** sans mot de passe. Il est donc possible de changer le mot de passe de n'importe quel utilisateur. Par défaut, le disque est monté en **lecture seule** dans ce mode, d'où la commande `mount -o remount,rw /`. Cette méthode est beaucoup plus rapide qu'une réinstallation, et elle conserve toute la configuration de la VM (Netplan, open-vm-tools).
- **Résultat attendu :** la connexion à VM01-UBU fonctionne avec le nouveau mot de passe.

### C4.2 – Vérifier la configuration avant les tests

- **Quand :** avant le premier ping.
- **Que faire :** s'assurer que les deux VMs ont la bonne IP et que les deux PC sont bien branchés sur le réseau de la salle.
- **Comment :**
  1. Vérifier que les câbles Ethernet des deux PC sont branchés.
  2. Sur VM01-UBU : `ip a` → l'adresse doit être **10.40.10.21/24**.
  3. Sur VM02-WIN : `ipconfig` → l'adresse doit être **10.40.10.22**.
- **Pourquoi :** si une VM n'a pas la bonne adresse, ou si un PC n'est pas relié au réseau, tous les tests échouent. Cette vérification évite de chercher une panne au mauvais endroit.
- **Résultat attendu :** les deux VMs ont les bonnes adresses, dans le même réseau 10.40.10.0/24.

### C4.3 – Tester la communication entre les deux VMs (T1 et T2)

- **Quand :** une fois la configuration vérifiée.
- **Que faire :** vérifier que les deux VMs se pinguent dans les deux sens.
- **Comment :**
  1. Dans le terminal de VM01-UBU :
```bash
     ping -c 4 10.40.10.22
```
  2. Dans le cmd de VM02-WIN :
```
     ping -n 4 10.40.10.21
```
  - Sous Linux, `-c 4` envoie 4 paquets (sans cette option, le ping ne s'arrête jamais). Sous Windows, c'est `-n 4`.
- **Pourquoi :** c'est le test principal du POC : une VM hébergée sur ESXi communique avec une VM hébergée sur Proxmox. On teste les deux sens, car un pare-feu peut bloquer les pings entrants d'un seul côté.
- **Résultat attendu :**
  - T1 : `4 packets transmitted, 4 received, 0% packet loss`, avec un **TTL de 128** (la machine qui répond est un Windows).
  - T2 : `Sent = 4, Received = 4, Lost = 0 (0% loss)`, avec un **TTL de 64** (la machine qui répond est un Linux).

### C4.4 – Tester l'accès aux hyperviseurs (T3 et T4)

- **Quand :** après T1 et T2.
- **Que faire :** vérifier que chaque VM atteint l'hyperviseur « d'en face ».
- **Comment :**
  1. Dans le terminal de VM01-UBU :
```bash
     ping -c 4 10.40.10.129
```
  2. Dans le cmd de VM02-WIN :
```
     ping -n 4 10.40.10.11
```
- **Pourquoi :** ces tests prouvent que les deux plateformes sont sur le même réseau physique : la VM d'ESXi voit Proxmox, et la VM de Proxmox voit ESXi.
- **Résultat attendu :** 4 réponses et 0 % de perte pour chacun des deux tests.

### C4.5 – Vérifier les tables ARP (T5 et T6)

- **Quand :** **juste après** T1 et T2, sinon les entrées ARP peuvent avoir expiré.
- **Que faire :** vérifier que chaque VM connaît l'adresse MAC de l'autre.
- **Comment :**
  1. Dans le terminal de VM01-UBU :
```bash
     ip neigh
```
  2. Dans le cmd de VM02-WIN :
```
     arp -a
```
- **Pourquoi :** avant d'envoyer un ping sur un réseau local, une machine doit connaître l'adresse MAC du destinataire. Elle la demande avec une requête **ARP** en broadcast (« qui a cette adresse IP ? »), et la réponse est gardée en mémoire dans la **table ARP**. Si l'adresse MAC de l'autre VM y apparaît, c'est qu'elles se sont trouvées directement, sans passer par un routeur.
- **Résultat attendu :**
  - VM01-UBU : une ligne `10.40.10.22 dev ens160 lladdr [adresse MAC] REACHABLE`.
  - VM02-WIN : une ligne `10.40.10.21   [adresse MAC]   dynamic`.

### C4.6 – Vérifier le nombre de sauts (T7)

- **Quand :** en dernier.
- **Que faire :** vérifier que les deux VMs sont sur le même segment réseau.
- **Comment :** dans le terminal de VM01-UBU :
```bash
  sudo apt install traceroute -y
  traceroute 10.40.10.22
```
- **Pourquoi :** traceroute affiche chaque équipement (routeur) traversé pour atteindre la destination. S'il n'y en a aucun, les deux VMs sont directement dans le même réseau, ce qui est le cas attendu puisqu'elles sont toutes les deux en 10.40.10.0/24.
- **Résultat attendu :** une seule ligne : `1  10.40.10.22  [temps] ms`.

### C4.7 – Prendre les captures et bonnes pratiques

- **Quand :** au fur et à mesure de chaque test.
- **Que faire :** conserver une preuve de chaque résultat et éviter que le problème se reproduise.
- **Comment :**
  1. Prendre une capture de chaque test **immédiatement** après l'avoir lancé.
  2. Remplir la colonne « Résultat obtenu » du tableau de l'étape C1.
  3. Noter tous les mots de passe dans un gestionnaire de mots de passe dès l'installation.
- **Pourquoi :** les configurations peuvent changer ou devenir inaccessibles après la séance, comme cela a été le cas ici. Une capture prise sur le moment reste la seule preuve fiable du résultat.
- **Résultat attendu :** le tableau de recette est entièrement rempli, avec une capture pour chaque test de T1 à T7.