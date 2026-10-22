# Rapport d'intervention — Dépannage poste de travail
*Projet 4 — Dépannage et diagnostic d'un poste en panne*

## Informations générales

| Champ | Valeur |
|---|---|
| Cours | IT Essentials — Mettre en service un poste de travail ICT |
| Classe | E1A |
| Technicien | [Ton nom] |
| Poste concerné | VM Windows 11 x64 (VMware Workstation) — nom d'hôte : boxbox |
| Date de l'intervention | 17.09.2026 |

## Étape 0 — Simulation de la panne (préparation du poste de test)

Avant l'intervention, la panne a été volontairement provoquée sur le poste de test (VM Windows 11) en désactivant l'adaptateur réseau physique via le Gestionnaire de périphériques, afin de reproduire un scénario réaliste de panne matérielle/pilote.

1. **Ouvrir le Gestionnaire de périphériques** : `Windows + X` → *Gestionnaire de périphériques*.
2. **Développer** la catégorie *Cartes réseau*.
3. **Sélectionner la carte physique** : Intel(R) 82574L Gigabit Network Connection (à distinguer des adaptateurs virtuels WAN Miniport).
4. **Désactiver le périphérique** : clic droit → *Désactiver l'appareil* → confirmer.
5. **Confirmer la panne** : l'icône réseau de la barre des tâches affiche un symbole barré (déconnecté), le navigateur affiche `ERR_INTERNET_DISCONNECTED`.

