# TP FINAL PARTIE 2 
## Cahier des charges 1-Dimensionnements de la plateforme et choix du processeur

---

## 1. Documentation des calculs sous Excel

Pour faire mes calculs, j'ai créé un classeur Excel avec un onglet par étape. Tous les onglets vont chercher leurs données dans l'onglet Inventaire : si une VM change, tout se recalcule automatiquement.

Onglets du classeur : **Inventaire**, **CPU**, **RAM**, **Stockage**, **Hôte**, **Processeur**, **Récap**.

### 1.2 Onglet Inventaire

J'ai commencé par recopier le tableau des 15 VMs de la consigne, à partir de la cellule A1. La ligne 1 contient les titres et les lignes 2 à 16 les VMs.

![image](https://hackmd.io/_uploads/rJMQEgVqMe.png)

J'ai vérifié que les chiffres étaient bien alignés à droite, pour m'assurer qu'Excel les reconnaissait comme des nombres et pas comme du texte.



###  Onglet CPU – Étape 1

Les VMs critiques et standard n'ont pas le même ratio d'overcommit, donc je les ai calculées séparément.

| Cellule | Formule | Résultat |
|---|---|---|
| B4 | `=SOMME.SI(Inventaire!H2:H16;"Critique";Inventaire!E2:E16)` | 20 vCPU |
| C4 | `1` (ratio 1:1) | |
| D4 | `=ARRONDI.SUP(B4/C4;0)` | 20 threads |
| B5 | `=SOMME.SI(Inventaire!H2:H16;"Standard";Inventaire!E2:E16)` | 40 vCPU |
| C5 | `3` (ratio 3:1) | |
| D5 | `=ARRONDI.SUP(B5/C5;0)` | 14 threads |
| B6 / D6 | `=B4+B5` / `=D4+D5` | 60 vCPU / 34 threads |
| B8 | `0,3` (croissance de 30 %) | |
| D9 | `=ARRONDI.SUP(D6*(1+B8);0)` | **45 threads** |

- **SOMME.SI** additionne les vCPU uniquement sur les lignes « Critique » ou « Standard ».
- **ARRONDI.SUP** arrondit à l'entier supérieur, car on ne peut pas avoir une fraction de thread.
![image](https://hackmd.io/_uploads/BkRwNeV5fg.png)



### 10.3 Onglet RAM – Étape 2

L'overcommit RAM étant interdit, j'ai simplement additionné toute la RAM.

| Cellule | Formule | Résultat |
|---|---|---|
| B3 | `=SOMME(Inventaire!F2:F16)` | 272 Go |
| B4 | `=CPU!B8` | 30 % |
| B5 | `=ARRONDI.SUP(B3*(1+B4);0)` | **354 Go** |

La cellule B4 va chercher la croissance dans l'onglet CPU. Comme ça, la valeur n'est saisie qu'à un seul endroit.

![image](https://hackmd.io/_uploads/r19FVgN9Me.png)


### 10.4 Onglet Stockage – Étape 3

| Cellule | Formule | Résultat |
|---|---|---|
| B3 | `=SOMME(Inventaire!G2:G16)` | 4 720 Go |
| B4 | `=CPU!B8` | 30 % |
| B5 | `=B3*(1+B4)` | 6 136 Go |
| B6 | `0,2` (espace libre) | |
| B7 | `=B5/(1-B6)` | 7 670 Go |
| B8 | `=B7/1000` | **7,67 To utiles** |
| B9 | `=B8*2` | 15,34 To (RAID 10) |
| B10 | `16` | **16 To bruts à acheter** |

- Je divise par 0,8 (et pas × 1,2) car les données ne doivent occuper que 80 % du volume.
- Je multiplie par 2 car le RAID 10 écrit chaque donnée en double.

![image](https://hackmd.io/_uploads/Skbi4eE5Gx.png)

### 10.5 Onglet Hôte – Étape 4

Règle N+1 : je divise la charge par 2 hôtes (et non 3), comme si un hôte était déjà en panne, puis j'ajoute la réserve de l'hyperviseur.

| Cellule | Formule | Résultat |
|---|---|---|
| B3 | `3` | Nombre d'hôtes |
| B4 | `=B3-1` | 2 hôtes restants |
| B6 | `=CPU!D9` | 45 threads |
| B7 | `=ARRONDI.SUP(B6/B4;0)` | 23 threads |
| B8 | `4` | Réserve hyperviseur |
| B9 | `=B7+B8` | **27 threads par hôte** |
| B11 | `=RAM!B5` | 354 Go |
| B12 | `=ARRONDI.SUP(B11/B4;0)` | 177 Go |
| B13 | `8` | Réserve hyperviseur |
| B14 | `=B12+B13` | **185 Go par hôte** |
| B16 | `=MAX(Inventaire!E2:E16)` | 8 vCPU |
| B17 | `=MAX(Inventaire!F2:F16)` | 64 Go (SRV-SQL01) |
| B18 | `=SI(ET(B16<=B7;B17<=B12);"Oui";"Non")` | **Oui** |
| B20 | `256` | RAM retenue |
| B21 | `8 x 32 Go` | Configuration |
| B22 | `=B20-B14` | 71 Go de marge |

- **MAX** trouve la plus grosse VM (le serveur SQL, en ligne 4 de l'inventaire).
- **SI(ET(...))** vérifie qu'elle tient sur un seul hôte en threads et en RAM.

![image](https://hackmd.io/_uploads/B1hWBe4czl.png)


### 10.6 Onglet Processeur – Étape 5

J'ai recopié le catalogue des 5 processeurs (lignes 4 à 8), puis ajouté des colonnes de calcul. J'ai tapé les formules en ligne 4, puis je les ai recopiées jusqu'à la ligne 8 avec la poignée de recopie.

| Cellule | Formule | Rôle |
|---|---|---|
| I4 | `=SI(ET(C4="x86-64";E4>=Hôte!$B$9);"Oui";"Non")` | Compatible si x86 et au moins 27 threads |
| K4 | `=SI(I4="Oui";H4*Hôte!$B$3;"-")` | Coût pour 3 hôtes |
| L4 | `=SI(I4="Oui";G4*Hôte!$B$3;"-")` | Consommation pour 3 hôtes |
| M4 | `=SI(I4="Oui";E4-Hôte!$B$9;"-")` | Marge de threads |
| B10 | `=INDEX(B4:B8;EQUIV(MIN(K4:K8);K4:K8;0))` | **EPYC 9124** |

- Les **$** bloquent la référence à la cellule de l'onglet Hôte quand on recopie la formule vers le bas.
- **INDEX/EQUIV** retrouve le nom du processeur compatible le moins cher.

![image](https://hackmd.io/_uploads/SJjYSxVcfl.png)


### 10.7 Onglet Récap

Cet onglet regroupe les résultats finaux en allant chercher les valeurs dans les autres onglets.

| Cellule | Formule | Résultat |
|---|---|---|
| B4 / C4 / D4 | `=CPU!D9` / `=Hôte!B9` / `=Processeur!B10` | 45 / 27 / EPYC 9124 |
| B5 / C5 / D5 | `=RAM!B5` / `=Hôte!B14` / `=Hôte!B21` | 354 / 185 / 8 x 32 Go |
| B6 / D6 | `=Stockage!B8` / `=Stockage!B10&" To bruts en RAID 10"` | 7,67 / 16 To bruts en RAID 10 |

Le **&** permet d'ajouter du texte après un nombre.

![image](https://hackmd.io/_uploads/HyBiSxV9ze.png)


### 10.8 Difficultés rencontrées

- **#VALEUR! dans l'onglet CPU :** j'avais saisi « 30% » et Excel l'avait enregistré comme du texte (aligné à gauche). Je l'ai remplacé par `0,3` au format pourcentage, et le calcul a fonctionné.
- **Erreur de formule dans l'onglet Processeur :** je tapais la formule du coût dans la mauvaise cellule (I5 au lieu de K4), avec des fautes de frappe (`"Oui$I"`, un `*` manquant, une parenthèse en trop). J'ai aussi dû adapter les formules au nom exact de mes onglets (**Hôte** et **Processeur**, sans « s »).


Vidéo récapitulative de tout le procédés :

https://drive.google.com/drive/folders/1hhnIZ39UXy0vtAMgIlZMUbXlLIPKLNBh?usp=sharing