### Projet 1 — Poste de travail pour un cabinet comptable

***Cours de référence :*** 
IT Essentials — Mettre en service un poste de travail ICT avec le système d’exploitation Classe : E1A Niveau : Débutant Durée : 1 semaine (PPE)

**Contexte**
Un petit cabinet comptable de 3 personnes vous engage pour mettre en service le poste de travail de sa nouvelle collaboratrice, prêt à l’emploi dès le lundi matin.

**Objectifs pédagogiques** : 
- Installer un système d’exploitation sur une machine physique ou virtuelle.
- Créer un compte utilisateur standard avec une politique de mot de passe simple.
- Installer les logiciels bureautiques de base.
- Configurer l’accès à une imprimante partagée sur le réseau local.
- Appliquer les premières mesures de sécurité (mises à jour, antivirus, pare-feu).
- Consignes / Étapes
- Installer le système d’exploitation depuis un support fourni.
- Créer un compte administrateur local et un compte utilisateur standard.
- Configurer le réseau (IP, nom de machine, groupe de travail).
- Installer les logiciels métier demandés par le formateur.
- Ajouter une imprimante réseau partagée et tester une impression.
- Vérifier l’activation des mises à jour, de l’antivirus et du pare-feu.
- Rédiger une fiche de mise en service.
- Livrables attendus
- Le poste opérationnel présenté au formateur.
- Une fiche de mise en service (Markdown/PDF).
Captures d’écran des étapes clés (compte, logiciel, imprimante, mises à jour).

# 1. INSTALLATION W11

Installer Windows 11 depuis l'image ISO au sein d'une machine virtuelle VMware Workstation, en respectant les prérequis matériels et les contraintes de partitionnement.

## 1.1 Téléchargement du fichier iso.

