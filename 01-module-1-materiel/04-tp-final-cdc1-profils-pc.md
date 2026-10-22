### Partie 2 - Cahiers des charges 1 (mise en situation professionnelle)
---
Vous intervenez en tant que Technicien IT pour répondre à trois mandats clients. Chaque cahier des charges précise le contexte, les exigences techniques, les contraintes et les livrables attendus.
### Cachiers des charges 1 - Conception de configurations hardware.
---
**Consignes** Votre entreprise reçoit trois demandes de devis pour des configurations PC différentes. Pour chaque profil, vous devez proposer une configuration complète, cohérente et compatible, dans le respect du budget imposé par le client.

**Consignes communes aux 3 profils :**

- Lister chaque composant (catégorie, marque/modèle si possible, prix unitaire en CHF).
- Vérifier et justifier la compatibilité de l'ensemble (socket CPU/carte mère, type de RAM, format boîtier/alimentation, TDP du CPU vs puissance de l'alimentation).
- Respecter le budget indiqué (tolérance de ± 10 %).
- Justifier chaque choix de composant clé par rapport à l'usage du client (2 à 3 phrases).
- Calculer et indiquer le prix total de la configuration

# Profil 1 — PC Gamer CHF 1'500.–

## Fiche de configuration

| Composant | Référence exacte | Prix CHF | Lien digitec |
|---|---|---|---|
| CPU | AMD Ryzen 5 7500F (boîte, AM5, 3.70 GHz, 6 cœurs) | 199.– | [lien](https://www.digitec.ch/fr/s1/product/amd-ryzen-5-7500f-am5-370-ghz-6-core-processeur-37801983) |
| Carte mère | Gigabyte B650M AORUS Elite (AM5, B650, mATX) | 136.– | [lien](https://www.digitec.ch/fr/s1/product/gigabyte-b650m-aorus-elite-am5-amd-b650-matx-carte-mere-57281378) |
| RAM | Corsair Vengeance DDR5-6000, 32 Go (2x16 Go) | 102.– | [lien](https://www.digitec.ch/fr/s1/product/corsair-vengeance-ddr5-6000mhz-32gb-2x16gb-2-x-16gb-3000-mhz-ddr-ram-dimm-memoire-vive-23541122) |
| SSD | Crucial P3, 1 To, M.2 2280 | 62.– | [lien](https://www.digitec.ch/fr/s1/product/crucial-p3-1000-go-m2-2280-ssd-21575420) |
| GPU | PNY GeForce RTX 5060 Ti Overclocked Dual Fan, 16 Go GDDR7 | 678.– | [lien](https://www.digitec.ch/fr/s1/product/pny-geforce-rtx-5060-ti-overclocked-dual-fan-16-go-gddr7-carte-graphique-57627998) |
| Boîtier | Corsair 3000D (ATX, noir) | 55.90 | [lien](https://www.digitec.ch/fr/s1/product/corsair-3000d-atx-boitier-pc-37097465) |
| Alimentation | Corsair CX650, 650 W, 80+ Bronze | 52.– | [lien](https://www.digitec.ch/fr/s1/product/corsair-cx650-650-w-alimentation-pc-38771599) |
| **Total** | | **1'284.90** | |
## Tableau de vérification de compatibilité

| Vérification | Détail |
|---|---|
| Socket CPU ↔ carte mère | AM5 ↔ AM5 |
| Refroidisseur ↔ socket | Ventirad stock compatible AM5  (TDP 65 W dans sa capacité) |
| Type RAM ↔ carte mère | DDR5 ↔ DDR5, jusqu'à 6000+ MHz en EXPO  |
| Stockage ↔ carte mère | SSD M.2 NVMe PCIe 4.0 ↔ slot M.2 PCIe 4.0 de la carte mère  |
| GPU ↔ carte mère | PCIe x16 ↔ slot PCIe x16 |
| Carte mère ↔ boîtier | Format mATX ↔ boîtier ATX Mid Tower (supporte ATX/mATX/ITX) |
| GPU ↔ boîtier | Longueur RTX 4060 Ti (~267 mm) ↔ 360 mm max supportés par le boîtier  |
| Ventirad ↔ boîtier | Wraith Stealth (~54 mm) ↔ 170 mm max supportés par le boîtier  |
| Connecteurs alimentation | 24 broches (carte mère), 8 broches EPS (CPU), 1x 8 broches PCIe (GPU) ↔ fournis par le PSU 550 W  |
| Certification alimentation | Exigence 80+ Bronze minimum ↔ PSU 80+ Gold (dépasse l'exigence) |

## Calcul du TDP total et marge de l'alimentation

| Composant | Consommation estimée |
|---|---|
| CPU | 65 W |
| Carte mère + RAM + ventilateurs boîtier | ~40 W |
| SSD NVMe | ~8 W |
| GPU (RTX 4060 Ti) | ~165 W |
| **TDP total estimé** | **~278 W** |

**Alimentation choisie :** 550 W → marge d'environ **272 W**, soit une charge d'environ 50 % en usage réel — un point de fonctionnement idéal pour le rendement, la longévité du matériel et le silence, tout en absorbant les pics de consommation transitoires du GPU.

## Vérification budget et contraintes client

| Repère | Valeur |
|---|---|
| Budget client | CHF 1'500.– |
| Tolérance ±10% autorisée | CHF 1'350.– à 1'650.– |
| Marge de sécurité voulue | Cible ≤ CHF 1'300.– |
| **Total de la configuration** | **CHF 1'212.90** |
| **Marge restante** | **CHF 287.10** |

| Contrainte demandée | Respectée ? |
|---|---|
| Particulier, jeux AAA récents en 1440p, 60+ FPS |  RTX 4060 Ti 16 Go |
| Carte graphique dédiée obligatoire |  GPU dédié, pas d'iGPU |
| Refroidissement efficace |  CPU 65 W + ventirad stock suffisant + boîtier bon airflow |
| Boîtier avec bon airflow | Corsair 3000D, façade mesh, 2 ventilateurs inclus |
| Alimentation 80 PLUS Bronze minimum |  80 PLUS Gold (dépasse le minimum) |

## Argumentaire — pourquoi ces choix

**CPU — AMD Ryzen 5 7500F :** pour un usage gaming pur, le GPU reste le composant déterminant pour les performances, pas le CPU. Un 6 cœurs/12 threads à 65 W de TDP est largement suffisant pour ne pas brider la RTX 4060 Ti dans la quasi-totalité des jeux actuels, ce qui permet de réorienter le budget vers le GPU plutôt que vers un CPU plus cher et peu utile ici.

**Refroidisseur CPU — Ventirad stock (Wraith Stealth) :** le TDP de 65 W du 7500F est modéré, le ventirad fourni de série suffit largement à le refroidir sous charge gaming, sans nécessité d'investir davantage sur ce poste.

**Carte mère — Gigabyte B650M AORUS Elite :** compatible avec le socket AM5 du CPU et la DDR5, chipset B650 suffisant pour ce niveau de composants (pas besoin du X670 plus cher, réservé à l'overclocking poussé ou au multi-GPU).

**Mémoire — 32 Go DDR5-6000 :** capacité confortable pour du gaming moderne avec applications en arrière-plan (Discord, navigateur, overlay de streaming), et la fréquence 6000 MHz correspond au point optimal de performance pour les CPU AMD AM5 (sweet spot reconnu).

**Stockage — SSD NVMe 1 To :** les jeux AAA récents pèsent de plus en plus lourd (certains dépassent 100-150 Go), donc 1 To offre une capacité confortable pour une bibliothèque de jeux, avec des temps de chargement rapides grâce au NVMe PCIe 4.0.

**Carte graphique — RTX 4060 Ti 16 Go :** c'est le composant central de cette configuration gaming. Elle offre un excellent niveau de performance en 1440p avec les paramètres graphiques élevés grâce à ses 16 Go de VRAM, tout en restant dans le budget imposé — une RTX 4060 8 Go classique serait plus limitée en VRAM à cette résolution.

**Boîtier — Corsair 3000D :** format imposé par la carte mère (mATX, compatible boîtier ATX), avec une façade mesh privilégiant le flux d'air pour dissiper la chaleur générée par le GPU sous charge prolongée en jeu — conforme à la contrainte d'airflow imposée par le client.

**Alimentation — Antec EAG Pro 550 W 80+ Gold :** dimensionnée avec une marge large (~272 W) par rapport à la consommation réelle estimée (~278 W), pour absorber les pics de consommation transitoires typiques des GPU modernes sous charge, tout en respectant et dépassant l'exigence de certification 80 PLUS Bronze minimum du client.

## Profil 2 — PC Administration (bureautique)
---
# Profil 2 — PC Administration (bureautique)

## 1. Fiche de configuration

| Composant | Référence / Format | Prix CHF | Lien digitec |
|---|---|---|---|
| CPU | AMD Ryzen 5 5600G — AM4, 6 cœurs/12 threads, iGPU Radeon Vega 7 intégré, ventirad Wraith Stealth inclus | 154.– | [lien](https://www.digitec.ch/fr/s1/product/amd-ryzen-5-5600g-am4-390-ghz-6-core-processeur-16242475) |
| Carte mère | AsRock A520M-HVS — AM4, chipset A520, format mATX | 44.90 | [lien](https://www.digitec.ch/fr/s1/product/asrock-a520m-hvs-am4-amd-a520-matx-carte-mere-13645756) |
| Mémoire (RAM) | Corsair Vengeance LPX — DDR4-3200, 16 Go (2x8 Go), format DIMM | 153.– | [lien](https://www.digitec.ch/fr/s1/product/corsair-vengeance-lpx-2-x-8gb-3200-mhz-ram-ddr4-dimm-memoire-vive-12454869) |
| Stockage | Integral Memory Série M2 — SSD NVMe PCIe, 500 Go, format M.2 2280 | 96.90 | [lien](https://www.digitec.ch/fr/s1/product/integral-memory-serie-m2-500-go-m2-2280-ssd-24129147) |
| Boîtier | Intertech H-606 — format mATX mini-tour, insonorisé | 37.90 | [lien](https://www.digitec.ch/fr/s1/product/intertech-h-606-matx-boitier-pc-13405927) |
| Alimentation | Thermalright TR-TB450S — 450 W, 80+ Bronze, format ATX | 42.90 | [lien](https://www.digitec.ch/fr/s1/product/thermalright-tr-tb450s-black-psu-80-bronze-450w-450-w-alimentation-pc-47876097) |
| **Total** | | **529.60** | |

**Budget client : CHF 600.–** → **Marge restante : CHF 70.40** 

## 2. Tableau de vérification de compatibilité

| Vérification | Détail  |
|---|---|
| Socket CPU ↔ carte mère | AM4 ↔ AM4|
| Type RAM ↔ carte mère | DDR4 ↔ DDR4, jusqu'à 3200 MHz supportés nativement |
| Stockage ↔ carte mère | SSD M.2 NVMe ↔ slot M.2 de la carte mère  |
| Carte mère ↔ boîtier | Format mATX ↔ boîtier mATX mini-tour  |
| Refroidisseur ↔ boîtier | Wraith Stealth (~55 mm) ↔ 150 mm max supportés |
| Connecteurs alimentation | 24 broches + 4+4 broches CPU ↔ fournis par le PSU 450 W |

## 3. Vérification des contraintes de la cliente

| Contrainte de la cliente | Comment |
|---|---|
| Pas de GPU dédié nécessaire  | iGPU Radeon Vega 7 intégré au Ryzen 5 5600G, suffisant pour bureautique/ERP web |
| Fiabilité et silence prioritaires | Boîtier insonorisé (H-606) + PSU largement surdimensionné (charge réelle ~23%, ventilateur tourne très bas) |
| Format compact accepté | Boîtier mATX mini-tour, carte mère mATX |
| Budget CHF 600.– | Total CHF 529.60, marge de CHF 70.40 |

## 4. Pourquoi ces choix

| Composant | Justification |
|---|---|
| **CPU — Ryzen 5 5600G** | Son iGPU Radeon Vega 7 élimine le besoin d'une carte graphique dédiée (contrainte client), tout en réduisant coût et consommation. 6 cœurs/12 threads largement suffisants pour multi-fenêtrage Office + ERP web sans ralentissement. |
| **Carte mère — AsRock A520M-HVS** | Chipset A520 fiable et économique, pas d'overclocking nécessaire pour du bureautique. Format mATX répond à la contrainte de compacité. |
| **RAM — 16 Go DDR4-3200** | Capacité confortable pour faire tourner Office + navigateur multi-onglets + ERP web simultanément sans ralentissement. |
| **SSD — 500 Go NVMe** | Le SSD (sans pièce mécanique) est un choix clé pour la fiabilité recherchée par la cliente, contrairement à un disque dur classique. 500 Go largement suffisant pour un usage bureautique. |
| **Boîtier — Intertech H-606** | Conçu spécifiquement pour les systèmes silencieux (panneaux insonorisés), répond directement à la contrainte de silence. Format mATX compact. |
| **Alimentation — Thermalright TR-TB450S 450 W** | Très large marge par rapport à la consommation réelle (~103 W, soit ~23% de charge) → le ventilateur tourne à très bas régime en permanence, ce qui maximise silence et fiabilité, sans surpayer pour une puissance inutile. |

### Profil 3 — PC Étudiant (informatique)
---
# Profil 3 — PC Étudiant (informatique)

## 1. Fiche de configuration

| Composant | Référence / Format | Prix CHF | Lien digitec |
|---|---|---|---|
| CPU | AMD Ryzen 5 8600G — AM5, 6 cœurs/12 threads, iGPU Radeon 760M, virtualisation (AMD-V) supportée | 143.– | [lien](https://www.digitec.ch/fr/s1/product/amd-ryzen-5-8600g-am5-430-ghz-6-core-processeur-42390586) |
| Carte mère | Gigabyte B650M AORUS Elite — AM5, chipset B650, mATX, **4 slots DDR5** (256 Go max) | 121.– | [lien](https://www.digitec.ch/fr/s1/product/gigabyte-b650m-aorus-elite-am5-amd-b650-matx-carte-mere-57281378) |
| RAM | Corsair Vengeance DDR5-5200, 16 Go (2x8 Go), format DIMM | 257.– | [lien](https://www.digitec.ch/en/s1/product/corsair-vengeance-2-x-8gb-5200-mhz-ddr5-ram-dimm-ram-25016132) |
| Stockage | Integral Memory Série M2 — SSD NVMe PCIe, 500 Go, format M.2 2280 | 96.90 | [lien](https://www.digitec.ch/fr/s1/product/integral-memory-serie-m2-500-go-m2-2280-ssd-24129147) |
| Boîtier | Corsair 3000D — ATX moyen tour, façade mesh | 55.90 | [lien](https://www.digitec.ch/fr/s1/product/corsair-3000d-atx-boitier-pc-37097465) |
| Alimentation | Corsair CX650 — 650 W, 80+ Bronze | 52.– | [lien](https://www.digitec.ch/fr/s1/product/corsair-cx650-650-w-alimentation-pc-38771599) |
| **Total** | | **725.80** | |

**Budget client : CHF 900.–** → **Marge restante : CHF 174.20**

## 2. Tableau de vérification de compatibilité

| Vérification | Détail  |
|---|---|
| Socket CPU ↔ carte mère | AM5 ↔ AM5 |
| Type RAM ↔ carte mère | DDR5 ↔ DDR5, 4 slots (2 utilisés / 2 libres pour extension)  |
| RAM minimum imposée | Exigence 16 Go minimum ↔ 16 Go installés, extensible jusqu'à 256 Go  |
| Stockage ↔ carte mère | SSD NVMe M.2 ↔ 2 slots M.2 sur la carte mère|
| Carte mère ↔ boîtier | Format mATX ↔ boîtier ATX (compatible mATX) |
| Alimentation | 650 W ↔ largement suffisant pour CPU 65W + futur GPU dédié éventuel  |

## 3. Vérification des contraintes du client

| Contrainte | Comment |
|---|---|
| Virtualisation (plusieurs VM) | Ryzen 5 8600G : 6 cœurs/12 threads, support AMD-V/SVM natif |
| Développement  | Multi-cœurs + SSD NVMe = compilation et IDE réactifs |
| Bureautique | Largement couvert par le CPU et 16 Go de RAM |
| Jeu occasionnel | iGPU Radeon 760M : jeux légers/eSport en 1080p sans carte dédiée |
| RAM minimum 16 Go, extensible | 16 Go installés, 2 slots DDR5 libres sur 4 pour évoluer |
| SSD NVMe recommandé | SSD M.2 NVMe 500 Go |
| CPU multi-cœurs adapté virtualisation  | 6 cœurs/12 threads, technologie de virtualisation AMD |
| Budget CHF 900.–  | Total CHF 725.80, marge de CHF 174.20 |

## 4. Pourquoi ces choix

| Composant | Justification |
|---|---|
| **CPU — Ryzen 5 8600G** | 6 cœurs/12 threads suffisants pour plusieurs VM simultanées + développement, et l'iGPU Radeon 760M couvre le "jeu occasionnel" sans le coût d'une carte graphique dédiée — un choix économique idéal pour un étudiant. |
| **Carte mère — Gigabyte B650M AORUS Elite** | 4 slots DDR5 (dont 2 libres) permettent une extension future de la RAM sans changer de carte mère, répondant directement à la contrainte "extensible" du client. |
| **RAM — 16 Go DDR5** | Respecte exactement le minimum imposé ; suffisant pour 2-3 VM légères + IDE + navigateur, extensible plus tard via les slots libres. |
| **SSD — 500 Go NVMe** | Répond à la recommandation SSD NVMe du client ; les VM et la compilation de code bénéficient directement de sa vitesse par rapport à un disque dur. |
| **Boîtier — Corsair 3000D** | Bon airflow pour dissiper la chaleur en cas d'usage intensif prolongé (VM + compilation), format ATX standard économique. |
| **Alimentation — Corsair CX650 650W** | Marge large pour couvrir une éventuelle carte graphique dédiée ajoutée plus tard par l'étudiant, cohérent avec l'esprit "extensible" du cahier des charges. |