# Créer la machine virtuelle1. Ouvrez VirtualBox



2. Cliquez sur le bouton "Nouvelle" (icône avec une étoile bleue, en haut à gauche de la fenêtre)
3. Une fenêtre "Créer une machine virtuelle" s'ouvre :
   - Dans le champ "Nom", tapez : SalleInfo-MultiUtilisateurs
   - Vérifiez que "Type" affiche "Microsoft Windows" (sinon cliquez sur le menu déroulant et sélectionnez-le)
   - Vérifiez que "Version" affiche "Windows 11 (64-bit)" (sinon cliquez sur le menu déroulant et sélectionnez-le)
   - Cliquez sur le bouton "Suivant" en bas à droite
4. Écran "Matériel" :
   - Cliquez-glissez le curseur "Mémoire de base" jusqu'à afficher 4096 Mo (ou tapez directement 4096 dans la petite case numérique à droite du curseur)
   - Dans le champ "Processeurs", réglez sur 2
   - Cliquez "Suivant"
5. Écran "Disque dur" :
   - Vérifiez que "Créer un disque dur virtuel maintenant" est coché (rond rempli)
   - Cliquez "Suivant"
6. Écran "Disque dur virtuel" : 
   - Vérifiez que "VDI (VirtualBox Disk Image)" est coché → cliquez "Suivant"
   - Vérifiez que "Dynamiquement alloué" est coché → cliquez "Suivant"
   - Dans le champ taille, tapez ou glissez jusqu'à 64,00 Go
   - Cliquez sur le bouton "Créer"
7. Vous revenez à la fenêtre principale de VirtualBox : votre VM apparaît dans la colonne de gauche avec le statut "Éteinte"

Vidéo 1 (drive ci-dessous)

## Réglages obligatoires pour Windows 11
8. Cliquez une seule fois sur le nom de votre VM dans la colonne de gauche (elle se surligne en bleu)
9. Cliquez sur le bouton "Configuration" (icône engrenage jaune, en haut de la fenêtre)
10. Dans la fenêtre qui s'ouvre, cliquez sur "Système" dans la liste de catégories à gauche
11. En haut de la zone de droite, cliquez sur l'onglet "Carte mère"
12. Dans la zone "Fonctionnalités étendues", cochez la case "Activer l'EFI (spéciaux OS uniquement)"
13. Cliquez sur l'onglet "Processeur" (juste à côté de "Carte mère")
14. Vérifiez que la case "Activer PAE/NX" est cochée
15. Toujours dans "Système", cherchez une section ou un onglet nommé "TPM"
16. Sélectionnez "TPM 2.0" dans le menu proposé
17. Cochez la case "Secure Boot" si elle est visible
18. Cliquez sur "Stockage" dans la liste de catégories à gauche
19. Dans la zone centrale "Arborescence de stockage", repérez la ligne avec une icône de disque bleu et le mot "Vide" dessous
20. Cliquez une fois sur cette ligne "Vide" pour la sélectionner
21. À droite, cliquez sur la petite icône de disque bleu à côté du menu déroulant "Lecteur optique"
22. Dans le menu qui apparaît, cliquez sur "Choisir un fichier de disque..."
23. Une fenêtre d'explorateur s'ouvre → naviguez jusqu'à votre dossier Téléchargements
24. Cliquez une fois sur votre fichier Win11....iso puis cliquez "Ouvrir"
25. Cliquez sur "Réseau" dans la liste de catégories à gauche
26. Onglet "Adaptateur 1" : vérifiez que "Activer la carte réseau" est coché
27. Cliquez sur le menu déroulant "Mode d'accès réseau" et sélectionnez "Accès par pont"
28. Cliquez sur le bouton "OK" en bas de la fenêtre

1. Dans la fenêtre principale VirtualBox, votre VM doit être sélectionnée en bleu
2. Cliquez sur le bouton "Démarrer" (icône flèche verte, en haut)
3. Une nouvelle fenêtre noire s'ouvre : c'est l'écran de la VM
4. Si le message "Press any key to boot from CD or DVD" apparaît, cliquez dans la fenêtre puis appuyez immédiatement sur une touche du clavier
5. Écran "Installation de Windows" :
   - Menu "Langue à installer" : Français (France)
   - Menu "Format horaire et monétaire" : Français (suisse)
   - Menu "Clavier ou méthode d'entrée" : Suisse (france)
   - Cliquez "Suivant"
