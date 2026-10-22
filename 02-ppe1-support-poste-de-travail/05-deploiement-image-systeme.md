# PROJET 5 — Déploiement standardisé d'un poste (image système)

**Module IT Essentials — Classe E1A**
*Documentation technique complète de la procédure*

Nom / Prénom : Rrudhani Deaa
Date : 20 septembre 2026

---

## 1. Introduction et cahier des charges

### 1.1 Contexte et objectifs

Ce projet s'inscrit dans le cadre du module IT Essentials (Projet 5). L'objectif est de mettre en place une procédure de déploiement standardisé d'un poste de travail à partir d'une image système, afin de réduire le temps d'installation et de garantir une configuration identique sur plusieurs postes. La démarche complète comprend : la rédaction d'un cahier des charges, l'installation et la configuration d'un poste de référence, la création d'une image système exploitable pour un déploiement automatisé, la validation de cette image sur un second poste, et enfin une comparaison chiffrée entre une installation manuelle et une installation automatisée.

Cette documentation détaille, de façon exhaustive, l'intégralité des actions réalisées entre le 17 et le 20 septembre 2026 : chaque écran de configuration traversé, chaque commande saisie, chaque disque ajouté ou retiré, chaque redémarrage et extinction de machine, ainsi que l'ensemble des difficultés rencontrées, la façon dont elles ont été diagnostiquées, et pourquoi certaines approches ont dû être abandonnées au profit d'autres.

### 1.2 Choix techniques et justification

- Poste de référence : machine virtuelle Windows 11, créée sous VMware Workstation Pro.
- Outil de clonage / déploiement : Clonezilla (édition Live), démarré via une image ISO sur lecteur CD/DVD virtuel.
- Environnement de virtualisation : VMware Workstation, permettant de travailler sans risque sur du matériel réel et de simuler plusieurs postes à partir d'une seule machine physique.

### 1.3 Cahier des charges du poste standard

| Élément | Spécification retenue |
|---|---|
| Système d'exploitation | Windows 11 (64 bits) |
| Environnement de virtualisation | VMware Workstation Pro |
| Espace disque système | 60 Go |
| Préparation avant capture | Généralisation via Sysprep (`/generalize /oobe`) |
| Outil de capture / déploiement | Clonezilla Live (mode device-image) |
| Logiciels / configuration additionnelle | ____________________ |


---

## 2. Installation et configuration du poste de référence

### 2.1 Création de la machine virtuelle de référence

