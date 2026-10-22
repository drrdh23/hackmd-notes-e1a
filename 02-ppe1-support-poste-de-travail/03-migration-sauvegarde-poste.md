# Projet 3 — Migration et sauvegarde d'un poste existant

## 0. Préparer l'environnement de travail

On réutilise le principe des projets précédents : une VM Windows 11 dans VirtualBox qui va jouer le rôle du "poste vieillissant" à migrer.

1. Ouvre VirtualBox
2. Soit tu réutilises une VM déjà créée (PC-COMPTA-01 par exemple), soit tu en crées une nouvelle nommée `PosteAMigrer` (même procédure que les projets 1 et 2 : Nouvelle → 4096 Mo RAM → 64 Go disque → EFI activé → TPM 2.0 → ISO Windows 11 chargée)
3. Démarre la VM et installe Windows 11 si ce n'est pas déjà fait (voir guide Projet 1/2 : `oobe\bypassnro` pour le compte local, etc.)
4. Crée un compte utilisateur, par exemple `Utilisateur1`, qui représente la personne dont on migre le poste

![image](https://hackmd.io/_uploads/BydGZ6DYMg.png)


---

## 1. Installer les VirtualBox Guest Additions

Cette étape est indispensable pour pouvoir échanger des fichiers entre la VM et ton PC hôte (ça sert de "support externe" de sauvegarde).

- Clique n'importe où dans l'écran de la VM (le bureau Windows avec les arbres verts) pour être sûr que le focus est bien dans Windows, pas dans VMware.
- Ouvre l'explorateur de fichiers Windows (icône dossier jaune dans la barre des tâches, ou touche Windows + E).
- Dans la liste des lecteurs, tu devrais voir un lecteur CD/DVD nommé quelque chose comme "VMware Tools". Double-clique dessus.
- Tu devrais voir un fichier setup.exe (ou setup64.exe pour Windows 64 bits). Double-clique dessus pour lancer l'installation.
- Windows va demander confirmation via le "Contrôle de compte d'utilisateur" → clique "Oui", puis suis l'assistant (Next, Next, Install).


---

## 1. Créer un dossier de sauvegarde côté hôte et le partager avec la VM

### 2.1 Sur ton PC HÔTE (ton vrai bureau Windows, pas la VM)
1. Ouvre l'Explorateur de fichiers (Windows + E)
2. Va sur ton disque C: (ou où tu veux)
3. Clic droit dans une zone vide → Nouveau → Dossier
4. Nomme-le `Sauvegarde VM`
5. Note son chemin complet, par exemple : `C:\Sauvegarde VM`

### 2.2 Dans les réglages de la VM (Vmware)
6. Si la VM est allumée, éteins-la d'abord : dans VirtualBox, clic droit sur la VM → "Fermer" → "Éteindre la machine"
7. Sélectionne la VM dans VirtualBox → clique "Configuration" (icône engrenage)
8. Clique sur "Dossiers partagés" dans la liste de gauche
9. Clique sur la petite icône dossier avec un "+" vert (à droite, en haut de la zone "Liste des dossiers partagés machine")
10. Champ "Chemin du dossier" : clique sur le menu déroulant → "Autre..." → sélectionne ton dossier `C:\SauvegardeVM` → OK
11. Champ "Nom du dossier" : laisse ou tape `Sauvegarde`
12. Coche la case "Montage automatique"
13. Coche la case "Rendre permanent"
14. Clique "OK", puis "OK" à nouveau pour fermer la Configuration
15. Redémarre la VM (bouton "Démarrer")



---

## 3. Créer des données de test à migrer (simulation du poste existant)

Dans la VM, connecté avec `Admin` :

1. Ouvre l'Explorateur de fichiers → clique sur "Documents" dans le menu de gauche
2. Clic droit dans une zone vide → Nouveau → Document texte
3. Nomme-le `RapportImportant`, ouvre-le, tape quelques mots dedans, enregistre (Ctrl + S) et ferme
4. Répète pour créer 2-3 autres fichiers de test (ex : `Budget`, `Notes`)
5. Fais pareil sur le Bureau : crée 1-2 fichiers ou dossiers de test
6. Ouvre Edge, va sur 2-3 sites, ajoute-les aux Favoris (étoile dans la barre d'adresse) pour simuler des paramètres à migrer
![Capture d'écran 2026-09-16 114141](https://hackmd.io/_uploads/B1P4dEdFzx.png)


---

## 4. Faire l'inventaire des données à conserver

Note dans un document (ce sera la base de ton rapport) les emplacements à sauvegarder :

- `C:\Users\Admin\Documents` → fichiers de travail
- `C:\Users\Admin\Desktop` → fichiers/raccourcis du Bureau
- `C:\Users\Admin\Pictures` → si présentes
- Favoris du navigateur Edge → `C:\Users\Admin\AppData\Local\Microsoft\Edge\User Data\Default\Bookmarks`




---

## 5. Sauvegarder les données sur le support externe (dossier partagé)

Dans la VM, connecté avec `Admin` (ou le compte admin pour accéder à tout) :

1. Ouvre l'Explorateur de fichiers
2. Clique sur "Ce PC" (ou "Réseau") dans le menu de gauche
3. Tu dois voir un lecteur réseau nommé quelque chose comme `\\VBOXSVR\Sauvegarde` ou une lettre de lecteur (ex: Z:) — si tu ne le vois pas, tape directement dans la barre d'adresse en haut : `\\VBOXSVR\Sauvegarde` puis Entrée
4. Cette fenêtre représente ton dossier hôte `C:\SauvegardeVM` — laisse-la ouverte
5. Ouvre une deuxième fenêtre Explorateur (Windows + E à nouveau)
6. Dans cette deuxième fenêtre, va dans `C:\Users\Utilisateur1\Documents`
7. Sélectionne tous les fichiers (Ctrl + A)
8. Fais un clic droit sur la sélection → "Copier" (ou Ctrl + C)
9. Reviens sur la première fenêtre (le dossier de sauvegarde `\\VBOXSVR\Sauvegarde`)
10. Clic droit dans une zone vide → "Coller" (ou Ctrl + V)
11. Répète les étapes 6 à 10 pour le dossier Desktop (crée d'abord un sous-dossier "Desktop" dans la sauvegarde pour ne pas tout mélanger)

![Capture d'écran 2026-09-16 120457](https://hackmd.io/_uploads/rkg6O4_KGg.png)
(desktop à été ajouter par la suite : voir vidéo)

---

## 6. Vérifier l'intégrité de la sauvegarde

### 6.1 Comptage rapide
1. Dans la VM, clique sur Démarrer, tape : `terminal`
2. Clic droit → "Exécuter en tant qu'administrateur" → Oui
3. Tape, puis Entrée, pour compter les fichiers originaux :
```
   dir C:\Users\Admin\Documents /a-d | find "fichier(s)"
```
4. Note le nombre de fichiers affiché
5. Tape, puis Entrée, pour compter les fichiers dans la sauvegarde :
```
   dir \\VBOXSVR\Sauvegarde /a-d | find "fichier(s)"
```
6. Compare les deux nombres : ils doivent être identiques
![Capture d'écran 2026-09-16 122630](https://hackmd.io/_uploads/Sy6etEuKMe.png)
![Capture d'écran 2026-09-16 122423](https://hackmd.io/_uploads/rkTetN_Fzx.png)

### 6.2 Vérification par empreinte 
7. Tape, puis Entrée (remplace le nom par un vrai fichier) :
```
   certutil -hashfile "C:\Users\Admin\Documents\RapportImportant.txt" SHA256
```
8. Note l'empreinte affichée (une longue suite de lettres/chiffres)
9. Tape la même commande sur le fichier copié dans la sauvegarde :
```
   certutil -hashfile "\\VBOXSVR\Sauvegarde\RapportImportant.txt" SHA256
```
10. Compare les deux empreintes : si elles sont identiques, le fichier est intact et n'a pas été corrompu pendant la copie
![Capture d'écran 2026-09-16 123153](https://hackmd.io/_uploads/S1sft4OFze.png)

---S

## 7. Réinstaller proprement le système d'exploitation

### 7.1 Méthode recommandée pour ce projet : réinitialisation Windows native
Plus rapide que refaire une VM complète, et pédagogiquement identique (le but est de montrer que les données survivent à une remise à zéro du système).

1. Dans la VM, clique sur Démarrer → Paramètres (Windows + I)
2. Menu de gauche → "Système" → clique sur "Récupération" (dans la liste à droite)
3. À côté de "Réinitialiser ce PC", clique sur le bouton "Réinitialiser le PC"
4. Une fenêtre propose deux choix : "Conserver mes fichiers" ou "Supprimer tout"
5. Clique sur "Supprimer tout" (c'est important : on veut simuler une vraie réinstallation propre, la sauvegarde externe doit être la seule protection des données)
6. Écran suivant : choisis "Téléchargement cloud" ou "Réinstallation locale" — prends "Réinstallation locale" (plus rapide, pas besoin d'internet)
7. Un écran récapitulatif liste ce qui va se passer → clique "Suivant"
8. Clique "Réinitialiser"
9. Windows redémarre et efface tout, puis réinstalle le système (15-30 minutes, la VM redémarre plusieurs fois seule)
10. Une fois terminé, tu retombes sur les écrans de configuration initiale de Windows (région, clavier, compte...) — reprends la procédure `oobe\bypassnro` du Projet 1 pour recréer un compte local

![Capture d'écran 2026-09-16 123819](https://hackmd.io/_uploads/rkTVKVutMg.png)


### 7.2 Alternative : réinstallation complète depuis l'ISO
Si tu préfères repartir totalement de zéro (VM neuve) :
1. Éteins et supprime la VM actuelle dans VirtualBox (clic droit → "Supprimer" → "Supprimer tous les fichiers")
2. Recrée une VM neuve en suivant exactement les étapes de la section 0 de ce document et du Projet 1 (installation Windows 11 complète)

---

## 8. Réinstaller les logiciels nécessaires

1. Une fois le nouveau compte local recréé (Box), connecte-toi dessus
2. Ouvre Edge, tape `libreoffice.org/download` → Entrée
3. Clique sur le bouton vert "Télécharger"
4. Ouvre le fichier `.msi` téléchargé → "Oui" (contrôle de compte)
5. Clique "Suivant" sur chaque écran (installation "Typique") → "Installer" → "Terminer"
6. Réinstalle tout autre logiciel qui figurait dans ton inventaire (section 4) de la même façon

![Capture d'écran 2026-09-16 143310](https://hackmd.io/_uploads/r1d4bSuKfl.png)
![Capture d'écran 2026-09-16 143238](https://hackmd.io/_uploads/B1_NWrOFfe.png)

---

## 9. Restaurer les données et vérifier leur intégrité

### 9.1 Reconnecter le dossier partagé
1. Si le dossier partagé `\\VBOXSVR\Sauvegarde` n'apparaît plus après la réinitialisation, réinstalle les Guest Additions (répète la section 1 de ce document) — la réinitialisation "Supprimer tout" efface aussi ce composant

### 9.2 Copier les données depuis la sauvegarde vers le nouveau système
2. Ouvre l'Explorateur de fichiers, tape dans la barre d'adresse : `\\VBOXSVR\Sauvegarde` → Entrée
3. Sélectionne tous les fichiers du dossier (Ctrl + A)
4. Clic droit → "Copier" (ou Ctrl + C)
5. Ouvre une deuxième fenêtre Explorateur, va dans `C:\Users\box\Documents`
6. Clic droit dans une zone vide → "Coller" (ou Ctrl + V)
7. Fais de même pour le sous-dossier "Desktop" de la sauvegarde vers le vrai Bureau

### 9.3 Vérifier l'intégrité après restauration
8. Ouvre le Terminal en administrateur (Démarrer → `terminal` → clic droit → "Exécuter en tant qu'administrateur" → Oui)
9. Refais la vérification par empreinte comme à l'étape 6.2, mais cette fois compare le fichier restauré avec le fichier de la sauvegarde :
```
   certutil -hashfile "C:\Users\box\Documents\RapportImportant.txt" SHA256
```
   puis
```
  certutil -hashfile "Z:\Sauvegarde VM\Rapport important.txt" SHA256
```
10. Les deux empreintes doivent être identiques → preuve que la restauration n'a rien corrompu

![image](https://hackmd.io/_uploads/rJ9ChNuFze.png)
![image](https://hackmd.io/_uploads/Hyq8TEuKMx.png)
![image](https://hackmd.io/_uploads/BJUGzHdYGx.png)


---

## 10. Reconfigurer les paramètres essentiels

1. Recrée les Favoris du navigateur Edge (ou importe-les si tu as sauvegardé le fichier Bookmarks à l'étape 5)
2. Renomme la machine si besoin (`Paramètres` → `Système` → `À propos` → "Renommer ce PC") 

4. Réactive les mises à jour, l'antivirus et le pare-feu si nécessaire (voir Projet 1, section 7)

![image](https://hackmd.io/_uploads/BklCCVuFMe.png)
![image](https://hackmd.io/_uploads/BkWgkS_FGg.png)
![image](https://hackmd.io/_uploads/S17vJBdFGg.png)



---

## 11. Rédiger la procédure de migration type

```
# Procédure de migration et sauvegarde d'un poste — Modèle réutilisable

## 1. Inventaire
- Documents utilisateur : C:\Users\<nom>\Documents
- Bureau : C:\Users\<nom>\Desktop
- Images : C:\Users\<nom>\Pictures
- Favoris navigateur : dossier AppData du navigateur concerné
- Liste des logiciels installés (Paramètres > Applications)

## 2. Sauvegarde
- Support utilisé : dossier partagé
- Méthode de copie : Ctrl C + Ctrl V
- Date de la sauvegarde : 16.09.2026

## 3. Vérification d'intégrité
- Méthode : comptage de fichiers + empreinte SHA256 (certutil -hashfile)
- Résultat : Conforme 

## 4. Réinstallation
- Méthode utilisée : Réinitialisation Windows 
- Édition installée : Windows 11 Professionnel
- Compte(s) recréé(s) : 1 

## 5. Restauration des données
- Emplacement restauré : C:\Users\box\Documents et C:\Users\box\Desktop
- Vérification d'intégrité post-restauration : Conforme

## 6. Reconfiguration
- Nom de machine : boxbox
- Paramètres réseau : NAT
- Sécurité (updates/antivirus/pare-feu) : Vérifiés

## 7. Durée totale de l'opération
5h 

---
````

https://drive.google.com/drive/folders/1pOKbDp-EY56CiKfJG4n0r4hFn0mMXTmH?usp=sharing