![Capture d'écran 2026-09-17 094816](https://hackmd.io/_uploads/S1fb0zYKfx.png)

Le poste est désormais dans un état de panne reproductible, prêt pour la phase de diagnostic ci-dessous.

## 1. Description de la panne signalée

Symptôme rapporté par l'utilisateur : le poste ne parvient plus à accéder au réseau local ni à Internet. Aucune page web ne se charge dans le navigateur, alors que le poste était fonctionnel la veille.

Contexte : aucune modification matérielle récente signalée. L'utilisateur n'a pas connaissance d'un changement de configuration.
![Capture d'écran 2026-09-17 094524](https://hackmd.io/_uploads/SyoGCGKKGx.png)
![Capture d'écran 2026-09-17 094444](https://hackmd.io/_uploads/Sy9f0MtKfg.png)


## 2. Méthodologie de diagnostic

La démarche suivie s'appuie sur une méthode structurée en 4 phases, conforme aux bonnes pratiques IT Essentials :

- **Identifier le problème** : recueillir les symptômes et le contexte auprès de l'utilisateur.
- **Établir une théorie de la cause probable** : lister plusieurs hypothèses plausibles, de la plus simple à la plus complexe.
- **Tester la théorie** pour déterminer la cause : vérifier chaque hypothèse avec l'outil adapté.
- **Établir un plan d'action, résoudre le problème et vérifier la solution** : appliquer le correctif puis valider le retour à la normale.

## 3. Hypothèses testées

| # | Hypothèse testée | Outil / méthode utilisé | Résultat | Conclusion |
|---|---|---|---|---|
| 1 | Panne générale du réseau (pas propre au poste) | Test sur navigateur + comparaison avec un autre poste | Erreur `ERR_INTERNET_DISCONNECTED` uniquement sur ce poste | Écartée (panne locale) |
| 2 | Problème de configuration IP (DHCP/adressage) | `ipconfig /all` | Aucune carte Ethernet listée dans la sortie — pas d'IP attribuée du tout | Écartée (pas un problème d'adressage, plus grave) |
| 3 | Adaptateur réseau désactivé au niveau système | Gestionnaire de périphériques | Carte "Intel(R) 82574L Gigabit Network Connection" affichée avec une flèche vers le bas (désactivée) | **Confirmée — cause racine** |
| 4 | Panne du pilote réseau (corruption) | Observateur d'événements (journal Système) | Événement 27, source `e1i68x64`, niveau Avertissement : "Network link is disconnected" à 09:44:26 — cohérent avec une désactivation volontaire, pas une erreur pilote | Écartée |

## 4. Étapes du diagnostic (avec captures d'écran)

### 4.1 Constat initial

Ouverture d'un navigateur (Microsoft Edge) : erreur `ERR_INTERNET_DISCONNECTED` affichée — "Il semble que vous n'êtes pas connecté(e) à Internet."

![Capture d'écran 2026-09-17 094524](https://hackmd.io/_uploads/r1srAMYYGl.png)

### 4.2 État de la connexion réseau

L'icône réseau dans la barre des tâches affiche un symbole barré, signe d'une absence totale de connexion (différent d'un simple "Pas d'accès Internet").

![Capture d'écran 2026-09-17 094444](https://hackmd.io/_uploads/SyVdAGtYzl.png)


### 4.3 Analyse avec ipconfig

La commande `ipconfig /all` ne fait apparaître aucune carte Ethernet dans la configuration IP — seules les informations générales de l'hôte (boxbox) sont affichées, sans section "Carte Ethernet". Cela indique que l'adaptateur est totalement inactif au niveau système, pas seulement débranché.

![Capture d'écran 2026-09-17 094744](https://hackmd.io/_uploads/rJJYAfFYMe.png)


### 4.4 Consultation du Gestionnaire de périphériques

La carte **Intel(R) 82574L Gigabit Network Connection** apparaît avec une flèche vers le bas dans la liste des Cartes réseau, confirmant qu'elle est désactivée. C'est la cause racine identifiée.

![Capture d'écran 2026-09-17 094816](https://hackmd.io/_uploads/S1rcAfYYzx.png)


### 4.5 Consultation de l'Observateur d'événements

Dans le journal **Système**, un événement d'avertissement (ID 27, source `e1i68x64`) confirme : *"Network link is disconnected"*, enregistré à 09:44:26 le 17.09.2026 — horaire cohérent avec le moment de la désactivation manuelle de la carte.
![image](https://hackmd.io/_uploads/HyemlXFYGl.png)


## 5. Résolution du problème

Correctif appliqué :

1. Ouverture du Gestionnaire de périphériques (`Windows + X`)
2. Développement de la catégorie *Cartes réseau*
3. Clic droit sur **Intel(R) 82574L Gigabit Network Connection**
4. Sélection de **Activer l'appareil**

![Capture d'écran 2026-09-17 095557](https://hackmd.io/_uploads/HycVxmKFfe.png)


## 6. Validation de la solution

Après réactivation de la carte, `ipconfig /all` confirme le retour à la normale :

- **Carte** : Intel(R) 82574L Gigabit Network Connection — DHCP activé : Oui
- **Adresse IPv4** : 192.168.209.140
- **Masque de sous-réseau** : 255.255.255.0
- **Passerelle par défaut** : 192.168.209.254
- **Serveur DHCP** : 192.168.209.2
- **Bail obtenu** : jeudi 17 septembre 2026, 09:55:45

Test complémentaire : navigation web fonctionnelle, pages chargées normalement.

![Capture d'écran 2026-09-17 095755](https://hackmd.io/_uploads/Hk9HxmtFfx.png)
![image](https://hackmd.io/_uploads/rJ5FxQFKfx.png)
## 7. Conclusion et recommandations

Cause racine : l'adaptateur réseau physique (Intel(R) 82574L Gigabit Network Connection) avait été désactivé au niveau du système d'exploitation via le Gestionnaire de périphériques, coupant toute communication réseau du poste — y compris l'obtention d'une adresse IP par DHCP.

Recommandation : lorsqu'un poste ne présente **aucune** carte réseau dans `ipconfig /all` (plutôt qu'une IP invalide ou une adresse APIPA), vérifier en priorité l'état d'activation de l'adaptateur dans le Gestionnaire de périphériques avant d'envisager une panne de câble ou de service DHCP — cela permet de gagner du temps sur le diagnostic.

Temps total d'intervention : 28min Statut du ticket : **Résolu**

