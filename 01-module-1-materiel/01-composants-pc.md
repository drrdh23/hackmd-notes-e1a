# Composants d'un PC — Caractéristiques

## Composants essentiels
---
### Carte mère
---

Le "squelette" du PC : relie tous les composants essentiels entre eux.


## Format (facteur de forme)
La taille et la disposition physique de la carte mère. Détermine quel boîtier peut l'accueillir.
- **ATX** : format standard, le plus courant, pour PC de bureau classiques
- **Micro-ATX (mATX)** : plus petit, moins de slots, pour PC compacts
- **Mini-ITX** : très petit, idéal pour PC ultra-compacts, mais peu de place pour les extensions
- **E-ATX** : plus grand que l'ATX, pour configurations puissantes (plusieurs GPU, beaucoup de RAM)

## Socket
L'emplacement physique où se branche le processeur. **Le socket doit obligatoirement correspondre au CPU choisi**, sinon ça ne rentre pas.
- Exemples Intel : LGA 1700, LGA 1200
- Exemples AMD : AM5, AM4
- Chaque génération de processeur utilise souvent un socket différent

## Slots RAM
Les emplacements où l'on insère les barrettes de mémoire vive.
- Nombre de slots (2 ou 4 en général)
- Capacité maximale supportée (ex : jusqu'à 128 Go)
- Type de RAM compatible (DDR4 ou DDR5, pas interchangeables)
- Fréquence maximale supportée (MHz)

## Ports PCIe
Les connecteurs qui permettent d'ajouter des cartes d'extension (carte graphique, carte réseau, carte son...).
- **PCIe x16** : le plus grand, utilisé pour la carte graphique
- **PCIe x1** : plus petit, pour cartes réseau, cartes son, etc.
- La version (PCIe 3.0, 4.0, 5.0) influence la vitesse de transfert des données

## Chipset
Le "cerveau secondaire" qui gère la communication entre le CPU et les autres composants (RAM, stockage, USB...).
- Détermine les fonctionnalités disponibles (overclocking possible ou non, nombre de ports USB, etc.)
- Exemples Intel : B760, Z790
- Exemples AMD : B650, X670
- Certains chipsets sont "haut de gamme" (plus de fonctionnalités) et d'autres "basiques"

## Autres éléments souvent présents
- **Ports SATA** : pour brancher disques durs/SSD classiques
- **Ports M.2** : pour SSD rapides (format NVMe)
- **Ports USB** (arrière et façade)
- **Connecteurs d'alimentation** : pour recevoir le courant du bloc d'alimentation
- **BIOS/UEFI** : le petit programme intégré qui démarre le PC avant même le système d'exploitation

---
# GPU (Carte graphique)
---

Le GPU (*Graphics Processing Unit*) est le composant chargé de calculer et générer les images affichées à l'écran. Il est optimisé pour effectuer énormément de calculs en parallèle, ce qui le rend essentiel pour les jeux, la vidéo, le graphisme 3D, et de plus en plus pour l'intelligence artificielle.

## Deux types de GPU

**GPU intégré (iGPU)**
- Directement intégré au processeur (pas de carte séparée)
- Consomme moins d'énergie, chauffe moins
- Suffisant pour la bureautique, la vidéo, et les jeux légers
- Partage la RAM du système (pas de mémoire vidéo dédiée)

**GPU dédié (carte graphique séparée)**
- Carte à part entière, branchée dans un port PCIe x16
- Possède sa propre mémoire vidéo (VRAM) et son propre système de refroidissement
- Indispensable pour le gaming exigeant, le montage vidéo, la 3D, l'IA

## Caractéristiques principales

**Mémoire vidéo (VRAM)**
- Mémoire dédiée au GPU, stocke les textures et données graphiques
- Capacité (Go) : 4-6 Go (basique), 8-12 Go (courant/gaming), 16 Go+ (haut de gamme)
- Type : GDDR6, GDDR6X (plus récent et rapide)

**Puce graphique (GPU chip)**
- Le "cœur" de la carte, fabriqué par NVIDIA ou AMD
- Exemples : NVIDIA GeForce RTX série, AMD Radeon RX série

**Fréquence (MHz/GHz)**
- Vitesse de calcul de la puce
- Souvent avec une fréquence de base et une fréquence "boost"

**Bus mémoire (bits)**
- Largeur du canal de communication entre le GPU et sa VRAM
- Plus il est large (ex : 256 bits vs 128 bits), plus les données circulent vite

**Nombre de cœurs CUDA / Stream Processors**
- Unités de calcul parallèles dédiées au GPU (équivalent des "cœurs" du CPU, mais en beaucoup plus grand nombre)
- Plus il y en a, plus la carte est puissante en théorie

**TDP (consommation)**
- Puissance nécessaire en Watts, influence le choix du bloc d'alimentation
- Les cartes haut de gamme peuvent nécessiter 300W+ à elles seules

**Sorties vidéo**
- HDMI, DisplayPort (parfois encore DVI sur d'anciens modèles)
- Déterminent combien et quel type d'écrans peuvent être branchés

**Refroidissement**
- Ventilateurs intégrés (1, 2 ou 3 selon les modèles)
- Certaines cartes haut de gamme utilisent un watercooling dédié

**Ray Tracing / DLSS-FSR**
- **Ray Tracing** : technologie de calcul de lumière réaliste (reflets, ombres précises)
- **DLSS (NVIDIA) / FSR (AMD)** : technologies d'upscaling par IA qui améliorent les performances sans trop sacrifier la qualité d'image

## Facteurs de compatibilité à vérifier
- **Taille physique** : certaines cartes sont très longues/larges, doivent rentrer dans le boîtier
- **Alimentation** : le PSU doit fournir assez de puissance et avoir les bons connecteurs (6/8 broches PCIe)
- **Port PCIe** : la carte mère doit avoir un port PCIe x16 disponible

## Rôle technique
Le GPU décharge le CPU des calculs liés à l'image : sans lui (ou avec un GPU faible), le CPU devrait tout calculer lui-même, ce qui est beaucoup plus lent pour l'affichage complexe (jeux 3D, rendu vidéo, etc.).

---
## Processeur (CPU)
---
Le CPU (*Central Processing Unit*) est le "cerveau" du PC. Il exécute toutes les instructions et calculs nécessaires au fonctionnement des programmes et du système d'exploitation.

### Caractéristiques principales

**Nombre de cœurs**
- Un cœur = une unité de calcul indépendante
- Plus il y a de cœurs, plus le CPU peut traiter de tâches en même temps (multitâche, applications lourdes)
- Exemples : 4 cœurs (basique), 8 cœurs (courant), 16+ cœurs (haut de gamme/pro)

**Threads (fils d'exécution)**
- Grâce à des technologies comme l'Hyper-Threading (Intel) ou le SMT (AMD), un cœur physique peut gérer 2 threads
- Un CPU "8 cœurs / 16 threads" traite plus efficacement les tâches multitâches

**Fréquence (GHz)**
- Vitesse à laquelle le CPU exécute les instructions
- Plus le chiffre est élevé, plus il est rapide (à cœurs égaux)
- On distingue la fréquence de base et la fréquence "boost" (turbo, temporaire, plus élevée)

**Socket**
- Type de connecteur physique sur la carte mère
- Doit impérativement correspondre entre le CPU et la carte mère (ex : AM5 pour AMD, LGA1700 pour Intel)

**Cache**
- Petite mémoire ultra-rapide intégrée au CPU (L1, L2, L3)
- Sert à stocker temporairement des données fréquemment utilisées pour accélérer les calculs

**TDP (Thermal Design Power)**
- Indique la chaleur dégagée par le CPU (en Watts)
- Permet de choisir un refroidissement adapté

**Marque**
- **Intel** (Core i3, i5, i7, i9...)
- **AMD** (Ryzen 3, 5, 7, 9...)

---

## RAM (mémoire vive)
---
La RAM stocke temporairement les données que le PC utilise activement. Elle est beaucoup plus rapide que le stockage permanent, mais elle s'efface complètement à l'extinction du PC.

### Caractéristiques principales

**Capacité (Go)**
- Détermine combien de données/programmes peuvent être utilisés simultanément sans ralentissement
- Exemples : 8 Go (usage basique), 16 Go (courant/gaming), 32 Go+ (professionnel, montage vidéo)

**Type (génération)**
- **DDR4** : encore très répandue
- **DDR5** : plus récente, plus rapide, plus chère
- Les types ne sont **pas compatibles entre eux** ni avec toutes les cartes mères

**Fréquence (MHz)**
- Vitesse à laquelle la RAM transfère les données
- Exemples : 3200 MHz (DDR4 courant), 5600 MHz (DDR5 courant)
- Plus élevé = meilleures performances, notamment en jeu

**Latence (CL)**
- Temps de réaction de la RAM (plus le chiffre est bas, mieux c'est)
- Moins déterminant que la fréquence pour un usage courant

**Dual/Quad Channel**
- Utiliser 2 (ou 4) barrettes identiques au lieu d'une seule améliore les performances, car les données transitent sur plusieurs "voies" en parallèle

**Format**
- **DIMM** : format standard pour PC de bureau
- **SO-DIMM** : format compact pour PC portables

---

## Disque SSD
---
Le SSD (*Solid State Drive*) est un support de stockage permanent sans pièce mécanique, utilisant des puces de mémoire flash (comme une grosse clé USB, mais bien plus rapide).

### Caractéristiques principales

**Capacité (Go/To)**
- Espace de stockage disponible
- Exemples : 250 Go (basique), 500 Go-1 To (courant), 2 To+ (gros usage)

**Interface de connexion**
- **SATA** : format 2.5", vitesse limitée (~550 Mo/s max), bon rapport prix/capacité
- **NVMe (via M.2)** : beaucoup plus rapide (jusqu'à 7000 Mo/s et plus), utilise le bus PCIe directement

**Vitesse de lecture/écriture**
- Indiquée en Mo/s ou Go/s
- Détermine la rapidité de démarrage du PC, de chargement des jeux/logiciels

**Endurance (TBW – Terabytes Written)**
- Quantité totale de données pouvant être écrites avant usure significative
- Plus le TBW est élevé, plus le SSD dure dans le temps

**Format physique**
- **2.5"** : comme un disque dur classique, avec câble SATA
- **M.2** : petite carte qui s'insère directement sur la carte mère, sans câble

### Avantages face au disque dur (HDD)
- Beaucoup plus rapide
- Silencieux (pas de pièces mobiles)
- Plus résistant aux chocs
- Consomme moins d'énergie
- Plus cher au Go que le HDD

---

## Bloc d'alimentation (PSU)
---
Le PSU (*Power Supply Unit*) convertit le courant électrique de la prise murale (alternatif) en courant utilisable par les composants du PC (continu), à différentes tensions.

### Caractéristiques principales

**Puissance (Watts)**
- Indique la capacité maximale d'alimentation
- Doit être suffisante pour tous les composants, notamment la carte graphique (souvent la plus gourmande)
- Exemples : 450W (basique), 650W (courant/gaming), 850W+ (haut de gamme, multi-GPU)

**Certification 80 PLUS**
- Mesure l'efficacité énergétique (moins de perte de courant en chaleur)
- Niveaux (du moins au plus efficace) : *Bronze, Silver, Gold, Platinum, Titanium*
- Un PSU plus efficace chauffe moins et consomme moins d'électricité pour la même puissance fournie

**Modularité**
- **Non modulaire** : tous les câbles sont fixés en permanence
- **Semi-modulaire** : câbles essentiels fixes, le reste amovible
- **Modulaire** : tous les câbles sont amovibles, ce qui facilite le rangement et améliore la circulation d'air

**Connecteurs fournis**
- 24 broches (carte mère)
- 4/8 broches EPS (CPU)
- 6/8 broches PCIe (carte graphique)
- SATA power (disques)
- Molex (accessoires divers)

**Format**
- **ATX** : format standard pour PC de bureau
- **SFX** : format compact, pour boîtiers Mini-ITX

### Point important
Un bloc d'alimentation de mauvaise qualité ou sous-dimensionné peut endommager les composants ou provoquer des instabilités (redémarrages, coupures). Il est recommandé de choisir une marque fiable et une puissance légèrement supérieure aux besoins réels.

---
## Refroidissement
---
### Ventirad / dissipateur CPU
Un système (ventilateur + métal) posé sur le processeur pour évacuer la chaleur qu'il produit.
- Type (à air ou liquide/watercooling)
- Taille
- Bruit (dB)

### Ventilateurs du boîtier
Des ventilateurs placés dans le boîtier pour faire circuler l'air et évacuer la chaleur globale.
- Taille (mm)
- Vitesse (RPM)
- Débit d'air

---
## Connectique
---

## 1. Connecteurs d'alimentation
Permettent de fournir l'électricité aux composants depuis le bloc d'alimentation (PSU).
- **24 broches (24-pin)** : alimente la carte mère
- **4/8 broches (EPS)** : alimente le processeur
- **PCIe 6/8 broches** : alimente la carte graphique
- **SATA power** : alimente disques durs/SSD
- **Molex 4 broches** : ancien standard, encore utilisé pour certains ventilateurs ou accessoires

## 2. Connecteurs de stockage
Relient les disques à la carte mère pour transférer les données.
- **SATA** : câble fin, pour disques durs et SSD 2.5"
- **M.2** : connecteur direct sur la carte mère, sans câble, pour SSD rapides (NVMe)
- **IDE (ancien)** : gros câble plat à 40 fils, obsolète aujourd'hui

## 3. Connecteurs d'extension (cartes filles)
Permettent d'ajouter des cartes sur la carte mère.
- **PCIe x16** : le plus grand slot, pour la carte graphique
- **PCIe x1/x4/x8** : slots plus petits, pour cartes réseau, cartes son, contrôleurs USB...

## 4. Connecteurs externes (arrière du boîtier / I/O)
Pour brancher les périphériques externes.
- **USB-A** : rectangulaire, le plus courant
- **USB-C** : réversible, plus récent, souvent plus rapide
- **Ethernet (RJ45)** : connexion réseau filaire
- **Jack audio 3.5 mm** : micro, casque, enceintes
- **VGA** : image, analogique, ancien
- **DVI** : image, numérique
- **HDMI** : image + son, standard actuel
- **DisplayPort** : image + son, standard actuel, souvent utilisé en pro/gaming

## 5. Connecteurs internes du boîtier (façade et panneau avant)
Relient les boutons et ports situés à l'avant du boîtier à la carte mère.
- **Bouton power / reset** : petits câbles à 2 broches
- **LED d'activité** (disque dur, alimentation) : petits câbles à 2 broches
- **USB façade** : câble interne relié à un en-tête USB sur la carte mère
- **Audio façade** : câble interne relié à un en-tête audio (HD Audio) sur la carte mère

## 6. Connecteurs de ventilation
Alimentent et pilotent les ventilateurs.
- **3 broches (fan)** : vitesse fixe ou contrôlée par tension
- **4 broches (PWM)** : contrôle précis de la vitesse par la carte mère

## Résumé rapide

| Type | Rôle principal | Exemple |
|---|---|---|
| Alimentation | Fournir l'électricité | 24-pin, EPS, SATA power |
| Stockage | Transférer les données | SATA, M.2 |
| Extension | Ajouter des cartes | PCIe x16, PCIe x1 |
| Externe (I/O) | Brancher périphériques/écran | USB, HDMI, RJ45 |
| Façade | Boutons et ports avant | USB façade, audio façade |
| Ventilation | Alimenter les ventilateurs | 3-pin, 4-pin PWM |

---
### Carte réseau
---
La carte réseau (NIC – *Network Interface Card*): composant qui permet à un ordinateur de se connecter à un réseau (local ou Internet) pour échanger des données avec d'autres appareils.

## Types de cartes réseau

### Carte réseau filaire (Ethernet)
Utilise un câble physique (RJ45) pour se connecter au réseau.
- **Avantages** : connexion stable, rapide, faible latence, peu sensible aux interférences
- **Inconvénients** : nécessite un câble, moins pratique pour se déplacer
- Vitesses courantes :
  - **10/100 Mbps** : ancien standard, obsolète aujourd'hui
  - **1 Gbps (Gigabit Ethernet)** : standard actuel le plus répandu
  - **2.5 Gbps / 5 Gbps / 10 Gbps** : haut de gamme, pour usages professionnels ou gros transferts

## Carte réseau Wi-Fi
Utilise les ondes radio pour se connecter sans fil au réseau.
- **Avantages** : pas de câble, mobilité, installation simple
- **Inconvénients** : généralement plus lente, plus sensible aux interférences et à la distance
- Normes Wi-Fi (du plus ancien au plus récent) :
  - **Wi-Fi 4 (802.11n)**
  - **Wi-Fi 5 (802.11ac)**
  - **Wi-Fi 6 (802.11ax)** : plus rapide, meilleure gestion de plusieurs appareils connectés
  - **Wi-Fi 6E / Wi-Fi 7** : dernières générations, encore plus performantes

## Intégration dans le PC

- **Intégrée à la carte mère** : la majorité des cartes mères modernes ont un port Ethernet et parfois le Wi-Fi intégré
- **Carte d'extension PCIe** : carte séparée insérée dans un port PCIe, utile si la carte mère n'a pas de Wi-Fi ou pour améliorer les performances
- **Clé USB Wi-Fi** : solution externe, pratique et simple à installer sans ouvrir le boîtier

## Caractéristiques techniques principales

| Caractéristique | Description |
|---|---|
| **Vitesse maximale** | Débit théorique de transfert (ex : 1 Gbps, 2.5 Gbps) |
| **Interface** | PCIe (carte interne), USB (externe), ou intégrée à la carte mère |
| **Type de connexion** | Filaire (RJ45) ou sans fil (Wi-Fi) |
| **Adresse MAC** | Identifiant unique physique de la carte, utilisé pour l'identifier sur le réseau |
| **Norme Wi-Fi supportée** | Détermine la vitesse et la portée en sans fil (ex : Wi-Fi 6) |
| **Bluetooth** | Souvent intégré aux cartes Wi-Fi modernes, pour connecter souris, claviers, écouteurs sans fil |

## Rôle technique
La carte réseau transforme les données du PC en signaux transmissibles (électriques pour l'Ethernet, radio pour le Wi-Fi), et fait l'inverse pour les données reçues. Elle gère aussi l'**adresse MAC**, un identifiant unique qui permet au réseau de reconnaître l'appareil.


---
### VGA / DVI (et HDMI, DisplayPort)
---

Sorties vidéo vers l'écran.
- **VGA** : analogique, ancien
- **DVI** : numérique, plus récent
- **HDMI / DisplayPort** : normes actuelles, avec son

---
### Lecteur optique
---
Lit/grave CD, DVD, Blu-ray. De moins en moins présent sur les PC récents.

---

## Connecteurs audio / micro
--- 
Ce sont les prises qui permettent de faire entrer ou sortir du son sur le PC : brancher un micro, un casque, des enceintes, etc.

## Types de connecteurs
- **Jack 3.5 mm** : le connecteur audio le plus courant, rond, présent sur casques/micros/enceintes classiques
- **USB** : certains micros/casques utilisent une connexion USB (numérique) au lieu du jack
- **Optique (S/PDIF)** : connecteur plus rare, transmet un son numérique de haute qualité vers un ampli/home cinéma

## Code couleur standard (jack 3.5 mm à l'arrière du PC)
| Couleur | Fonction |
|---|---|
| **Vert** | Sortie audio (haut-parleurs/casque) |
| **Rose** | Entrée micro |
| **Bleu** | Entrée ligne (line-in, ex : platine, instrument) |
| **Noir/Gris/Orange** | Haut-parleurs supplémentaires (configurations 5.1/7.1) |

## Emplacement
- **Arrière du boîtier** : connecteurs fixes, reliés directement à la carte mère
- **Façade du boîtier** : ports pratiques à l'avant, reliés à la carte mère via un câble interne (connecteur **HD Audio**)

## Où est géré le son ?
- **Carte son intégrée (onboard)** : puce présente directement sur la carte mère, suffisante pour un usage normal
- **Carte son dédiée (extension PCIe)** : meilleure qualité audio, utile pour le montage audio, le mixage, ou les audiophiles

## Caractéristiques techniques
- **Nombre de canaux** : stéréo (2.0), 5.1, 7.1 (son surround)
- **Qualité audio** : mesurée en bits et kHz (ex : 24 bits/192 kHz pour du haut de gamme)
- **Réduction de bruit** : certaines cartes son filtrent les interférences électriques

---

### Boîtier
--- 

Le boîtier (ou "tour") est la structure externe qui accueille, protège et organise tous les composants internes du PC.

### Formats de boîtier
Le format doit être compatible avec celui de la carte mère.
- **Full Tower** : très grand, pour configurations puissantes avec beaucoup de composants
- **Mid Tower** : format le plus courant, bon compromis taille/espace
- **Mini Tower / Micro-ATX** : plus compact, pour PC standards peu encombrants
- **Mini-ITX** : très petit, pour PC ultra-compacts (salon, portabilité)

## Rôle du boîtier
- **Protection physique** : contre la poussière, les chocs, les projections
- **Support structurel** : maintient tous les composants en place
- **Gestion de la ventilation** : permet à l'air de circuler pour refroidir les composants
- **Organisation des câbles** : espace pour ranger et cacher les câbles (cable management)

## Caractéristiques à prendre en compte
| Caractéristique | Description |
|---|---|
| **Format compatible** | Doit correspondre au format de la carte mère (ATX, mATX, ITX...) |
| **Emplacements de ventilateurs** | Nombre et taille des ventilateurs supportés |
| **Emplacements de stockage** | Nombre de baies pour disques durs/SSD |
| **Longueur max carte graphique** | Certains boîtiers compacts limitent la taille du GPU |
| **Façade** | Ports USB, jack audio, boutons power/reset |
| **Matériau** | Acier, aluminium, verre trempé (souvent pour la vitre latérale) |
| **Filtres à poussière** | Empêchent l'accumulation de poussière à l'intérieur |

## Aujourd'hui
Le choix du boîtier dépend surtout de :
- La taille de la carte mère utilisée
- Le nombre de composants à installer (GPU, disques, ventilateurs)
- Les besoins de refroidissement
- L'esthétique (vitre latérale, RGB, design épuré...)