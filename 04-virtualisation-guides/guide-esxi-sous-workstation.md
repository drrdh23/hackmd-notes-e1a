# TP VMware ESXi sous VMware Workstation (PC portable) — Guide détaillé

## 0. Prérequis

1. Va sur `https://support.broadcom.com/group/ecx/productdownloads?subfamily=VMware%20Workstation%20Pro&freeDownloads=true`
3. Crée un compte Broadcom Support Portal (gratuit) si tu n'en as pas, connecte-toi
4. Accepte les CGU + le formulaire "Trade Compliance" (infos exactes, sinon délai de validation)
5. Télécharge **VMware Workstation Pro 17.5.2 ou supérieur** (.exe, ~700 Mo)
6. Sur le même portail, cherche "VMware vSphere Hypervisor (ESXi)" → télécharge l'ISO **ESXi 8.0 Update 3e** (~500 Mo, fichier `.iso`, pas le `.exe` ni vCenter)
7. Vérifie que ton laptop a bien la virtualisation activée dans son propre BIOS (VT-x/AMD-V) — sinon Workstation refusera de créer une VM 64 bits

## 1. Installer VMware Workstation Pro

1. Double-clique sur le .exe téléchargé
2. Assistant → **Next**
3. Accepte le contrat de licence → coche la case → **Next**
4. Emplacement d'installation → laisse par défaut → **Next**
5. Décoche "Check for product updates" et "Help improve..." si tu veux → **Next**
6. **Next** → **Install** → attends la fin (quelques minutes)
7. Écran de licence : clique **"Finish"** sans rien saisir dans le champ clé de licence → c'est la version gratuite pour usage personnel
8. Redémarre le PC si demandé

## 2. Créer la VM ESXi dans Workstation

1. Ouvre VMware Workstation Pro
2. Menu **File > New Virtual Machine...** (ou Ctrl+N)
3. Type de configuration : coche **Typical (recommended)** → **Next**
4. "Installer disc image file (iso)" → clique **Browse...** → sélectionne l'ISO ESXi téléchargé → **Next**
   (Workstation affiche normalement "VMware ESXi detected" automatiquement)
5. Nom de la VM : ex. `ESXi-Lab1` → choisis l'emplacement sur le disque → **Next**
6. Taille du disque : mets **40** (Go) → coche **"Store virtual disk as a single file"** → **Next**
7. Avant de cliquer sur Finish, clique sur **"Customize Hardware..."** :
   - **Memory** : mets **4096 MB** minimum (8192 si tu comptes ajouter vCenter plus tard)
   - **Processors** : mets **2** processeurs, 1 cœur chacun ; en bas coche **"Virtualize Intel VT-x/EPT or AMD-V/RVI"** (souvent déjà grisé/coché par défaut = ok)
   - **Network Adapter** : sélectionne **Bridged** (pour joindre l'interface web ESXi depuis le laptop) — ou NAT si pas de réseau physique dispo
   - Clique **Close**
8. Clique **Finish** → la VM apparaît dans l'onglet Workstation, ne démarre pas encore automatiquement
9. Clique sur **"Power on this virtual machine"** (bouton vert ▶ ou dans le menu VM > Power > Power On)

## 3. Installer ESXi (dans la fenêtre de la VM)

Clique dans la fenêtre de la VM pour capturer le clavier (Ctrl+Alt pour en sortir si besoin).

1. Écran **"Welcome to the VMware ESXi 8.0.x Installation"** → appuie sur **Entrée**
2. **End User License Agreement (EULA)** → appuie sur **F11** (Accept and Continue)
3. **Select a Disk to Install or Upgrade** → le disque de 40 Go doit être sélectionné (flèches haut/bas si plusieurs) → **Entrée**
4. **Please select a keyboard layout** → flèches jusqu'à **French** → **Entrée**
5. **Enter a root password** → tape le mot de passe, **Tab**, retape-le pour confirmer → **Entrée**
6. **Confirm Install** → appuie sur **F11** (Install) — l'installation démarre (barre de progression, quelques minutes)
7. **Installation Complete** → retire l'ISO virtuel : menu Workstation **VM > Removable Devices > CD/DVD > Disconnect** (ou laisse, ce n'est pas bloquant en VM) → appuie sur **Entrée** pour redémarrer

## 4. Premier réglage réseau (écran DCUI jaune/gris)

Après le redémarrage tu arrives sur l'écran **"System Customization"** (fond jaune/gris), avec en bas l'IP obtenue en DHCP.

1. Appuie sur **F2** → entre `root` + ton mot de passe → **Entrée**
2. Dans le menu, descends avec les flèches jusqu'à **"Configure Management Network"** → **Entrée**
3. **"Network Adapters"** → vérifie qu'une carte est cochée (barre espace) → **Échap** pour revenir
4. **"IPv4 Configuration"** → **Entrée** :
   - Si le DHCP donné par Workstation te convient, laisse **"Use dynamic IPv4 address"** (espace pour sélectionner) → **Entrée**
   - Sinon, sélectionne **"Set static IPv4 address..."** (espace), **Tab** pour naviguer entre Address / Subnet Mask / Default Gateway, tape les valeurs → **Entrée**
5. **Échap** pour sortir du sous-menu → message **"Configuration Change"** → appuie sur **Y** pour confirmer et redémarrer le réseau management
6. **Échap** à nouveau pour revenir à l'écran principal — note l'IP affichée en bas (ex. `192.168.x.x`)

