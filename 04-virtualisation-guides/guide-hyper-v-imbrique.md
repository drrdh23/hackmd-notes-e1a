# TP Microsoft Hyper-V — Guide détaillé (sur PC portable, VM imbriquée)

Comme pour ESXi, on fait ça en VM dans VMware Workstation : une VM **Windows Server 2022**, dans laquelle on active le rôle Hyper-V, dans laquelle on crée ensuite une VM Windows 10 (double imbrication). Ça marche, mais il faut bien activer la virtualisation imbriquée sur la VM Server.

## 0. Prérequis

1. Télécharge l'ISO **Windows Server 2022** (évaluation gratuite 180 jours) sur `https://www.microsoft.com/evalcenter/download-windows-server-2022`
2. Télécharge aussi l'ISO **Windows 10** (ou 11) que tu installeras plus tard DANS Hyper-V, sur `https://www.microsoft.com/software-download/windows10`
3. RAM conseillée sur le laptop : **16 Go minimum**, idéalement 32 Go (Server 2022 + Hyper-V + VM Windows 10 imbriquée consomment beaucoup)

## 1. Créer la VM Windows Server 2022 dans VMware Workstation

1. **File > New Virtual Machine...** (Ctrl+N) → **Typical** → **Next**
2. **Browse...** → sélectionne l'ISO Windows Server 2022 → **Next**
   (si Workstation ne détecte pas l'OS automatiquement, sélectionne manuellement "Microsoft Windows > Windows Server 2022" dans l'écran suivant)
3. Nom de VM : `WinServer2022-HyperV` → **Next**
4. Taille disque : **80 Go minimum** (Server + Hyper-V + VM imbriquée) → **Store virtual disk as a single file** → **Next**
5. **Customize Hardware...** avant de finir :
   - **Memory** : **8192 MB minimum** (12-16 Go si possible, car la VM Windows 10 imbriquée en consommera aussi)
   - **Processors** : **4 processeurs** si ton CPU le permet, en bas coche impérativement **"Virtualize Intel VT-x/EPT or AMD-V/RVI"** — sans ça, Hyper-V refusera de démarrer des VM à l'intérieur (erreur "l'hyperviseur n'est pas exécuté")
   - **Network Adapter** : **Bridged**
   - **Close**
6. **Finish** → **Power on this virtual machine**

## 2. Installer Windows Server 2022

1. Écran "Windows Setup" → langue/format horaire → **Next**
2. **Install now**
3. Choix de l'édition : sélectionne **Windows Server 2022 Standard (Desktop Experience)** (avec interface graphique, plus simple pour la suite) → **Next**
4. Accepte le contrat de licence → **Next**
5. **Custom: Install Windows only (advanced)**
6. Sélectionne le disque de 80 Go → **Next** → l'installation copie les fichiers et redémarre plusieurs fois automatiquement
7. Au premier démarrage : écran **"Customize settings"** → définis le mot de passe administrateur, confirme → **Finish**
8. **Ctrl+Alt+Insert** (raccourci Workstation pour Ctrl+Alt+Suppr) → connecte-toi avec `Administrator` + le mot de passe

## 3. Ajouter le rôle Hyper-V (Server Manager)

1. **Server Manager** s'ouvre automatiquement à la connexion (sinon : icône dans la barre des tâches)
2. Menu **Manage** (en haut à droite) → **Add Roles and Features**
3. **Before You Begin** → **Next**
4. **Installation Type** : laisse **"Role-based or feature-based installation"** → **Next**
5. **Server Selection** : le serveur local est déjà sélectionné → **Next**
6. **Server Roles** : coche la case **Hyper-V**
   - Une popup **"Add features that are required for Hyper-V?"** apparaît → clique **"Add Features"**
   - → **Next**
