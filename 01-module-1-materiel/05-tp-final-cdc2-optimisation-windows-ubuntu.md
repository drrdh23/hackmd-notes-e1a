### CAHIER DES CHARGE 2 - Déploiement  des systèmes d'exploitation

**Contexte** : Le client a besoin de deux postes de travail opérationnels : un poste sous Windows 10 Pro et un poste sous Ubuntu.

**Partie A — Windows 10 Pro**
Installer Windows 10 Pro (partitionnement GPT/UEFI).
Configuration minimale du système : nom du poste (convention à définir), compte utilisateur avec mot de passe sécurisé, langue/clavier FR-CH, fuseau horaire Europe/Zurich, connexion réseau fonctionnelle.
Mises à jour Windows Update effectuées.
Configuration minimale et optimisation du PC : désactivation des programmes inutiles au démarrage, plan d'alimentation adapté à l'usage, pare-feu et Windows Defender actifs, nettoyage de disque, point de restauration système créé.
Vérification finale à documenter : winver, ipconfig, liste des programmes au démarrage (gestionnaire des tâches).

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
- Installer disc image file (iso) entrer le fichier w10.iso télécharger au préalable (pour la clé bootable) 
- Guest operating system : Microsoft windows version W11x64
- Only files needed to support a TPM
- entrer un mdp
- Maximum disk size (GB) 50GB ( à verifier dans fichier>pc le nombre de gb restant)
- finish
- vidéo 2.

https://drive.google.com/drive/folders/14gx6_N0ZUeWy4AGf9eHx4CSsGYhzRpkb

### 2.5 Configuration du compte utilisateur
- gérer les parametre 
- langue clavier FR-CH, fuseau horaire Europe/Zurich
- Mise à Jour Windows Update effectuées
- vidéo 1. 

https://drive.google.com/drive/folders/14gx6_N0ZUeWy4AGf9eHx4CSsGYhzRpkb

### 2.6 Configuration réseau et renommage du poste

Le réseau de la VM a été configuré, et le poste a été renommé conformément à la consigne.

- Nom du poste : [PC01_W10]
- Adresse IP obtenue (`ipconfig`) : [192.168.209.134]