## 5. Connexion à l'interface web

1. Sur le laptop (pas besoin de sortir de la VM), ouvre un navigateur → `https://<ip-notée-plus-haut>/ui`
2. Accepte l'avertissement de certificat auto-signé ("Avancé" → "Continuer vers...")
3. Connecte-toi : **User name** = `root`, **Password** = celui défini à l'étape 3.5

## A.2 — Réseau virtuel

1. Menu de gauche → **Networking**
2. Onglet **"Virtual switches"** (en haut) → bouton **"Add standard virtual switch"**
   - Nom : `vSwitch1` → laisse le reste par défaut → **Add**
3. Onglet **"Port groups"** → bouton **"Add port group"**
   - Name : `VLAN20-Test`
   - VLAN ID : `20`
   - Virtual switch : sélectionne `vSwitch1` dans le menu déroulant
   - → **Add**
4. Va dans **Virtual Machines** → sélectionne une VM → **Actions > Edit settings** → sur la ligne "Network Adapter 1", change le menu déroulant vers `VLAN20-Test` → **Save**
5. Test d'isolement : crée 2 VM sur `VLAN20-Test`, laisse-en une 3e sur "VM Network" (par défaut) → depuis une VM invitée, `ping` les autres → les 2 sur VLAN20 se répondent, la 3e ne répond pas

## A.3 — Stockage

1. Menu de gauche → **Storage** → onglet **"Datastores"** → bouton **"New datastore"**
2. **Select creation type** : coche **"Create new VMFS datastore"** → **Next**
3. **Select device** : choisis le disque/l'espace libre disponible → **Next**
4. Donne un **nom** au datastore, laisse **VMFS 6** → **Next**
5. **Ready to complete** → vérifie le résumé → **Finish**
6. Le nouveau datastore apparaît dans la liste avec sa capacité

(Pour NFS : même bouton "New datastore" → coche "Mount NFS datastore" → renseigne Name, NFS server (IP), NFS share (chemin) → Finish. Nécessite un serveur NFS existant sur le réseau.)

## A.4 — Créer une VM sous ESXi

1. **Storage** → clique sur le datastore → bouton **"Datastore browser"** → **Upload** → sélectionne l'ISO de l'OS que tu veux installer dans la VM (upload depuis le laptop)
2. Menu de gauche → **Virtual Machines** → bouton **"Create / Register VM"**
3. **"Create a new virtual machine"** → **Next**
4. Name : donne un nom ; Compatibility : `ESXi 8.0 virtual machine` ; Guest OS family / version → **Next**
5. **Select storage** : choisis le datastore → **Next**
6. **Customize settings** :
   - CPU : nombre de vCPU
   - Memory : RAM en Mo
   - Hard disk : taille, menu déroulant → **Thin provisioned** ou **Thick provisioned, eagerly/lazily zeroed** (précise selon consigne du TP)
   - Network Adapter 1 : sélectionne le port group voulu
   - CD/DVD Drive 1 : menu déroulant → **"Datastore ISO file"** → **Browse** → sélectionne l'ISO uploadé à l'étape 1, coche **"Connect at power on"**
   - → **Next**
7. **Ready to complete** → **Finish**
8. Sélectionne la VM créée → bouton **"Power on"** → clique sur la VM → **Console** pour dérouler l'installation de l'OS invité normalement

## A.5 — Snapshots

1. **Virtual Machines** → sélectionne la VM → bouton **"Actions"** → **Snapshots > Take snapshot**
2. Renseigne un **Name**, une Description optionnelle
3. Coche **"Include VM's memory"** si tu veux pouvoir revenir aussi à l'état RAM exact
4. → **Take snapshot**
5. Pour gérer : **Actions > Snapshots > Manage snapshots** → boutons **Revert to**, **Delete**, **Delete all** disponibles sur l'arbre de snapshots

⚠️ Ne laisse jamais un snapshot actif trop longtemps (grossit en continu, sature le datastore).

## A.6 — Clonage et "templates"

⚠️ **Important** : sans vCenter, le Host Client autonome (`/ui`) n'a **pas** de vrai bouton "Clone" ni "Convert to template" au clic droit — ces fonctions demandent vCenter Server. Voici les deux cas :

**Sans vCenter (clonage manuel via le Datastore Browser) :**
1. VM source éteinte → **Storage** → clique le datastore → **Datastore browser**
2. Sélectionne le dossier de la VM source → **Create directory** → nomme le nouveau dossier (ex. `VM-Clone`)
3. Ouvre le dossier source, sélectionne tous les fichiers de la VM (sauf les `.log`) → bouton **Copy** → choisis le dossier créé → **Copy**
4. Une fois la copie terminée (suivi dans "Recent tasks" en bas), retourne dans le nouveau dossier → clic droit sur le fichier **.vmx** → **Register VM**
5. Démarre la VM copiée → un message demande **"I Copied It" / "I Moved It"** → choisis **"I Copied It"** (régénère l'adresse MAC)
6. **Actions > Edit settings > VM Options** pour renommer proprement la VM

**Avec vCenter (si tu l'as installé) :** clic droit sur la VM → **Clone > Clone to Virtual Machine** ou **Clone to Template**, puis clic droit sur un template → **Deploy VM from this template** — c'est ce mode qui correspond exactement à l'énoncé "Cloner / Convertir en modèle / Déployer depuis un template".