6. Cliquez sur le grand bouton "Installer maintenant" au centre de l'écran
7. Cliquez sur le lien "Je n'ai pas de clé de produit" en bas de l'écran
8. Cliquez sur "Windows 11 Pro" pour le sélectionner (il se surligne en bleu) puis cliquez "Suivant"
9. Cochez la case "J'accepte les termes du contrat de licence" en bas à gauche puis cliquez "Suivant"
10. Cliquez sur "Personnalisée : installer uniquement Windows (avancé)" (la deuxième option)
11. Cliquez une fois sur la ligne "Lecteur 0 Espace non alloué" pour la sélectionner puis cliquez "Suivant"
12. L'installation démarre automatiquement : une barre de progression défile avec plusieurs étapes et la VM redémarre seule plusieurs fois — ne touchez à rien, attendez 15 à 25 minutes

Vidéo 2 (drive ci-dessous)

## Contourner le compte Microsoft obligatoire
13. Écran "Est-ce la bonne région ?" : vérifiez Suisse ou France → cliquez "Oui"
14. Écran "Disposition ou configuration du clavier" : laissez Français (Suisse) ou équivalent → cliquez "Oui"
15. Écran "Voulez-vous ajouter une deuxième disposition de clavier ?" → cliquez "Ignorer" en bas
16. Vous arrivez sur l'écran "Configuration réseau requise" (liste de réseaux Wi-Fi visible même en VM)
17. Appuyez simultanément sur les touches Maj (Shift) et F10 de votre clavier
18. Une fenêtre noire "Invite de commandes" s'ouvre par-dessus l'écran
19. Tapez exactement, puis appuyez sur Entrée :
    oobe\bypassnro
20. L'écran devient noir quelques secondes puis la VM redémarre automatiquement — attendez
21. Vous revenez à l'écran "Configuration réseau requise" une seconde fois : cliquez sur le lien "Je n'ai pas de connexion Internet" en bas à gauche
22. Cliquez sur "Continuer avec une configuration limitée"
23. Écran "Qui va utiliser cet appareil ?" : dans le champ texte, tapez Maintenance → cliquez "Suivant"
24. Écran mot de passe : tapez un mot de passe d'au moins 8 caractères (ex: <MOT_DE_PASSE> → cliquez "Suivant", retapez-le pour confirmer → cliquez "Suivant"
25. Trois écrans de questions de sécurité apparaissent : pour chacun, cliquez sur le menu déroulant pour choisir une question, tapez une réponse dans le champ en dessous, cliquez "Suivant" (répétez 3 fois)
26. Écrans "Paramètres de confidentialité" (plusieurs pages) : pour chaque page, basculez les interrupteurs sur "Non" (ils deviennent gris) puis cliquez "Suivant" en bas à droite
27. Si un écran propose des applications du Microsoft Store, cliquez "Ignorer pour l'instant"
28. Un écran "Ça y est presque..." avec un cercle qui tourne s'affiche → attendez 2 à 5 minutes
29. Le Bureau Windows 11 apparaît (fond bleu, barre des tâches en bas) → l'installation est terminée

Vidéo 2 (drive ci-dessous)
Ce compte existe déjà : c'est "Maintenance" créé à l'étape 23 ci-dessus, automatiquement administrateur.
Pour le confirmer :

1. Cliquez sur le bouton Démarrer (logo Windows, centre de la barre des tâches)
2. Tapez au clavier : panneau de configuration
3. Appuyez sur Entrée
4. Dans la fenêtre qui s'ouvre, cliquez sur "Comptes d'utilisateurs"
5. Cliquez à nouveau sur "Comptes d'utilisateurs"
6. Vous voyez une ligne avec le nom "Maintenance" et, en dessous ou à côté, la mention "Administrateur"

