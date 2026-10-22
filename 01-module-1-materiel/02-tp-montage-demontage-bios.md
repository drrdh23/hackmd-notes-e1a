# TP1 — Montage / Démontage d'un PC

*Rapport d'intervention — PC fixe (tour)*

---

## 1. Contexte et objectifs

Ce rapport documente l'intervention réalisée dans le cadre du TP1, consistant à identifier, démonter puis remonter une unité centrale (PC fixe Lenovo ThinkCentre M Series), en respectant les règles de sécurité relatives aux interventions sur matériel informatique (décharges électrostatiques, mise hors tension).

Les objectifs pédagogiques visés sont les suivants :

- Identifier chaque composant matériel et expliquer son rôle.
- Respecter les règles de sécurité (ESD, mise hors tension).
- Démonter puis remonter la machine en la laissant fonctionnelle.

## 2. Matériel utilisé

- PC fixe Lenovo ThinkCentre M Series (format SFF)
- Tournevis plat
- Surface de travail dégagée

## 3. Procédure de sécurité respectée

Avant toute intervention, j'ai vérifiée que le PC était hors tension et débranché du secteur, conformément aux règles de sécurité ESD (décharge électrostatique) et de prévention des risques électriques. Cette vérification a été effectuée avant l'ouverture du boîtier et maintenue tout au long du démontage.

## 4. Déroulement du démontage

### 4.1 Ouverture du boîtier et observation initiale

Après vérification de la mise hors tension, nous avons retiré le panneau latéral du boîtier afin d'accéder à l'intérieur de l'unité centrale. Avant de procéder au démontage proprement dit, nous avons observé l'agencement général de la machine afin de repérer l'emplacement de chaque composant (carte mère, alimentation, disque dur, lecteur optique, ventilateur), ce qui a facilité l'organisation du démontage et anticipé le remontage.