1. Menu **File > New Virtual Machine...** (Ctrl+N).
2. Mode **Typical (recommended)**.
3. Source d'installation : **I will install the operating system later**.
4. Guest OS = **Microsoft Windows**, Version = **Windows 11 x64**.
5. Nom de la VM et emplacement (`C:\vm\Windows 11 x64 (3)`).
6. Disque : **60 Go**, option **Split virtual disk into multiple files**.
7. Écran récapitulatif → **Customize Hardware...** (RAM, CPU, CD/DVD) → **Finish**.
![Capture d'écran 2026-09-19 214632](https://hackmd.io/_uploads/HJv9BspYMe.png)
![Capture d'écran 2026-09-19 214659](https://hackmd.io/_uploads/Syx6rs6tfg.png)
![Capture d'écran 2026-09-19 214852](https://hackmd.io/_uploads/SJ-eIjTtze.png)


Le type d'invité « Windows 11 » associe automatiquement un module **TPM virtuel**, un firmware **UEFI**, et impose le **chiffrement de la VM** — comportement qui explique le blocage rencontré section 4.1.



### 2.2 Installation et configuration de Windows 11

Installation standard via l'ISO monté sur le CD/DVD virtuel (langue/clavier, édition, partitionnement du disque de 60 Go, copie des fichiers), puis configuration de base du poste (section 1.3).
![Capture d'écran 2026-09-17 105131](https://hackmd.io/_uploads/HJs0Bo6FMx.png)

### 2.3 Ajout du disque de dépôt dans les paramètres de la VM

1. VM éteinte → clic droit → **Settings** (Ctrl+D).
2. Onglet **Hardware** → **Add...**
3. **Add Hardware Wizard** → **Hard Disk** → Next.
4. Contrôleur : **NVMe** (SATA/SCSI testés par la suite sur d'autres VM) → Next.
5. **Create a new virtual disk** → Next.
6. Taille **60 Go**, **Split virtual disk into multiple files** → Next.
7. Emplacement/nom (`C:\vm\new desk.vmdk`, puis plus tard `C:\vm\Disque clone.vmdk`) → Finish.
8. **OK**.
![Capture d'écran 2026-09-20 200328](https://hackmd.io/_uploads/BJMKUiaKzl.png)
![Capture d'écran 2026-09-20 200247](https://hackmd.io/_uploads/HyztIiTtGe.png)

![Capture d'écran 2026-09-19 221557](https://hackmd.io/_uploads/S1XELjpYGe.png)


Sur la VM de référence, cette procédure s'est déroulée sans aucune difficulté : les huit étapes ci-dessus ont été suivies normalement et le disque de dépôt est apparu directement dans l'explorateur Windows, prêt à être formaté. Ce n'est que plus tard, lors d'une tentative similaire sur une VM de test distincte (**Poste-Cible-Test**, voir section 4.1), qu'un blocage lié au TPM est apparu précisément à l'étape 5, avec l'erreur : *« The selected virtual disk is unencrypted and cannot be used with an encrypted virtual machine »*.


### 2.4 Préparation du poste avec Sysprep et diskpart

**Sysprep :**
```
cd C:\Windows\System32\Sysprep
sysprep.exe /generalize /oobe /shutdown
```
`/generalize` supprime les identifiants machine ; `/oobe` force le redémarrage sur l'écran d'accueil ; `/shutdown` éteint automatiquement à la fin. La VM s'éteint exactement à l'écran **« Sélectionnez votre pays ou votre région »** — ne jamais valider cet écran avant la capture.




**Préparation du disque de dépôt sans casser l'état Sysprep :**
1. Rallumer la VM (redémarre sur l'écran OOBE).
2. **Maj + F10 (Shift+F10)** → invite de commandes admin, sans valider OOBE.
3. `diskpart`
4. `list disk` (le disque vierge affiche « Libre : 60 G octets » en totalité, contrairement aux disques déjà partitionnés qui n'affichent que « 1024 K octets »).
5. `select disk 1`
6. `clean`
7. `create partition primary`
8. `format fs=ntfs quick label="stockage"`
9. `assign`
10. `exit`
11. `shutdown /s /t 0` (sans jamais toucher à l'OOBE).

```
list disk
select disk 1
clean
create partition primary
format fs=ntfs quick label="stockage"
assign
exit
```

![Capture d'écran 2026-09-20 135732](https://hackmd.io/_uploads/ryL28jaYGx.png)
![Capture d'écran 2026-09-20 140121](https://hackmd.io/_uploads/B1bRIj6tfg.png)



---

## 3. Création de l'image système et script de déploiement automatisé

### 3.1 Outil utilisé : Clonezilla

1. VM Settings → **CD/DVD (SATA)**.
2. Cocher **Connect at power on**.
3. **Use ISO image file** → sélectionner `clonezilla-live-....iso`.
4. Démarrer la VM → menu GRUB Clonezilla.

### 3.2 Procédure de sauvegarde (savedisk) — première tentative, jeudi 19 septembre, 20h41



1. Menu GRUB → **Clonezilla live (Default settings)**.
2. Langue → Français.
3. Clavier → **Don't touch keymap**.
4. **Start Clonezilla**.
5. Mode → **device-image**.
6. Dépôt → **local_dev**.
7. Sélection du disque de dépôt (« new desk.vmdk »), dossier racine **/**.
8. Mode → **Beginner mode**.
9. Opération → **savedisk**.
10. Nom de l'image → auto-généré.
11. Disque source → identifié par ses 4 partitions (ESP FAT32 ~200 Mo, MSR 16 Mo, Windows NTFS ~59 Go, Recovery NTFS ~793 Mo), coché à la barre d'espace.
12. Compression → **-z9p**.
13. Vérification fsck → **-sfsck**.
14. Confirmation finale → `y` + Entrée (double confirmation).
15. Attente de la progression Partclone.


**Difficulté annexe — clavier AZERTY/QWERTY** : l'option « Don't touch keymap » ayant été conservée, le clavier est resté en QWERTY alors que le clavier physique et en AZERTY (lettres A/Q/Z/W/M et symboles « / », « - » décalés). Contournement : correspondance manuelle entre les deux dispositions + usage de Tab pour la complétion automatique.

**Résultat et découverte du problème**

> **19/09 — 21h22 (≈41 min plus tard)** : fin de l'opération, message « Cette image a été sauvegardée avec succès ».

Vérification directe :
```
ls -la /home/partimag
```
![image](https://hackmd.io/_uploads/rkTqjopKfl.png)

→ **dossier vide**, aucune donnée écrite. Message d'erreur découvert plus tard dans les journaux :
```
Le répertoire dépôt de l'image est en lecture seule, non inscriptible : /home/partimag
'ocs-live-general' finished with error!
```
Tentative de réparation :
```
ntfsfix /dev/nvme0n2p2
```
→ échec, `Permission denied` au niveau du périphérique bloc. Décision : abandon du disque, reprise le lendemain.
![image](https://hackmd.io/_uploads/SkHHhsaYze.png)

![image](https://hackmd.io/_uploads/B1dW3jaFGx.png)

Enseignement méthodologique : un message de succès ne suffit jamais à garantir la validité d'une sauvegarde ; une vérification directe du contenu réellement écrit est indispensable.

### 3.3 Deuxième tentative de sauvegarde — Dimanche 20 septembre

Nouveau disque « Disque clone.vmdk » ajouté (section 2.3) puis formaté via la procédure Shift+F10 + diskpart (section 2.4). Procédure savedisk (étapes 3.2, 1 à 17) intégralement recommencée. Point de vigilance : l'option fsck était cette fois présélectionnée par défaut sur **-fsck** (interactif) au lieu de **-sfsck** — resélection manuelle nécessaire. Vérification post-sauvegarde activée (« Oui »).

**Résultat : sauvegarde réussie et vérifiée**, 4 partitions confirmées restaurables, ~15,3 Go sur 60 Go (26 %),.

![image](https://hackmd.io/_uploads/BJ_ty3TFze.png)



**Identification du bon fichier disque parmi plusieurs fichiers résiduels** : le dossier `C:\vm\` contenait des résidus (« disque vide image.vmdk », « Disk new.vmdk », un dossier GUID de la VM « Poste-Cible-Test » supprimée). Identification par comparaison taille/date de modification dans l'explorateur Windows : « new desk.vmdk » resté à 73,4 Mo constant (rien écrit), fragments `Windows 11 x64 (3)-s001.vmdk` à `-s016.vmdk` inchangés (disque source, lu mais jamais modifié).

Erreur annexe : `Cannot open the disk 'C:\vm\new desk.vmdk'… Module 'Disk' power on failed` — dossier de verrouillage `.lck` résiduel, résolu en vérifiant qu'aucune VM ne l'utilisait puis en le supprimant.

Le bouton **Remove** des VM Settings, censé détacher le disque défectueux, est resté grisé même disque sélectionné (limitation VMware non résolue) — disque laissé attaché mais inutilisé.

---

## 4. Test de déploiement sur un second poste

### 4.1 Recherche d'un second poste : trois tentatives successives

**Tentative 1 — VM « Poste-Cible-Test », type d'invité Windows 11 :**
1. New VM Wizard → Guest OS Windows 11.
2. Écran **Encryption Information** → case chiffrement décochée.
3. Nouvel essai avec l'option restreinte **Only files needed to support a TPM are encrypted**.
4. Ajout du disque de dépôt → erreur : *« The selected virtual disk is unencrypted and cannot be used with an encrypted virtual machine »*, quelle que soit l'option choisie.
![image](https://hackmd.io/_uploads/rk_xWh6Ffx.png)

Cause : le type d'invité Windows 11/10 associe automatiquement un TPM virtuel imposant le chiffrement de la VM.


**Tentative 2 — même VM, changement de type d'invité :**
1. VM Settings → Options → General → Guest OS → **Other / Other 64-bit**.
2. Disque de dépôt ajouté sans erreur cette fois.
3. Démarrage Clonezilla → erreur noyau : *« No working init found. Try passing init= option to kernel. »*
4. Correction 1 : CD/DVD SATA→IDE (déjà IDE, sans effet).
5. Correction 2 : disque de données NVMe→SATA (sans effet).
6. Correction 3 : option de démarrage « VGA 800x600 & To RAM » (sans effet).
7. VM « Poste-Cible-Test » supprimée.

Cause : incompatibilité firmware BIOS hérité / émulation NVMe.



**Tentative 3 (retenue) — réutilisation de la VM Ubuntu existante :**
1. Sélection de la VM Ubuntu déjà fonctionnelle.
2. Ajout d'un disque vierge (cible), contrôleur **SCSI**.
3. Ajout du disque de dépôt « Disque clone.vmdk ».

Difficulté : premier ajout du disque de dépôt fait alors que la VM de référence était encore allumée → signalé réussi par VMware mais non concrétisé (scan Clonezilla montrait toujours « text », pas « stockage »). Corrigé en éteignant la VM de référence avant de refaire l'ajout.

Ce choix évite le TPM forcé des VM Windows et l'incompatibilité BIOS/NVMe de la tentative 2.

![image](https://hackmd.io/_uploads/BkrbpjaYfg.png)



### 4.2 Procédure de restauration (restoredisk)

1. Démarrage VM Ubuntu sur Clonezilla, vérification préalable de l'image via l'explorateur intégré.
2. Opération → **restoredisk** (attention, « savedisk » reste surligné par défaut).
3. Image source → `2026-09-20-14-img`.
4. Disque cible → `sda` (seul disque proposé), coché à la barre d'espace.
5. Vérification pré-restauration → 4 partitions confirmées restaurables.
6. Double confirmation `y`/`y`.
7. Progression Partclone.


### 4.3 Échec du premier redémarrage post-restauration et diagnostic

Message en boucle : `/init: line 38: can't open /dev/sr1: No medium found`, puis netboot proposé, puis shell BusyBox.


![image](https://hackmd.io/_uploads/SkN6kh6FMl.png)


Tentatives, dans l'ordre :
1. Déconnexion du CD/DVD (VM Settings).
2. Échap/F2 au démarrage → échec (timing trop court).
3. **VM Settings → Options → Boot Options → Force BIOS setup** → entrée automatique dans le BIOS.
4. Dans le BIOS, onglet Boot, tentative de réorganisation avec **+/-** → échec (AZERTY, pas de pavé numérique).
5. Abandon ; examen direct de **VM Settings → Options → Advanced**.
6. Découverte : **Firmware type = BIOS** au lieu de UEFI.

![image](https://hackmd.io/_uploads/rkLkgh6Yfg.png)


Explication : le disque restauré est GPT/UEFI (partition système EFI) ; un firmware BIOS ne peut pas le démarrer, d'où la retombée systématique sur le CD/DVD quel que soit l'ordre de démarrage configuré. Le réglage de l'ordre de boot (étapes 3-4) n'aurait donc jamais suffi.

### 4.4 Résolution et validation finale

1. **VM Settings → Options → Advanced → Firmware type → UEFI**.
2. **Secure Boot** laissé décoché.
3. **OK**, redémarrage.

Résultat : chargement immédiat du Windows Boot Manager, logo Windows affiché.
![Capture d'écran 2026-09-20 154744](https://hackmd.io/_uploads/B1ozen6Ffx.png)
![Capture d'écran 2026-09-20 170243](https://hackmd.io/_uploads/SJsGx3TFMl.png)



Preuve de réussite du test de déploiement. Par contrainte de temps, la configuration OOBE n'a pas été poursuivie jusqu'au Bureau ; le chargement réussi du Windows Boot Manager a été retenu comme preuve suffisante.

---

## 5. Comparaison des temps d'installation

### 5.1 Méthodologie

Temps automatisé = mesuré dans les journaux Clonezilla (restoredisk). Temps manuel = durée de référence usuelle (non re-mesurée en conditions réelles par contrainte de temps).

### 5.2 Tableau comparatif

| Étape | Installation manuelle (référence) | Déploiement automatisé (Clonezilla, mesuré) |
|---|---|---|
| Installation du système d'exploitation | ≈ 25 à 30 min | ≈ 4 min |
| Configuration post-installation | Nécessaire à chaque poste | Déjà incluse dans l'image |
| **Total pour un poste** | **≈ 25 à 30 min** | **≈ 4 min** |

### 5.3 Analyse

Gain de temps de 85 à 90 % sur la seule installation de l'OS, encore plus significatif en intégrant la configuration logicielle (réalisée une seule fois sur le poste de référence).

---

## 6. Bilan des difficultés rencontrées

| Problème rencontré | Cause identifiée | Solution retenue |
|---|---|---|
| Disque non chiffré impossible à attacher à la VM « Poste-Cible-Test » (Windows 11) | TPM virtuel automatique imposant le chiffrement (même avec « TPM files only ») | Changement du type d'invité en « Other/Other 64-bit » |
| Erreur de noyau « No working init found » sur cette même VM | Firmware BIOS hérité incompatible avec l'émulation NVMe | Abandon et suppression de la VM ; réutilisation d'une VM Ubuntu fonctionnelle |
| Clavier QWERTY par défaut dans Clonezilla (clavier physique AZERTY) | Option « Don't touch keymap » conservée | Correspondance manuelle des touches ; usage de Tab |
| Première sauvegarde « réussie » mais image vide | Dépôt monté en lecture seule, non signalé clairement | Nouveau disque créé via diskpart ; sauvegarde recommencée et vérifiée |
| Ordre d'énumération des disques NVMe instable | Comportement connu de l'émulation NVMe sous VMware | Identification par structure des partitions et étiquette |
| Fichiers .vmdk résiduels prêtant à confusion | Restes de tentatives précédentes et de la VM supprimée | Comparaison taille/date de modification dans l'explorateur Windows |
| Erreur « Cannot open the disk… power on failed » | Dossier de verrouillage .lck résiduel | Vérification qu'aucune VM ne l'utilisait, puis suppression |
| Disque de dépôt absent du scan malgré ajout signalé réussi | VM de référence encore allumée lors de l'ajout | VM éteinte avant l'ajout ; nouvelle tentative vérifiée |
| Réordonnancement du boot impossible (touches +/- inopérantes) | Clavier AZERTY, pas de pavé numérique | Abandon ; examen direct des paramètres avancés |
| Échec de démarrage du disque restauré (« No medium found ») | Firmware VM en BIOS alors que le disque est GPT/UEFI | Passage du firmware de BIOS à UEFI |
| Bouton « Remove » grisé pour détacher un disque obsolète | Limitation de l'interface VMware Workstation, non résolue | Disque laissé attaché mais inutilisé |

---



---

## 7. Conclusion

Malgré de nombreux obstacles techniques — liés pour l'essentiel aux contraintes spécifiques de la virtualisation (association TPM/chiffrement, incompatibilité BIOS/NVMe, instabilité de la nomenclature des disques, échec silencieux d'une sauvegarde) — l'ensemble de la chaîne de déploiement a été mis en œuvre et validé avec succès : un poste de référence standardisé a été créé et généralisé via Sysprep, une image système vérifiée a été produite avec Clonezilla, puis cette image a été déployée avec succès sur un second poste (simulé), démarrant de façon autonome et indépendante du poste d'origine.

La comparaison chiffrée (environ 4 minutes pour le déploiement automatisé contre 25 à 30 minutes pour une installation manuelle de l'OS seul) confirme l'intérêt concret d'une politique de déploiement par image pour l'approvisionnement de plusieurs postes identiques. L'enseignement le plus important de ce projet reste toutefois méthodologique : un message de succès affiché par un outil ne suffit jamais à garantir la validité d'une opération critique (sauvegarde, déploiement) ; une vérification directe et systématique du résultat obtenu est indispensable.