![image](https://hackmd.io/_uploads/r1QAB_8tzx.png)

À répéter 3 fois en entier, avec un nom différent à chaque fois (Etudiant1, puis Etudiant2, puis Etudiant3) :

1. Cliquez sur Démarrer, puis sur l'icône engrenage "Paramètres" (ou appuyez sur Windows + I)
2. Dans le menu de gauche de la fenêtre Paramètres, cliquez sur "Comptes"
3. Dans la liste à droite, cliquez sur "Autres utilisateurs"
4. À côté de "Ajouter un autre utilisateur", cliquez sur le bouton "Ajouter un compte"
5. Une fenêtre demande un e-mail → cliquez sur le petit lien "Je ne dispose pas des informations de connexion de cette personne" tout en bas
6. Écran suivant → cliquez sur le lien "Ajouter un utilisateur sans compte Microsoft" en bas
7. Champ "Qui va utiliser cet appareil ?" : tapez Etudiant1 → cliquez "Suivant"
8. Champ mot de passe : tapez un mot de passe d'au moins 8 caractères → cliquez "Suivant", confirmez-le → cliquez "Suivant"
9. Choisissez 3 questions de sécurité avec réponses (comme précédemment) → "Suivant" à chaque fois
10. Vous revenez à la page "Autres utilisateurs" : le compte Etudiant1 apparaît dans la liste
11. Cliquez sur le nom Etudiant1 pour dérouler ses options
12. Cliquez sur le bouton "Modifier le type de compte" qui apparaît
13. Vérifiez que le menu déroulant affiche "Utilisateur standard" (sinon cliquez dessus et sélectionnez-le)
14. Cliquez "OK"

Recommencez entièrement les étapes 4 à 14 pour Etudiant2, puis à nouveau pour Etudiant3.

Vidéo 3. (drive ci-dessous)

## Créer les dossiers
1. Assurez-vous d'être connecté avec le compte Maintenance (regardez en bas à gauche du menu Démarrer, ou déconnectez-vous et reconnectez-vous avec ce compte si besoin)
2. Cliquez sur l'icône "Explorateur de fichiers" dans la barre des tâches (dossier jaune)
3. Dans le menu de gauche de la fenêtre qui s'ouvre, cliquez sur "Ce PC"
4. Double-cliquez sur "Windows (C:)"
5. Faites un clic droit dans une zone vide de la fenêtre (espace blanc, pas sur un fichier)
6. Dans le menu qui apparaît, cliquez sur "Nouveau" puis sur "Dossier"
7. Un dossier nommé "Nouveau dossier" apparaît avec le nom en surbrillance modifiable → tapez PosteEtudiants puis appuyez sur Entrée
8. Double-cliquez sur ce dossier PosteEtudiants pour entrer dedans
9. Refaites clic droit → Nouveau → Dossier → tapez Etudiant1 → Entrée
10. Refaites la même chose pour créer Etudiant2
11. Refaites la même chose pour créer Etudiant3
    (vous devez maintenant avoir 3 sous-dossiers visibles dans PosteEtudiants)
Vidéo 3.

## Restreindre l'accès au dossier Etudiant1
12. Faites un clic droit sur le dossier Etudiant1
13. Cliquez sur "Propriétés" (généralement tout en bas du menu)
14. Dans la fenêtre qui s'ouvre, cliquez sur l'onglet "Sécurité" (en haut de la fenêtre)
15. Cliquez sur le bouton "Avancé" en bas de cet onglet
16. Une fenêtre "Paramètres de sécurité avancés pour Etudiant1" s'ouvre
17. En haut de cette fenêtre, cliquez sur le bouton "Désactiver l'héritage"
18. Une petite fenêtre de choix apparaît → cliquez sur "Convertir les autorisations héritées en autorisations explicites sur cet objet"
19. Cliquez "OK" en bas de la fenêtre "Paramètres de sécurité avancés"
20. Vous revenez sur l'onglet "Sécurité" de la fenêtre Propriétés → cliquez sur le bouton "Modifier"
21. Une nouvelle fenêtre "Autorisations pour Etudiant1" s'ouvre, avec une liste de groupes/utilisateurs en haut
22. Cliquez une fois sur "Utilisateurs" (ou "Users") dans cette liste pour le sélectionner
23. Cliquez sur le bouton "Supprimer" juste en dessous de la liste
24. Cliquez sur le bouton "Ajouter..."
25. Une fenêtre "Sélectionner Utilisateurs ou Groupes" s'ouvre → dans le grand champ texte en bas, tapez Etudiant1
26. Cliquez sur le bouton "Vérifier les noms" → le nom se souligne s'il est reconnu correctement
27. Cliquez "OK"
28. Le compte Etudiant1 apparaît maintenant dans la liste → cliquez dessus pour le sélectionner
29. Dans le tableau "Autorisations pour Etudiant1" en dessous, repérez la ligne "Contrôle total" et cochez la case dans la colonne "Autoriser"
30. Cliquez "OK"
31. Cliquez "OK" à nouveau pour fermer complètement la fenêtre Propriétés

Vidéo 4.

## Répéter pour les 2 autres dossiers
32. Refaites entièrement les étapes 12 à 31 pour le dossier Etudiant2, en tapant Etudiant2 partout où c'était écrit Etudiant1
33. Refaites entièrement les étapes 12 à 31 pour le dossier Etudiant3, en tapant Etudiant3 partout où c'était écrit Etudiant1
![image](https://hackmd.io/_uploads/HJjlZK8Fzx.png)

1. Connecté en tant que Maintenance, cliquez sur Démarrer
2. Tapez au clavier : secpol.msc
3. Appuyez sur Entrée
4. Une fenêtre "Contrôle de compte d'utilisateur" apparaît → cliquez "Oui"
5. Dans la fenêtre "Stratégie de sécurité locale", dans le menu de gauche, double-cliquez sur "Stratégies locales" pour dérouler la liste
6. Cliquez une fois sur "Options de sécurité"
7. Dans la longue liste qui s'affiche à droite, faites défiler vers le bas jusqu'à trouver la ligne "Console d'ouverture de session interactive : délai d'inactivité pour le verrouillage de la machine"
8. Double-cliquez sur cette ligne
9. Une fenêtre s'ouvre avec un champ numérique "Secondes" → effacez la valeur et tapez 300
10. Cliquez "OK"
11. Fermez la fenêtre "Stratégie de sécurité locale" (croix en haut à droite)

![image](https://hackmd.io/_uploads/SJ5xQ-vYfe.png)
InactivityTimeoutSecs : 300 confirme que le réglage est bien enregistré dans le Registre — le verrouillage automatique après 5 minutes d'inactivité (300 secondes) est maintenant actif.
vidéo 19 (drive ci-dessous)
## Si secpol.msc n'a pas cette option (édition Famille)
12. Cliquez sur Démarrer → Paramètres (Windows + I)
13. Menu de gauche → "Personnalisation"
14. Cliquez sur "Écran de verrouillage"
15. Cliquez sur "Délai d'expiration de l'écran" (le nom exact peut varier selon la version)
16. Sous "Écran", cliquez sur le menu déroulant et sélectionnez "5 minutes"
17. Revenez en arrière (flèche en haut à gauche), cliquez sur "Économiseur d'écran"
18. Cliquez sur le menu déroulant et choisissez "Vide" (ou un autre)
19. Dans le champ "Attendre", tapez ou réglez 5 minutes
20. Cochez la case "À la reprise, afficher l'écran de connexion"
21. Cliquez "OK"

1. Cliquez sur Démarrer, tapez : terminal
2. Faites un clic droit sur "Terminal" dans les résultats
3. Cliquez sur "Exécuter en tant qu'administrateur"
4. Une fenêtre "Contrôle de compte d'utilisateur" apparaît → cliquez "Oui"
5. Dans la fenêtre bleu foncé qui s'ouvre, tapez exactement puis appuyez sur Entrée :
   net accounts /minpwlen:8
6. Le message "La commande s'est terminée correctement." doit s'afficher
7. Tapez ensuite, puis Entrée :
   net accounts /maxpwage:90
8. Le même message de confirmation doit s'afficher


## Test avec Etudiant1
1. Cliquez sur Démarrer
2. En haut du menu Démarrer, cliquez sur votre icône/nom de compte (petit cercle, en haut à gauche du menu)
3. Un petit menu apparaît → cliquez sur "Se déconnecter"
4. Sur l'écran de connexion, cliquez sur le nom "Etudiant1"
5. Tapez son mot de passe dans le champ → appuyez sur Entrée
6. Une fois sur le Bureau, cliquez sur l'icône "Explorateur de fichiers" dans la barre des tâches
7. Cliquez sur "Ce PC" dans le menu de gauche, puis double-cliquez sur "Windows (C:)"
8. Double-cliquez sur le dossier "PosteEtudiants"
9. Double-cliquez sur le dossier "Etudiant1" : il doit s'ouvrir normalement, vous voyez son contenu (vide pour l'instant, c'est normal)
10. Revenez en arrière (flèche retour en haut à gauche de la fenêtre)
11. Double-cliquez sur le dossier "Etudiant2" : un message d'erreur doit apparaître, du type "Vous n'êtes pas actuellement autorisé à accéder à ce dossier", avec un bouton "Continuer" (qui demanderait un mot de passe administrateur)
12. Ne cliquez PAS sur "Continuer" — cliquez sur "Annuler" à la place
13. Faites une capture d'écran de ce message avant de cliquer Annuler
![image](https://hackmd.io/_uploads/BkxDk3LKMe.png)
![Capture d'écran 2026-09-15 134229](https://hackmd.io/_uploads/HJqv13IYfe.png)
Vidéo 18 (drive ci-dessous)
## Test avec Etudiant2
14. Cliquez sur Démarrer → icône de compte en haut → "Se déconnecter"
15. Cliquez sur "Etudiant2" sur l'écran de connexion, entrez son mot de passe → Entrée
16. Répétez les étapes 6 à 13 en remplaçant : vous devez pouvoir ouvrir Etudiant2, mais pas Etudiant1 ni Etudiant3
![IMG_0061](https://hackmd.io/_uploads/rkT-_fvYGg.jpg)

## Test avec Etudiant3
17. Cliquez sur Démarrer → icône de compte en haut → "Se déconnecter"
18. Cliquez sur "Etudiant3" sur l'écran de connexion, entrez son mot de passe → Entrée
19. Répétez les étapes 6 à 13 en remplaçant : vous devez pouvoir ouvrir Etudiant3, mais pas Etudiant1 ni Etudiant2
![IMG_0062](https://hackmd.io/_uploads/BJsVuzDtMx.jpg)

# Rapport technique — Architecture des comptes (Salle informatique)

## 1. Vue d'ensemble
| Élément | Détail |
|---|---|
| Système d'exploitation | Windows 11 Pro |
| Environnement | Machine virtuelle (VirtualBox) |
| Nombre de comptes | 1 administrateur + 3 utilisateurs standards |

## 2. Architecture des comptes
| Compte | Type | Rôle |
|---|---|---|
| Maintenance | Administrateur | Gestion technique, jamais utilisé au quotidien |
| Etudiant1 | Utilisateur standard | Accès à son propre dossier uniquement |
| Etudiant2 | Utilisateur standard | Accès à son propre dossier uniquement|
| Etudiant3 | Utilisateur standard | Accès à son propre dossier uniquement|

## 3. Cloisonnement mis en place
- C:\PosteEtudiants\Etudiant1 : Contrôle total réservé à Etudiant1 (héritage désactivé)
- C:\PosteEtudiants\Etudiant2 : Contrôle total réservé à Etudiant2
- C:\PosteEtudiants\Etudiant3 : Contrôle total réservé à Etudiant3
- Les profils utilisateurs (C:\Users\...) restent également privés par défaut sous Windows

## 4. Politique de sécurité
| Mesure | Valeur |
|---|---|
| Longueur minimale du mot de passe | 8 caractères |
| Expiration du mot de passe | 90 jours |
| Verrouillage automatique après inactivité | 5 minutes |

## 5. Tests de cloisonnement effectués
| Compte testé | Accès à son propre dossier | Accès aux dossiers des autres |
|---|---|---|
| Etudiant1 | Réussi | Refusé (capture jointe) |
| Etudiant2 | Réussi | Refusé (capture jointe) |
| Etudiant3 | Réussi | Refusé (capture jointe) |


## 7. Conclusion

La mise en service du poste multi-utilisateurs répond aux exigences du cahier des
charges : un compte de maintenance distinct des comptes d'usage quotidien, trois
comptes standards cloisonnés par des permissions NTFS explicites, une politique de
mot de passe (8 caractères minimum) et un verrouillage automatique après 5 minutes
d'inactivité. Les tests menés sur chaque compte étudiant confirment que l'accès
reste strictement limité à son propre espace, ce qui garantit la confidentialité
des données entre utilisateurs successifs de la salle. Le poste est prêt pour une
mise en démonstration et une utilisation en conditions réelles.

https://drive.google.com/drive/folders/1_eyoc_0ZJ5ugyIlhNg0Vh0JZ4K303KUK?usp=sharing

Vidéo résumer sur la partie étudiant