![Capture d'écran 2026-09-10 161637](https://hackmd.io/_uploads/S1Te3Vxtzl.png)


![Capture d'écran 2026-09-10 161817](https://hackmd.io/_uploads/rklBnVltfg.png)



### 4.2 Retrait du lecteur optique

Le lecteur optique (lecteur/graveur DVD) a été dégagé de son emplacement, après avoir localisé sa fixation.

![Capture d'écran 2026-09-10 161918](https://hackmd.io/_uploads/BkKd34gFfx.png)


### 4.3 Débranchement des câbles internes

L'ensemble des câbles internes a ensuite été débranché : câble d'alimentation SATA et câble de données du disque dur, connecteurs de façade (bouton d'alimentation, LED, ports USB avant), ainsi que les câbles reliant l'alimentation à la carte mère.

### 4.4 Retrait du disque dur

Le disque dur a été extrait de son support après démontage des vis de fixation. Il s'agit d'un disque Seagate Barracuda 7200.12 de 500 Go, dont les caractéristiques (référence, capacité, vitesse de rotation) ont été relevées directement sur l'étiquette du composant.

![Capture d'écran 2026-09-10 162001](https://hackmd.io/_uploads/r1Oi3Eltzx.png)


### 4.5 Observation de la carte mère et du socket processeur

Une fois le disque dur et le lecteur optique dégagés, la carte mère Lenovo a pu être observée dans son ensemble. Le socket du processeur a été identifié à ce moment.

![Capture d'écran 2026-09-10 162141](https://hackmd.io/_uploads/B12ZT4xFMx.png)


![Capture d'écran 2026-09-10 162641](https://hackmd.io/_uploads/Sk8w1rgtzl.png)






### 4.6 Retrait des barrettes de mémoire vive (RAM)

Les deux barrettes de RAM ont été retirées des slots DIMM de la carte mère, en écartant les clips de maintien latéraux avant extraction. Il s'agit de modules DDR3 Samsung de 4 Go chacun (référence PC3-12800U), dont l'étiquette a été photographiée pour identification.

![image](https://hackmd.io/_uploads/Sk7klBetzx.png)

![image](https://hackmd.io/_uploads/H1CZxHxKMe.png)


![image](https://hackmd.io/_uploads/B1ZWxrxFGl.png)



### 4.7 Retrait de la pile CMOS

Enfin, la pile CMOS a été retirée de son support sur la carte mère, en faisant glisser le clip de retenue métallique. Il s'agit d'une pile bouton CR2032 de marque KTS.

![image](https://hackmd.io/_uploads/H13HxSlFze.png)



## 5. Fiche d'identification des composants

| Composant | Rôle | Caractéristique |
|---|---|---|
| Boîtier / unité centrale | Contient et protège l'ensemble des composants | Lenovo ThinkCentre M Series, format SFF (Small Form Factor) |
| Disque dur (HDD) |Le SSD (Solid State Drive) est un support de stockage permanent sans pièce mécanique, utilisant des puces de mémoire flash (comme une grosse clé USB, mais bien plus rapide). | Seagate Barracuda 7200.12, 500 Go, 7200 tr/min, interface SATA |
| Carte mère |Le "squelette" du PC:  Interconnecte l'ensemble des composants (CPU, RAM, stockage, alimentation) | Lenovo, Rev 1.0, socket CPU de type LGA, 4 emplacements RAM (DIMM) |
| Processeur (CPU) | Le CPU (*Central Processing Unit*) est le "cerveau" du PC. Il exécute toutes les instructions et calculs nécessaires au fonctionnement des programmes et du système d'exploitation. | Absent lors de la prise de vue : socket vide avec résidus de pâte thermique visibles, preuve qu'un CPU était installé |
| Mémoire vive (RAM) — 4 modules | La RAM stocke temporairement les données que le PC utilise activement. Elle est beaucoup plus rapide que le stockage permanent, mais elle s'efface complètement à l'extinction du PC. | 4 x Samsung 4 Go DDR3 PC3-12800U (DDR3-1600), format DIMM 2Rx8 — soit 16 Go au total|
| Pile CMOS | Alimente la mémoire BIOS pour conserver la date, l'heure et les réglages hors tension | Pile bouton CR2032, lithium, marque KTS |
| Lecteur optique | Lecture et gravure de disques CD/DVD | Lecteur DVD-RW Multi Recorder, Double Couche (DL) |
| Bloc d'alimentation (PSU) | Le PSU (*Power Supply Unit*) convertit le courant électrique de la prise murale (alternatif) en courant utilisable par les composants du PC (continu), à différentes tensions. | Format compact intégré au boîtier, étiquette de danger électrique visible |
| Ventilateur | Assure le refroidissement interne du boîtier | Marque AVC, connecteur 4 broches (PWM) |

## 6. Remontage

Le remontage a été réalisé dans l'ordre inverse du démontage, à savoir :

- Remise en place de la pile CMOS sur son support.
- Réinsertion des quatres barrettes de RAM dans leurs slots DIMM, jusqu'au verrouillage des clips.
- Remise en place du disque dur et fixation par les vis d'origine à l'aide d'un tournevis.
- Rebranchement de l'ensemble des câbles internes (alimentation, données SATA, connecteurs de façade).
- Remise en place du lecteur optique dans sa baie.
- Fermeture du panneau latéral du boîtier.

Par la suite j'ai assemblé le cable VGA (bleu) qui est un connecteur DB-15 afin de le connecter à l'ecran et vérifier si le pc était fonctionnel au début le pc transmettait une alarme de sécurité cela voulait dire que nous l'avions pas bien assemblé le problème venait des RAM qui n'etait pas bien clipser, puis une fois fini l'écran c'est allumer avec succès et nous avons pu entrer dans le bioss
![image](https://hackmd.io/_uploads/H1rdrBgFMx.png)

![image](https://hackmd.io/_uploads/Bk9sSSeKfe.png)



# TP 3 — Configuration du BIOS/UEFI : mot de passe et Clear CMOS

*Rapport d'intervention — HP Envy x360*

---

## 1. Contexte et objectifs

Ce rapport documente l'intervention réalisée dans le cadre du TP consistant à sécuriser un poste via le BIOS/UEFI (définition d'un mot de passe superviseur) et à savoir intervenir en cas de blocage (mot de passe oublié) via un Clear CMOS.

L'intervention a été réalisée sur un ordinateur portable **HP Envy x360**, ce qui implique des différences importantes par rapport à la procédure standard sur PC fixe, détaillées ci-dessous.

Les objectifs pédagogiques visés sont les suivants :

- Naviguer dans le BIOS/UEFI.
- Définir puis supprimer un mot de passe BIOS.
- Comprendre la procédure de Clear CMOS, y compris ses limites sur ce type de machine.

## 2. Matériel utilisé

- PC portable HP Envy x360
- [À compléter — tournevis si tentative d'ouverture, sinon aucun matériel physique nécessaire]

## 3. Procédure de sécurité respectée

Avant toute manipulation, l'ordinateur a été correctement mis hors tension. Aucune ouverture du châssis n'a été effectuée sur cette machine personnelle.

## 4. Déroulement de l'intervention

### 4.1 Accès au BIOS/UEFI

Sur HP Envy x360, l'accès au BIOS ne se fait pas par une touche unique au démarrage comme sur d'autres marques, mais en deux temps :

1. Ordinateur doit être éteint, appuyer sur le bouton d'alimentation puis appuyer de manière répétée sur la touche "Échap" jusqu'à l'apparition du menu de démarrage (Startup Menu).
2. Depuis ce menu, appuyer sur **F10** pour ouvrir l'utilitaire de configuration du BIOS.
3. ![image](https://hackmd.io/_uploads/ryw-wSetGl.png)



### 4.2 Définition d'un mot de passe superviseur

Une fois entré dans le bioss je suis allé dans l'onglet **Sécurité**, un mot de passe superviseur (Administrator Password) a été défini.

![image](https://hackmd.io/_uploads/SJxQvSxKMx.png)
![image](https://hackmd.io/_uploads/HkjVvreFMl.png)

Après la sauvegarde en appuyant sur la touche f10 le bios a demandé ce mot de passe avant d'autoriser l'accès à l'utilitaire de configuration.
![image](https://hackmd.io/_uploads/By1tPSxtfl.png)



### 4.4 Impossibilité de réaliser le Clear CMOS physique (pile)

**Le retrait physique de la pile CMOS et son nettoyage n'ont pas pu être réalisés sur cette machine. Car le TP 1 ainsi que celui ci ont été fait sur deux jours j'ai donc préférée le faire directement sur mon pc portable**

l'ouverture du châssis nécessite le retrait de nombreuses vis sous la coque et présente un risque réel d'endommagement (nappes de connexion fragiles, absence de pièces de rechange facilement disponibles), ce qui n'était pas raisonnable à tenter sur du matériel personnel dans le cadre de ce TP.

### 4.5 Procédure qui aurait dû être suivie (si pile physique accessible)

Si la machine avait disposé d'une pile CMOS physique accessible (cas d'un PC fixe ou de certains portables réparables), la procédure aurait été la suivante :

1. Éteindre complètement l'ordinateur, débrancher le secteur, et retirer la batterie principale si elle est amovible.
2. Toucher une partie métallique non peinte du châssis pour évacuer l'électricité statique (ou utiliser un bracelet antistatique).
3. Retirer la trappe d'accès arrière (vis à dévisser avec un tournevis cruciforme adapté).
4. Localiser la pile CMOS (pile bouton ronde, généralement CR2032) sur la carte mère.
5. Retirer délicatement la pile de son support (clip à pousser sur le côté).
6. Nettoyer les contacts du support de pile ainsi que la pile elle-même avec un chiffon sec ou de l'alcool isopropylique si oxydation visible, afin d'assurer un bon contact électrique.
7. Attendre 1 à 5 minutes pour permettre la décharge complète des condensateurs résiduels.
8. Remettre la pile en place dans le bon sens (le + vers le haut).
9. Refermer la trappe, rebrancher, et vérifier au redémarrage que le BIOS est revenu aux valeurs par défaut (date/heure incorrectes, mot de passe superviseur supprimé).

### 4.6 Méthode de Clear du mot de passe effectué 

Plutôt que de réaliser un Clear CMOS (physique ou par combinaison de touches), la suppression du mot de passe superviseur a été effectuée directement depuis l'interface du BIOS, selon la procédure suivante :

1. Redémarrage du PC portable et accès au BIOS via la même méthode qu'à l'étape 4.1 (Échap puis F10).


2. Saisie du mot de passe superviseur actuellement défini, afin d'accéder à l'utilitaire de configuration.
3. Retour dans l'onglet **Security**, sur le champ de modification du mot de passe superviseur.
4. Saisie du mot de passe actuel dans le champ "Ancien mot de passe" (Old Password).
5. Le champ "Nouveau mot de passe" (New Password) a été laissé **volontairement vide**, puis validé.


![image](https://hackmd.io/_uploads/rJ1y9Betzl.png)

7. Sauvegarde des modifications et redémarrage (F10 / Save Changes and Exit), comme à l'étape 4.3.

**Constat :** après ce redémarrage, le BIOS ne demande plus de mot de passe superviseur pour y accéder — laisser le champ "nouveau mot de passe" vide a eu pour effet de supprimer complètement la protection par mot de passe, plutôt que de la remplacer par un nouveau mot de passe.

### 4.7 Comparaison avec la procédure de Clear CMOS classique

Cette méthode logicielle (suppression via le BIOS) aboutit au même résultat fonctionnel qu'un Clear CMOS — la suppression du mot de passe superviseur — mais elle est très différente :

- **Clear CMOS (physique ou par touches)** : réinitialise **l'ensemble** des paramètres du BIOS à leurs valeurs d'usine (date/heure, ordre de démarrage, mot de passe), généralement utilisé lorsqu'on a **oublié** le mot de passe et qu'on n'a donc pas d'autre moyen d'y accéder.
- **Suppression via le BIOS (méthode utilisée ici)** : ne fonctionne que si l'on **connaît déjà** le mot de passe actuel, et ne supprime que ce mot de passe, sans toucher aux autres réglages (date/heure, ordre de démarrage restent inchangés).

Cette distinction est importante car la méthode utilisée ici n'est pas une solution applicable dans le cas du "mot de passe oublié" évoqué dans le contexte du TP, puisqu'elle nécessite justement de connaître le mot de passe pour le supprimer.


## 5. Constat avant / après

les paramètres avant et après n'ont donc pas changé l'heure et la date sont restés telles qu'elles étaient.


## 7. Conclusion

Cette intervention a permis de mettre en pratique la navigation dans une interface BIOS/UEFI HP ainsi que la définition d'un mécanisme de sécurité (mot de passe superviseur). L'impossibilité de réaliser physiquement le Clear CMOS sur ce modèle a permis de mettre en évidence une différence architecturale importante entre PC fixe et ordinateur portable moderne, et la procédure alternative par combinaison de touches propre à HP a été identifiée comme substitut fonctionnel.

# TP4 — Création d'une clé USB bootable (Windows 11 et Ubuntu)

---

## 1. Contexte et objectifs

Ce rapport documente la préparation des supports d'installation nécessaires aux TP suivants (TP5 — installation Windows 11, TP6 — installation Ubuntu), via la création de deux clés USB bootables à l'aide de Rufus.

Objectifs pédagogiques :

- Créer une clé USB bootable Windows 11 avec Rufus.
- Créer une clé USB bootable Ubuntu.

## 2. Matériel utilisé

- Deux clés USB de 32GB chacune
- Logiciel Rufus 
- ISO Windows 11 : www.microsoft.com 
- ISO Ubuntu 
- PC avec accès internet

## 3. Téléchargement et vérification des ISO
1. Aller sur powershell (terminal de hp) 
2. Taper la commande : `cd $env:USERPROFILE\Downloads` pour verifier que le fichier est bien présent 
![image](https://hackmd.io/_uploads/rkgeS8gYfg.png)

3. Puis taper " Get-FileHash NOMDUFICHIER.iso -Algorithm SHA256" 
![Capture d'écran 2026-09-10 180700](https://hackmd.io/_uploads/rkQLL8gtzl.png)
4. le calcul du hash a été effectué, Cependant Microsoft ne fournit pas de valeur de référence publique pour cette ISO c'est une limite légitime

### 3.2 Téléchargement de l'ISO Ubuntu


### 3.3 Vérification du checksum

Le TP impose de vérifier l'intégrité de chaque ISO téléchargée avant utilisation.

- Méthode utilisée : sha256sum et filehash

1. installation Ubuntu.iso sur le site https://ubuntu.com/download/desktop.
2. Télécharger le fichier puis en attendant aller sur le SHA256SUM (https://releases.ubuntu.com/) pour vérifier (contrairement au W11 ubuntu fournis l'accès public de son HASH) ![image](https://hackmd.io/_uploads/rJXXOIxKzg.png)
3. Une fois Télécharger Taper la commande `dir` relever le nom du fichier ubuntu.iso
4. Taper le code Get-FileHash NOMDUFICHIER.iso -Algorithm SHA256

![image](https://hackmd.io/_uploads/r1nmKLlKzg.png)

5. Les noms sont identiques donc il s'agit bien de fichier iso verifier par SHA 256 et filehash


## 4. Création de la clé USB bootable Windows 11

### 4.1 Ouverture de Rufus et sélection de la clé
Installer l'application Rufus qui est gratuite sur windows app 
et inserer la clé USB il faut qu'elle soit détéctable sinon le fichier ne peut pas se joindre 


### 4.2 Sélection de l'image ISO

![image](https://hackmd.io/_uploads/BJPfqIxtfe.png)


### 4.3 Configuration du schéma de partition

Nous devons mettre le shcéma de répartition en format GPT et le sytème NTFS 



### 4.4 Lancement de l'écriture

![image](https://hackmd.io/_uploads/rJyicLeYfe.png)

Pour vérifier que le fichier est bien présent j'ai retirer la clé USB puis je l'ai remise le fichier était bien présent 

![image](https://hackmd.io/_uploads/H1Ns08lKGl.png)


## 5. Création de la clé USB bootable Ubuntu

### 5.1 Sélection de la clé et de l'ISO



### 5.2 Configuration du schéma de partition

Schéma **GPT** / système cible **UEFI** appliqué / 2 passes, conformément à la même contrainte que pour la clé Windows.

![image](https://hackmd.io/_uploads/BJBEkvxtMl.png)


### 5.3 Lancement de l'écriture

![image](https://hackmd.io/_uploads/Hkwu1DeYfx.png)

puis faire de même et verifier si le fichier apparaît.

![image](https://hackmd.io/_uploads/SJW0JveYMg.png)


## 6. Conclusion

Cette intervention a permis de préparer deux supports d'installation bootables fonctionnels (Windows 11 et Ubuntu) via Rufus, en respectant la contrainte d'un partitionnement GPT/UEFI, condition nécessaire aux installations prévues dans les TP5 et TP6.

## TP 5 Installation W11 

Le TP demande de vérifier le démarrage de chaque clé via le menu de boot physique (F12/F11/Échap selon le PC). Cette étape n'a pas été réalisée sur mon ordinateur personnel (HP Envy x360), pour la raison suivante :

Démarrer réellement sur la clé USB Windows 11 déclenche l'entrée dans le programme d'installation, qui nécessite ensuite un partitionnement du disque — au risque, en cas de mauvaise manipulation (mauvais disque sélectionné, table rase lancée par erreur), d'effacer le système actuellement installé sur la machine. Dans la mesure où il s'agit de mon unique PC personnel, contenant mes données de travail, je n'ai pas souhaité prendre ce risque uniquement pour valider l'affichage du menu de démarrage, d'autant que la présence confirmée des fichiers bootables sur la clé (section 1.4) constitue déjà une preuve suffisante que celle-ci est fonctionnelle et utilisable sur un poste dédié à cet effet.

C'est pour cette raison que la suite de l'installation (TP5 et TP6) a été réalisée en **machine virtuelle via VMware Workstation**, ce qui permet de valider l'ensemble de la procédure d'installation sans risque pour la machine physique.

---

### 2.1 Contexte et objectifs

Installer Windows 11 depuis l'image ISO (ou la clé USB créée en TP4) au sein d'une machine virtuelle VMware Workstation, en respectant les prérequis matériels et les contraintes de partitionnement.

### 2.2 Création de la machine virtuelle

Une nouvelle machine virtuelle a été créée dans VMware Workstation avec la configuration suivante :

- RAM allouée : [7 Go]
- Espace disque : [50 GO]
- TPM virtuel activé : [OUI]


### 2.3 Démarrage sur le support d'installation

Installation de Vmware 
La VM a été configurée pour démarrer sur l'image ISO Windows 11 (ou la clé USB montée dans la VM).


### 2.4 Partitionnement et installation

Une fois sur l'application :
- File
- New virtual Machine
- Installer disc image file (iso) entrer le fichier w11.iso télécharger au préalable (pour la clé bootable) 
- Guest operating system : Microsoft windows version W11x64
- Only files needed to support a TPM
- entrer un mdp
- Maximum disk size (GB) 50GB ( à verifier dans fichier>pc le nombre de gb restant)
- finish
- Videos : 1/2/3

<iframe src="https://satomitlearning.sharepoint.com/sites/Perso/_layouts/15/embed.aspx?UniqueId=e0d981b1-63f7-4157-b2d8-30194d63a653&embed=%7B%22ust%22%3Atrue%2C%22hv%22%3A%22CopyEmbedCode%22%7D&referrer=StreamWebApp&referrerScenario=EmbedDialog.Create" width="1280" height="720" frameborder="0" scrolling="no" allowfullscreen title="video_04.mp4"></iframe>

https://drive.google.com/drive/folders/1794TuFOV477IKZgK5YwnO6F9qjdAlpDI

### 2.5 Configuration du compte utilisateur



### 2.6 Configuration réseau et renommage du poste

Le réseau de la VM a été configuré, et le poste a été renommé conformément à la consigne.
s
- Nom du poste : [PC01_W11]
- Adresse IP obtenue (`ipconfig`) : [192.168.209.128]

![image](https://hackmd.io/_uploads/B1MYcvxKfx.png)


### 2.8 Constat

L'installation de Windows 11 en machine virtuelle a été menée à terme avec succès, aboutissant à un poste fonctionnel, à jour et configuré (réseau, nom de poste, outils VMware).

---

## TP6 : Installation d'Ubuntu (VMware Workstation)

### 3.1 Contexte et objectifs

Installer Ubuntu Desktop au sein d'une seconde machine virtuelle VMware Workstation, selon la même logique que pour Windows 11 (TP5), afin de disposer d'un second poste pour les TP réseau suivants (notamment TP7).

### 3.2 Création de la machine virtuelle

La procédure suivie a été la même que pour la VM Windows 11 : création d'une nouvelle VM dans VMware Workstation, démarrage s

### 3.3 Installation d'Ubuntu

Le type d'installation "Installation normale" a été sélectionné, avec effacement du disque virtuel (table rase).

- File
- New Virtual Machine
- Installer disc image file : fichier ubuntu.iso
- personnalisation linux
- Nommer la machine
- location (ou trouver le fichier)
- maximum Size : 30GB
- multiple files
- finish
- video 4

https://drive.google.com/drive/folders/1794TuFOV477IKZgK5YwnO6F9qjdAlpDI

### 3.4 Création du compte utilisateur

Un compte utilisateur non-root a été créé avec mot de passe, conformément à la contrainte du TP.



### 3.5 Mise à jour du système

Une fois l'installation terminée, le système a été mis à jour via le terminal :

```bash
sudo apt update && sudo apt upgrade
```

### 3.6 Relevé de l'adresse IP

```bash
ip a
```

Adresse IP obtenue : [192.168.209.130]

![image](https://hackmd.io/_uploads/B1_UIYxFGe.png)



### 3.7 Constat

L'installation d'Ubuntu en machine virtuelle a été réalisée selon la même méthodologie que pour Windows 11 : création de VM, démarrage sur le support, installation, création de compte, puis mise à jour du système et relevé réseau.

---

## Conclusion générale

Ce TP a permis de préparer deux supports d'installation bootables fonctionnels (Windows 11 et Ubuntu), confirmés par la présence effective des fichiers de démarrage sur chaque clé USB à l'issue de l'écriture avec Rufus. Le test de démarrage physique sur PC personnel n'a volontairement pas été réalisé afin de préserver l'intégrité du système en place sur cette machine, la présence des fichiers bootables constituant déjà une preuve suffisante de fonctionnalité. La validation complète de la procédure d'installation a en revanche été effectuée en environnement virtualisé (VMware Workstation), pour Windows 11 comme pour Ubuntu, aboutissant dans les deux cas à un poste fonctionnel, mis à jour et correctement configuré au niveau réseau.


# TP7 — Test de connectivité réseau entre machine virtuelle et machine physique (ping)

*Rapport d'intervention*

---

## 1. Contexte et objectifs

Ce TP consiste à vérifier que les postes installés lors des TP5 (Windows 11) et TP6 (Ubuntu), tous deux en machine virtuelle sous VMware Workstation, communiquent correctement sur le même réseau via un test de ping bidirectionnel.

Objectifs pédagogiques :

- Configurer le bon mode réseau.
- Relever les adresses IP et tester le ping dans les deux sens.
- Diagnostiquer un problème réseau simple.

## 2. Prérequis

- VM Windows 11 installée et fonctionnelle (TP5).
- VM Ubuntu installée et fonctionnelle (TP6).
- VMware Workstation.

## 3. Relevé des adresses IP

### 3.1 VM Windows 11

Commande utilisée :ipconfig: 

Adresse IPv4 relevée : [192.168.209.128]


### 4.2 VM Ubuntu

Commande utilisée :

```bash
ip a
```

Adresse IP relevée : [192.168.209.128]

![Capture d'écran 2026-09-10 193552](https://hackmd.io/_uploads/rJTZXFltfl.png)



### 4.3 Vérification de l'appartenance au même sous-réseau

Les deux adresses relevées appartiennent au même sous-réseau (mêmes premiers blocs d'adresse selon le masque, ex. `/24` → 3 premiers blocs identiques), confirmant que les deux VM sont bien positionnées sur le même réseau local.

## 5. Test de ping — Windows vers Ubuntu

Commande exécutée depuis la VM Windows 11 : ping: 

![image](https://hackmd.io/_uploads/Sy8zLKetzx.png)

 Windows-> Ubuntu à bien fonctionner 

## 6. Test de ping — Ubuntu vers Windows

Commande exécutée depuis la VM Ubuntu :

```bash
ping <IP_Windows>
```

![image](https://hackmd.io/_uploads/Hk6qIYeYMg.png)



## 7. Diagnostic (en cas d'échec initial)

Un premier test de ping depuis Ubuntu vers Windows peut échouer par défaut (timeout, "Request timed out"), sans que cela signifie que les deux machines ne sont pas sur le même réseau. La cause la plus fréquente est le **pare-feu Windows Defender**, qui bloque par défaut les requêtes ICMP entrantes (le ping), même lorsque les deux machines sont correctement configurées en réseau.




### 7.1 Vérification et correction appliquée

Pour autoriser le ping entrant sur la VM Windows, la règle de pare-feu correspondante a été activée :

- Via l'interface graphique : Pare-feu Windows Defender avec fonctions avancées de sécurité > Règles de trafic entrant > activation des règles "Fichiers et imprimantes partagés (requête d'écho - ICMPv4-In)". clic droit activer la règle

![IMG_9938](https://hackmd.io/_uploads/ryWC0DeKGe.jpg)



![image](https://hackmd.io/_uploads/HJ91yOetfg.jpg)




## TP 2 Configuration PC Gamer 
Mon Client est une personne qui débute dans le gaming via le pc et à un budget de 2500CHF grâce au site pc part picker je vais lui créer un pc abordable en respectant son budget

## Fiche de configuration

| Composant | Référence | Prix indicatif |
|---|---|---|
| CPU | AMD Ryzen 5 7600 (6 cœurs / 12 threads, socket AM5, 65 W TDP) | 163,00 CHF |
| Refroidisseur CPU | Ventirad stock fourni avec le Ryzen 5 7600 | 0 CHF (inclus) |
| Carte mère | MSI PRO B850M (AM5, DDR5, ATX/mATX) | 189,99 CHF |
| Mémoire (RAM) | 32 Go DDR5-6000 (2x16 Go) — ex. Corsair Vengeance RGB CL36 | 489,99 CHF | | Stockage | SSD NVMe PCIe 4.0, 1 To (ex. Crucial P3Plus) |~150 CHF |
| Carte graphique (GPU) | MSI SHADOW GeForce RTX 5070 12 Go | 917,69 CHF |
| Boîtier ("affaire") | Boîtier ATX Mid Tower, bon airflow (ex. Montech AIR 903 MAX) | 79,98CHF |
| Alimentation électrique (PSU) | 750 W, 80+ Gold, modulaire | 76,98 CHF |
| Système d'exploitation | Windows 11 Famille via usb 64bits | 128,99 CHF |
| Moniteur | Écran 27" 1440p, 240 Hz, IPS | 219,99 CHF |
| **Total estimé** | | **2266,51** |

## Tableau de vérification de compatibilité

| Vérification | Détail |
|---|---|
| Socket CPU ↔ carte mère | AM5 ↔ AM5  |
| Refroidisseur ↔ socket | Ventirad stock compatible AM5 (TDP 65 W dans sa capacité) |
| Type RAM ↔ carte mère | DDR5 ↔ DDR5 |
| Stockage ↔ carte mère | SSD M.2 NVMe PCIe 4.0 ↔ slot M.2 PCIe 4.0 de la carte mère  |
| GPU ↔ carte mère | PCIe x16 ↔ slot PCIe x16  |
| Carte mère ↔ boîtier | Format ATX/mATX ↔ boîtier ATX Mid Tower  |
| GPU ↔ boîtier | Longueur RTX 5070 (~300 mm) ↔ espace disponible dans le boîtier  (à vérifier sur la fiche du boîtier) |
| Connecteurs alimentation | 24 broches (carte mère), 8 broches EPS (CPU), 1x 16 broches ou 2x 8 broches PCIe (GPU selon modèle) ↔ fournis par le PSU 750W |
| OS ↔ matériel | Windows 11 nécessite TPM 2.0 + Secure Boot ↔ natifs sur carte mère AM5/chipset B650 |
| Moniteur ↔ GPU | DisplayPort/HDMI 2.1 ↔ sorties vidéo présentes sur la RTX 5070  |

## Calcul du TDP total et marge de l'alimentation

| Composant | Consommation estimée |
|---|---|
| CPU | 65 W |
| Carte mère + RAM + ventilateurs boîtier | ~40 W |
| SSD NVMe | ~8 W |
| GPU (RTX 5070) | ~220 W |
| **TDP total estimé** | **~330 W** |

**Alimentation choisie :** 750 W → marge d'environ **420 W**, largement suffisante pour absorber les pics de charge transitoires (particulièrement importants sur GPU haut de gamme) et permettre une évolution future.

## Argumentaire — pourquoi ces choix

**CPU — AMD Ryzen 5 7600 :** pour un usage gaming, le GPU est le composant déterminant pour les performances, pas le CPU. Un Ryzen 5 6 cœurs/12 threads est largement suffisant pour ne pas brider la RTX 5070 dans la quasi-totalité des jeux actuels, ce qui permet de réorienter le budget vers le GPU plutôt que vers un CPU plus cher et peu utile ici.

**Refroidisseur CPU — Ventirad stock :** le TDP de 65 W du 7600 est modéré, le ventirad fourni de série suffit largement à le refroidir sous charge gaming, sans nécessité d'investir davantage sur ce poste.

**Carte mère — MSI PRO B850 :** compatible avec le socket AM5 du CPU et la DDR5, chipset B650 suffisant pour ce niveau de composants.

**Mémoire — 32 Go DDR5-6000 :** capacité confortable pour du gaming moderne avec applications en arrière-plan (Discord, navigateur, overlay de streaming), et la fréquence 6000 MHz correspond au point optimal de performance pour les CPU AMD AM5 (sweet spot officiel constructeur).

**Stockage — SSD NVMe 1 To :** les jeux AAA récents pèsent de plus en plus lourd (certains dépassent 100-150 Go), donc 1 To offre une capacité confortable pour une bibliothèque de jeux, avec des temps de chargement rapides grâce au NVMe PCIe 4.0.

**Carte graphique — RTX 5070 :** c'est le composant central de cette configuration gaming. Elle offre un excellent niveau de performance en 1440p avec les paramètres graphiques élevés, tout en restant dans un budget raisonnable comparé aux modèles encore plus haut de gamme (RTX 5080/5090), dont le surcoût n'est pas justifié pour un usage 1440p standard.

**Boîtier — ATX Mid Tower :** format imposé par la carte mère (ATX/mATX), avec un bon flux d'air privilégié pour dissiper la chaleur générée par le GPU sous charge prolongée en jeu.

**Alimentation — 750 W 80+ Gold :** dimensionnée avec une marge large (~420 W) par rapport à la consommation réelle estimée (~330 W), pour absorber les pics de consommation transitoires typiques des GPU modernes sous charge, et anticiper une éventuelle mise à niveau future du GPU.

**Système d'exploitation — Windows 11 :** retenu pour sa compatibilité native avec la quasi-totalité des jeux et systèmes anti-triche actuels, contrairement à Linux qui reste plus limité sur ce point pour du gaming. Le chipset B650 intègre nativement le TPM 2.0 et le Secure Boot requis, sans contournement nécessaire.

**Moniteur — 27" 1440p 240 Hz :** résolution et taux de rafraîchissement cohérents avec la puissance de la RTX 5070 MAX, qui est justement calibrée pour offrir de bonnes performances en 1440p à fréquence élevée — un moniteur Full HD serait sous-exploité, et la 4K nécessiterait un GPU plus puissant.

![image](https://hackmd.io/_uploads/By5pUOgKGg.png)

Le budget à bien été respecter ainsi il lui reste même de l'argent pour pouvoir s'acheter un clavier et une souris