7. **Features** : rien à cocher de plus → **Next**
8. **Hyper-V** (page d'intro) → **Next**
9. **Create Virtual Switches** : coche la carte réseau que tu veux utiliser pour un futur commutateur externe (tu peux laisser décoché si tu veux tout créer manuellement plus tard) → **Next**
10. **Virtual Machine Migration** : laisse décoché → **Next**
11. **Default Stores** : laisse les chemins par défaut (`C:\ProgramData\Microsoft\Windows\Hyper-V\`) ou change si tu veux stocker ailleurs → **Next**
12. **Confirmation** : coche **"Restart the destination server automatically if required"** → confirme le popup → **Install**
13. La barre de progression tourne, le serveur **redémarre automatiquement** en cours de route

## 4. Redémarrage / finalisation

1. Après le redémarrage automatique, reconnecte-toi (`Administrator` + mot de passe)
2. Server Manager se rouvre et termine la configuration ("Configuration successful" en général visible dans les notifications, icône drapeau en haut de Server Manager)
3. Le rôle Hyper-V est maintenant actif

## 5. Ouvrir Hyper-V Manager

1. **Server Manager** → menu **Tools** (en haut à droite) → **Hyper-V Manager**
2. Dans le panneau de gauche, clique sur le nom de ton serveur pour voir les VM (vide pour l'instant)

## 6. Créer un vSwitch interne

1. Dans le panneau **Actions** à droite → **Virtual Switch Manager...**
2. Dans la liste de gauche : sélectionne **Internal** → clique **Create Virtual Switch**
3. Donne un nom (ex. `vSwitch-Interne`) → laisse le type "Internal network" → **OK**
4. (Le type Internal permet aux VM de communiquer entre elles ET avec l'hôte Server 2022, mais pas vers l'extérieur — voir B.2 plus bas pour Externe/Privé)

## 7. Créer la VM Windows 10

1. Panneau **Actions** → **New > Virtual Machine...**
2. **Before You Begin** → **Next**
3. **Specify Name and Location** : nom (ex. `VM-Win10-Test`) → **Next**
4. **Specify Generation** : choisis **Generation 2** (UEFI/Secure Boot, recommandé) ou **Generation 1** si l'ISO Windows 10 est ancien/32 bits → **Next**
5. **Assign Memory** : **4096** Mo → décoche "Use Dynamic Memory for this virtual machine" si tu veux du statique pur, ou laisse coché → **Next**
6. **Configure Networking** : menu déroulant → sélectionne `vSwitch-Interne` → **Next**
7. **Connect Virtual Hard Disk** : **Create a virtual hard disk** → Size = **40 Go** → **Next**
8. **Installation Options** : **Install an operating system from a bootable image file** → **Browse...** → sélectionne l'ISO Windows 10 (copié au préalable dans la VM Server via un dossier partagé Workstation, ou monté en ISO virtuel Workstation puis copié) → **Next**
9. **Summary** → **Finish**
10. La VM apparaît dans la liste → clic droit dessus → **Settings** → **Processor** → mets **Number of virtual processors = 2** → **OK**
11. Si Generation 2 : **Settings > Security** → décoche **"Enable Secure Boot"** si l'installeur Windows 10 refuse de démarrer (rare avec ISO officiel récent, mais utile à savoir)

## 8. Installer l'OS et tester

1. Double-clique sur la VM (ou clic droit → **Connect...**) pour ouvrir la fenêtre de la VM
2. Bouton **Start** (▶ vert en haut) pour démarrer
3. L'installeur Windows 10 se lance depuis l'ISO → déroule l'installation normalement (langue, "Install now", clé produit "I don't have a product key" possible pour test, édition, licence, "Custom: Install Windows only", sélection du disque de 40 Go, installation)
4. Après plusieurs redémarrages automatiques, configuration initiale (région, clavier, compte local) → arrivée sur le bureau Windows 10
5. **Résultat attendu du TP2 : la VM démarre, tu es sur le bureau Windows 10 — capture d'écran pour ton rendu si besoin**

---

# Référence complète — Partie B

## B.1 — Installation
- Rôle Windows Server (fait ci-dessus) ou fonctionnalité "Hyper-V" activable directement sur Windows 10/11 Pro via **Panneau de configuration > Programmes > Activer ou désactiver des fonctionnalités Windows > cocher Hyper-V**
- Outils d'administration : **Hyper-V Manager** (local/basique), **Windows Admin Center** (interface web moderne, multi-serveurs), **SCVMM** (System Center Virtual Machine Manager, gestion à grande échelle avec vrais templates)

## B.2 — Réseau virtuel
Trois types de commutateur (Virtual Switch Manager) :
- **Externe** : relié à une carte réseau physique, les VM sortent sur le réseau/Internet
- **Interne** : VM ↔ VM et VM ↔ hôte, pas d'accès extérieur
- **Privé** : VM ↔ VM uniquement, même pas l'hôte
VLAN : sur chaque carte réseau virtuelle → **Settings de la VM > Network Adapter > VLAN ID**, cocher "Enable virtual LAN identification" et indiquer l'ID

## B.3 — Stockage
- **VHD** (ancien format, limite 2 To) vs **VHDX** (recommandé, jusqu'à 64 To, plus résilient) — choix au moment de "Connect Virtual Hard Disk" dans l'assistant de création de VM, ou via **Hyper-V Manager > New > Hard Disk**
- **Disque fixe** (taille réservée immédiatement, meilleures perfs) vs **dynamique** (grossit à l'usage, comme le Thin d'ESXi)
- **Disque de différenciation** : disque enfant qui ne stocke que les différences par rapport à un disque parent en lecture seule — pratique pour des VM de test jetables à partir d'un master
- **Storage Spaces Direct (S2D)** : équivalent vSAN, agrège le stockage local de plusieurs nœuds en cluster — notion théorique, non réalisable avec un seul hôte nested

## B.4 — Création d'une VM (rappel des choix clés)
- **Génération 1** : BIOS classique, compatible OS anciens/32 bits
- **Génération 2** : UEFI + Secure Boot, requis pour Windows 10/11 modernes, meilleures perfs de boot
- **Mémoire statique** : quantité fixe réservée ; **dynamique** : la VM démarre avec un minimum et grossit selon le besoin jusqu'à un plafond (Settings > Memory > Enable Dynamic Memory, Minimum/Maximum/Buffer)

## B.5 — Checkpoints
- **Checkpoint standard** : capture disque + mémoire (comme un snapshot ESXi avec mémoire) — pratique pour reprendre un état de test exact, mais pas cohérent pour des serveurs applicatifs (bases de données notamment)
- **Checkpoint de production** : utilise VSS (Volume Shadow Copy Service) côté invité, **pas de mémoire capturée**, garantit un état disque cohérent au redémarrage — **recommandé en production**
- Réglage : clic droit VM → **Settings > Checkpoints** → choisir le type par défaut
- Prise d'un checkpoint : clic droit VM → **Checkpoint**

## B.6 — Export/Import et clonage
- **Exporter** : clic droit sur une VM (arrêtée ou en marche) → **Export...** → choisir un dossier de destination
- **Importer** : dans Hyper-V Manager → **Import Virtual Machine...** → pointer vers le dossier exporté → choisir **"Copy the virtual machine"** pour obtenir un clone indépendant (régénère les identifiants) ou **"Register"** pour la réutiliser telle quelle
- C'est cette combinaison Export puis Import-en-copie qui fait office de **clonage** en Hyper-V Manager seul
- **SCVMM** : pour de vrais templates réutilisables avec personnalisation automatisée (sysprep intégré), équivalent du "Convert to template" de vCenter

## Exercices Hyper-V
- **QCM** : différences entre les 3 types de commutateurs ; VHD vs VHDX ; checkpoint standard vs production
- **TP 3 commutateurs** : créer un switch Externe, un Interne, un Privé, vérifier les accès/non-accès attendus entre VM et vers Internet pour chacun
- **TP Gen2 + VHDX dynamique** : créer une VM Génération 2 avec un disque VHDX dynamique, vérifier dans les propriétés du fichier que la taille réelle sur disque est bien inférieure à la taille max déclarée
- **TP checkpoint** : prendre un checkpoint standard, modifier la VM, revenir en arrière (clic droit checkpoint → **Apply**), constater le retour à l'état précédent
- **TP export/import** : exporter une VM, l'importer en copie sous un autre nom, vérifier qu'elle a un nouvel identifiant (GUID) et peut tourner en parallèle de l'originale
- **Réflexion — checkpoint de production** : pourquoi VSS et l'absence de capture mémoire rendent ce mode plus sûr pour un serveur applicatif (base de données, Active Directory) qu'un checkpoint standard qui peut geler des transactions en cours