# Télécharger l'image ISO de Windows 11
- Ouvrir le site officiel Microsoft
- Cliquez sur ce lien pour ouvrir directement la page officielle : - [https://www.microsoft.com/fr-fr/software-download/windows11](https://www.microsoft.com/fr-fr/software-download/windows11)
- La page "Télécharger Windows 11" de Microsoft s'affiche
- Sur cette page, il y a plusieurs sections (outil de création de support, mise à niveau, etc.)
- Cherchez le titre **"Télécharger l'image disque Windows 11 (ISO)"** c'est la 3ᵉ section normalement, après "Mettre à niveau vers Windows 11" et "Créer un support d'installation Windows 11"
- Cliquez sur le bouton bleu **"Télécharger"** juste à côté du menu déroulant
- La page se recharge légèrement, un nouveau menu déroulant apparaît, intitulé "Choisissez la langue du produit"
- Sélectionnez **"Français"** dans la liste
4- Télécharger 



## 1.2 Téléchargement et vérification des ISO

Aller sur powershell (terminal de hp)

Taper la commande : `cd $env:USERPROFILE\Downloads` pour verifier que le fichier est bien présent

![image](https://hackmd.io/_uploads/rkgeS8gYfg.png)
Puis taper " Get-FileHash NOMDUFICHIER.iso -Algorithm SHA256"

 ![image](https://hackmd.io/_uploads/rJXXOIxKzg.png)
 
le calcul du hash a été effectué, Cependant Microsoft ne fournit pas de valeur de référence publique pour cette ISO c'est une limite légitime








## 2.1 Création de la machine virtuelle

Une nouvelle machine virtuelle a été créée dans VMware Workstation avec la configuration suivante :

RAM allouée : [7 Go]
Espace disque : [50 GO]
TPM virtuel activé : [OUI]
2.3 Démarrage sur le support d'installation
Installation de Vmware
La VM a été configurée pour démarrer sur l'image ISO Windows 11 (ou la clé USB montée dans la VM).

## 2.2 Partitionnement et installation
Une fois sur l'application :

- File
- New virtual Machine
- Installer disc image file (iso) entrer le fichier w11.iso télécharger au préalable (pour la clé bootable)
- Guest operating system : Microsoft windows version W11x64
- Only files needed to support a TPM
entrer un mdp
- Maximum disk size (GB) 50GB ( à verifier dans fichier>pc le nombre de gb restant)
- finish

Vidéo : 1-2.
https://drive.google.com/file/d/1YXnP-W4yQiFuauio7qhiH-O7-J6IdG7W/view?usp=drive_link


### 2.3 Configuration du compte utilisateur



### 2.4 Configuration réseau et renommage du poste

Le réseau de la VM a été configuré, et le poste a été renommé conformément à la consigne.

- Nom du poste : [PC01_DR]
- Adresse IP obtenue (`ipconfig`) : [192.168.209.128]

![image](https://hackmd.io/_uploads/B1MYcvxKfx.png)


---

## 3. Installer le système d'exploitation (depuis l'ISO)

1. VM sélectionnée → cliquez **"Démarrer"** (flèche verte)
2. Si "Press any key to boot from CD or DVD" apparaît → cliquez dans la fenêtre et appuyez sur une touche immédiatement
3. Écran "Installation de Windows" : Langue/Format/Clavier = **Français** → **Suivant**
4. Cliquez **"Installer maintenant"**
5. **"Je n'ai pas de clé de produit"**
6. Sélectionnez **"Windows 11 Pro"** → **Suivant**
7. Cochez **"J'accepte les termes du contrat de licence"** → **Suivant**
8. Cliquez **"Personnalisée : installer uniquement Windows (avancé)"**
9. Cliquez sur **"Lecteur 0 Espace non alloué"** → **Suivant**
10. L'installation copie les fichiers et redémarre plusieurs fois seule (15-25 min, patience)
Vidéo 3.
https://drive.google.com/file/d/1YXnP-W4yQiFuauio7qhiH-O7-J6IdG7W/view?usp=drive_link


### 3.1 Contourner le compte Microsoft obligatoire
1. Écran **"Est-ce la bonne région ?"** → **Oui**
2. Écran clavier → **Oui**
3. **"Ajouter une deuxième disposition ?"** → **Ignorer**
4. Sur l'écran réseau, appuyez **Maj + F10** → une invite de commandes s'ouvre
5. Tapez :    → Entrée → la VM redémarre seule
6. De retour sur l'écran réseau → cliquez **"Je n'ai pas de connexion Internet"**
7. Cliquez **"Continuer avec une configuration limitée"**
8. **"Qui va utiliser cet appareil ?"** : tapez `Admin` → **Suivant**
9. Créez un mot de passe (min. 8 caractères) → **Suivant** → confirmez → **Suivant**
10. Répondez aux 3 questions de sécurité → **Suivant** à chaque fois
11. Sur les écrans de confidentialité, basculez tout sur **Non** → **Suivant**
12. Ignorez la proposition d'applications si elle apparaît
13. Attendez la finalisation (2-5 min) → le Bureau Windows 11 apparaît

 Vidéo 4. 
https://drive.google.com/file/d/1YXnP-W4yQiFuauio7qhiH-O7-J6IdG7W/view?usp=drive_link

---

##  Créer un compte utilisateur standard

Le compte `Admin` créé pendant l'installation est déjà administrateur. Reste à créer le compte standard.

1. **Démarrer** → **Paramètres** (`Windows + I`) → **Comptes** → **Autres utilisateurs**
2. Cliquez **"Ajouter un compte"**
3. Cliquez **"Je ne dispose pas des informations de connexion de cette personne"**
4. Cliquez **"Ajouter un utilisateur sans compte Microsoft"**
5. Tapez `Collaboratrice1` → **Suivant**
6. Créez un mot de passe (min. 8 caractères) → **Suivant** → confirmez → **Suivant**
7. Répondez aux 3 questions de sécurité → **Suivant**
8. Sur la page "Autres utilisateurs", cliquez sur **Collaboratrice1** → **"Modifier le type de compte"**
9. Vérifiez que le menu affiche **"Utilisateur standard"** → **OK**

Vidéo 5. 
https://drive.google.com/file/d/1YXnP-W4yQiFuauio7qhiH-O7-J6IdG7W/view?usp=drive_link




---

## Installer les logiciels métier

*(à défaut de liste précise du formateur, LibreOffice convient parfaitement)*

1. Ouvrez Edge → tapez `libreoffice.org/download` → Entrée
2. Cliquez sur le bouton vert **"Télécharger"**
3. Ouvrez le fichier `.msi` téléchargé → **Oui** (contrôle de compte)
4. Cliquez **Suivant** sur chaque écran (installation "Typique") → **Installer** → **Terminer**
5. **Démarrer**, tapez `LibreOffice Calc` → Entrée pour vérifier que ça s'ouvre

Vidéo 10.
https://drive.google.com/file/d/1YXnP-W4yQiFuauio7qhiH-O7-J6IdG7W/view?usp=drive_link


---

##  Ajouter une imprimante réseau partagée et tester une impression

**Sur le PC HÔTE (votre vrai PC, pas la VM) :**
1. **Paramètres** → **Bluetooth et appareils** → **Imprimantes et scanners**
2. Cliquez sur **"Microsoft Print to PDF"** ou **"Microsoft XPS Document Writer"**
3. **"Propriétés de l'imprimante"** → onglet **"Partage"**
4. Cochez **"Partager cette imprimante"**, nommez-la `ImpCompta` → **Appliquer** → **OK**
5. Notez le nom du PC hôte : **Paramètres** → **Système** → **À propos** → "Nom de l'appareil"

**Dans la VM :**
6. **Paramètres** → **Bluetooth et appareils** → **Imprimantes et scanners** → **"Ajouter un appareil"**
7. Cliquez **"L'imprimante que je veux n'est pas répertoriée"**
8. Sélectionnez **"Sélectionner une imprimante partagée par nom"**
9. Tapez :

Vidéo 6 
https://drive.google.com/file/d/1YXnP-W4yQiFuauio7qhiH-O7-J6IdG7W/view?usp=drive_link

##  Groupe de travail
1. **Démarrer**, tapez `panneau de configuration` → Entrée
2. **Système et sécurité** → **Système** → **"Paramètres système avancés"**
3. Onglet **"Nom de l'ordinateur"** → **"Modifier..."**
4. Sélectionnez **"Groupe de travail"**, tapez `CABINET-COMPTA` → **OK**
5. Confirmez les messages → **"Redémarrer maintenant"**

Vidéo 10.
https://drive.google.com/file/d/1YXnP-W4yQiFuauio7qhiH-O7-J6IdG7W/view?usp=drive_link

##  Vérification du pare-feu, configuration réseau (IP statique) et mise à jour Windows 11

**Contexte :** poste Windows 11 (machine virtuelle VMware, nom ` khk`
**Objectif :** vérifier la protection (pare-feu/antivirus), sauvegarder la stratégie de sécurité, passer le poste en adresse IP fixe, puis mettre Windows à jour.

---

## Étape 1 — Ouvrir le Pare-feu Windows Defender

1. Cliquer sur la barre de recherche Windows (barre des tâches).
2. Taper **« pare feu »**.
3. Dans les résultats, sous **Applications**, cliquer sur **Pare-feu Windows Defender**.

->  *But : accéder à l'outil de gestion du pare-feu pour en contrôler l'état.*

---

## Étape 2 — Ouvrir la console avancée et consulter la vue d'ensemble

1. Depuis le panneau Pare-feu Windows Defender, ouvrir **« Pare-feu Windows Defender avec fonctions avancées de sécurité »**.
2. Sur la page d'accueil (nœud « …sur Ordinateur local »), lire la **Vue d'ensemble** :
   - **Profil de domaine** : actif
   - **Profil privé** : actif
   - **Profil public** : actif
   - Pour chaque profil : connexions entrantes non conformes = **bloquées**, connexions sortantes = **autorisées**.


![Capture d'écran 2026-09-14 155312](https://hackmd.io/_uploads/ryJtc3Htfg.png)

---

## Étape 3 — Vérifier la protection antivirus

1. Ouvrir **Sécurité Windows** (depuis le menu Démarrer ou la recherche).
2. Aller dans **Protection contre les virus et menaces**.
3. Vérifier : « Aucune menace actuelle ».

Vidéo 7

https://drive.google.com/file/d/1YXnP-W4yQiFuauio7qhiH-O7-J6IdG7W/view?usp=drive_link

---

## Étape 4 — Exporter (sauvegarder) la stratégie du pare-feu

1. Revenir dans **Pare-feu Windows Defender avec fonctions avancées de sécurité**.
2. Dans le volet **Actions** (à droite), cliquer sur **Exporter la stratégie…**
3. Choisir l'emplacement (ex. *Documents*), donner un nom de fichier.
4. Le fichier est enregistré au format **`.wfw`**.

Vidéo 10.

https://drive.google.com/file/d/1YXnP-W4yQiFuauio7qhiH-O7-J6IdG7W/view?usp=drive_link

---

## Étape 5 — Ouvrir l'invite de commandes en administrateur

1. Barre de recherche Windows → taper **« cmd »**.
2. Sur le résultat **Invite de commandes**, cliquer sur **Exécuter en tant qu'administrateur**.

->  *But : disposer des droits nécessaires pour consulter/modifier la configuration réseau.*

Vidéo 10. https://drive.google.com/file/d/1YXnP-W4yQiFuauio7qhiH-O7-J6IdG7W/view?usp=drive_link

---

## Étape 6 — Relever la configuration IP actuelle

1. Dans l'invite de commandes, taper : 2. Noter les valeurs affichées, par exemple :
   - Adresse IPv4 : `192.168.208.129`
   - Masque de sous-réseau : `255.255.255.0`
   - Passerelle par défaut : `192.168.208.2`

-> *But : connaître l'adresse IP attribuée automatiquement (DHCP) avant de la remplacer par une adresse fixe, pour rester sur le même sous-réseau.*

---

## Étape 7 — Accéder aux propriétés de la carte réseau

1. Ouvrir le **Panneau de configuration** → **Réseau et Internet** → **Centre Réseau et partage**.
2. Dans le menu de gauche, cliquer sur **Modifier les paramètres de la carte**.
3. Double-cliquer sur la carte **Ethernet0** → **Propriétés**.
4. Sélectionner **Protocole Internet version 4 (TCP/IPv4)** → cliquer sur **Propriétés**.

-> *But : ouvrir la fenêtre de configuration IP de la carte réseau.*
Vidéo 8
https://drive.google.com/file/d/1YXnP-W4yQiFuauio7qhiH-O7-J6IdG7W/view?usp=drive_link

---

## Étape 8 — Configurer une adresse IP statique

1. Cocher **Utiliser l'adresse IP suivante**.
2. Renseigner :
   - **Adresse IP** : une adresse **dans le même sous-réseau** que celui relevé à l'étape 6 (ex. `192.168.209.140`)
   - **Masque de sous-réseau** : `255.255.255.0`
   - **Passerelle par défaut** : `192.168.208.2` (identique à celle relevée à l'étape 6)
3. Cliquer sur **OK**, puis **Fermer**.



 -> *But : figer l'adresse IP du poste (au lieu d'une attribution automatique par DHCP), utile par exemple pour un poste serveur ou tout équipement devant garder une adresse stable. L'adresse et la passerelle doivent impérativement appartenir au même sous-réseau que le réseau local, sinon la connexion est perdue.*

---

## Étape 9 — Vérifier la nouvelle configuration

1. Retourner dans l'invite de commandes et retaper :    OU
2. Dans **Centre Réseau et partage**, cliquer sur la connexion **Ethernet0** → vérifier l'**État de Ethernet0** (Connectivité IPv4, octets envoyés/reçus).

-> *But : confirmer que la nouvelle adresse IP statique est bien appliquée et que l'accès réseau/Internet fonctionne.*



---

## Étape 10 — Mettre Windows à jour

1. Ouvrir **Paramètres** (⊞ Win + I) → **Système**.
2. Cliquer sur la tuile **Windows Update** (message « votre attention est requise »).
3. Cliquer sur **Rechercher des mises à jour** puis **Tout télécharger et installer**.
4. Attendre la fin du téléchargement/installation.
5. Le PC redémarre automatiquement (« Redémarrage en cours »).

->  *But : appliquer les correctifs de sécurité et mises à jour du système.*

Vidéo 9.

https://drive.google.com/file/d/1YXnP-W4yQiFuauio7qhiH-O7-J6IdG7W/view?usp=drive_link

---

## Étape 11 — Confirmer que le système est à jour

1. Après le redémarrage, rouvrir **Paramètres → Windows Update**.
2. Cliquer sur **Rechercher des mises à jour**.
3. Vérifier le message **« Vous êtes à jour »**.

-> *But : valider que toutes les mises à jour disponibles ont bien été installées.*
![image](https://hackmd.io/_uploads/HJpaBTrtzg.png)

---

Vidéo résumer de tout (10)
https://drive.google.com/file/d/1YXnP-W4yQiFuauio7qhiH-O7-J6IdG7W/view?usp=drive_link

---