![image](https://hackmd.io/_uploads/SkkbXTZKzx.png)

### 2.7 Configuration minimale et optimisation du PC

**Date :** 11/09/2026
**Machine :** VM (machine virtuelle)

---

## 1. Désactivation des programmes inutiles au démarrage

### Étapes réalisées

1. Clic droit sur la barre des tâches → **Gestionnaire des tâches** (ou `Ctrl + Maj + Échap`).
2. Clic sur Plus de détails (si mode simplifié).
3. Onglet Démarrage
4. Analyse des programmes listés : Windows Security,Microsoft OneDrive, Microsoft Edge.

![Capture d'écran 2026-09-11 201524](https://hackmd.io/_uploads/ByZRIsztGe.png)
![Capture d'écran 2026-09-11 201458](https://hackmd.io/_uploads/rJzR8szYzx.png)


### Décisions

| Programme | Statut appliqué | Justification |
|---|---|---|
| Windows Security | Activé | Sécurité système essentielle, ne doit jamais être désactivé |
| Microsoft OneDrive | Désactivé | Non indispensable au démarrage, synchronisation lançable manuellement si besoin |
| Microsoft Edge (startup boost) | Désactivé | Préchargement non essentiel, n'empêche pas l'utilisation normale du navigateur |

### Remarque sur la colonne "Impact au démarrage"

La colonne **Impact au démarrage** affiche **"Non mesuré"** pour l'ensemble des programmes. Ceci s'explique par le fait que Windows calcule habituellement cet indicateur à partir de mesures réelles de temps de démarrage collectées sur plusieurs sessions, ce que l'environnement virtualisé (VM) ne permet pas de mesurer correctement. La décision de désactivation s'est donc basée sur la **pertinence fonctionnelle** de chaque programme plutôt que sur l'impact mesuré.


---

## 2. Plan d'alimentation adapté à l'usage

### Étapes réalisées

1. **Paramètres** → **Système** → **Alimentation et batterie**.
2. Sélection du **Mode d'alimentation**.
3. Choix retenu : **Économie d'énergie / Meilleure efficacité énergétique**.


![Capture d'écran 2026-09-11 202710](https://hackmd.io/_uploads/S1_4DjMKze.png)


![Capture d'écran 2026-09-11 201743](https://hackmd.io/_uploads/H1ONwoGKfe.png)
![Capture d'écran 2026-09-11 201923](https://hackmd.io/_uploads/SJOVwozKMe.png)




### Justification

Le mode d'alimentation Économie d'énergie a été sélectionné car la machine est utilisée ponctuellement (une seule session), sans besoin de performances soutenues. Ce mode limite la consommation en réduisant la fréquence du processeur, ce qui est suffisant pour l'usage prévu.



---

## 3. Pare-feu et Windows Defender

### Étapes réalisées

1. Paramètres → Confidentialité et sécurité → Sécurité Windows.
2. Vérification de Pare-feu et protection réseau (profils Domaine/Privé/Public).
3. Vérification de Protection contre les virus et menaces.
4. Test en PowerShell (administrateur) avec la commande :

\`\`\`powershell
Get-MpComputerStatus
\`\`\`

![Capture d'écran 2026-09-11 202103](https://hackmd.io/_uploads/Hk_4PjGKGg.png)
![IMG_9938](https://hackmd.io/_uploads/ryWC0DeKGe.jpg)



![image](https://hackmd.io/_uploads/HJ91yOetfg.jpg)




### Constat

La **protection en temps réel** n'est pas visible sur cette machine.




---

## 4. Nettoyage de disque

### Étapes réalisées

1. Recherche Windows → **Nettoyage de disque**.
2. Sélection du lecteur **C:**.
3. Analyse et affichage de l'espace récupérable.
4. Clic sur **Nettoyer les fichiers système** pour inclure les fichiers de mise à jour Windows.
5. Sélection des catégories à supprimer (fichiers temporaires, corbeille, fichiers journaux, miniatures, etc.).
6. Validation avec **Supprimer les fichiers**.

### Résultat

- Espace gagné 13,1 Mo


![Capture d'écran 2026-09-11 202836](https://hackmd.io/_uploads/HkdNwofYfg.png)

---

## 5. Point de restauration système

### Étapes réalisées

1. Recherche Windows → **Créer un point de restauration**.
2. Ouverture de la fenêtre **Propriétés système**, onglet **Protection du système**.
3. Vérification que le lecteur **C:** a la protection **Activée**.
4. Clic sur **Créer...**.
5. Nom donné au point : deaa



![Capture d'écran 2026-09-11 203809](https://hackmd.io/_uploads/H1_NPiMKGl.png)
![Capture d'écran 2026-09-11 203736](https://hackmd.io/_uploads/HkdVvoztMx.png)

## 6. Vérification finale

### Commandes exécutées

| Commande | Objectif |
|---|---|
| `winver` | Affiche la version, le build et l'édition de Windows |
| `ipconfig` | Affiche la configuration réseau (adresse IP, passerelle, DNS) |

### Résultats

**winver :**

Vidéo 5.
https://drive.google.com/drive/folders/14gx6_N0ZUeWy4AGf9eHx4CSsGYhzRpkb

### Programmes au démarrage (état final)

Liste finale après optimisation, vérifiée via **Gestionnaire des tâches → onglet Démarrage**.


---
## Installation d'Ubuntu (VMware Workstation)

**1.1 Contexte et objectifs** 

Installer Ubuntu Desktop au sein d'une seconde machine virtuelle VMware Workstation, selon la même logique que pour Windows 10, afin de disposer d'un second poste.

**1.2 Création de la machine virtuelle**

La procédure suivie a été la même que pour la VM Windows 11 : création d'une nouvelle VM dans VMware Workstation, démarrage.

**1.3 Installation d'Ubuntu** 
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
- Vidéo 3.


https://drive.google.com/drive/folders/14gx6_N0ZUeWy4AGf9eHx4CSsGYhzRpkb

**1.4 Création du compte utilisateur**
Un compte utilisateur non-root a été créé avec mot de passe, conformément à la contrainte du TP.
- Langue : Français
- Clavier : Suisse
- Disposition : France - Suisse
- fuseau horaire : Europe / Zurich
- MAJ

https://drive.google.com/drive/folders/14gx6_N0ZUeWy4AGf9eHx4CSsGYhzRpkb

**1.5 Mise à jour du système** 

Une fois l'installation terminée, le système a été mis à jour via le terminal :

3.6 Relevé de l'adresse IP
ip a
Adresse IP obtenue : [192.168.209.129]

![image](https://hackmd.io/_uploads/rkU5eAWtzx.png)



3.7 Constat
L'installation d'Ubuntu en machine virtuelle a été réalisée selon la même méthodologie que pour Windows 10 : création de VM, démarrage sur le support, installation, création de compte, puis mise à jour du système et relevé réseau, protection des données, activation des pare-feux.



Ping ![image](https://hackmd.io/_uploads/HJI1bRZKzg.png)


![image](https://hackmd.io/_uploads/HkRZWCbFzg.png)
