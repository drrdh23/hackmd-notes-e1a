# TP et notes de cours — réseaux, systèmes et virtualisation

Mes travaux pratiques et notes de cours de la formation **E1A** au **Geneva Institute of Technology**, rédigés au fil des modules sur HackMD puis rangés ici par thème.
Les projets les plus aboutis ont leur propre dépôt documenté : voir [Projets documentés](#projets-documentés).

> Les mots de passe de labo sont masqués (`<REDACTED>`, `<MOT_DE_PASSE>`). Les entreprises clientes citées sont des cas d'étude fournis par l'école. Les captures d'écran sont hébergées sur HackMD.

## 01 — Module 1 : matériel et bases
| Note | Contenu |
|---|---|
| [Composants d'un PC](01-module-1-materiel/01-composants-pc.md) | Format, socket, RAM, PCIe, chipset, carte réseau |
| [TP montage / démontage et BIOS](01-module-1-materiel/02-tp-montage-demontage-bios.md) | Démontage et remontage d'un PC, fiche des composants, mot de passe BIOS et Clear CMOS |
| [TP final — questionnaire hardware](01-module-1-materiel/03-tp-final-questionnaire-hardware.md) | Questions de cours sur le matériel |
| [TP final — CDC 1 : profils de PC](01-module-1-materiel/04-tp-final-cdc1-profils-pc.md) | Configurations par profil client, compatibilité, TDP, budget |
| [TP final — CDC 2 : optimisation Windows / Ubuntu](01-module-1-materiel/05-tp-final-cdc2-optimisation-windows-ubuntu.md) | Démarrage, alimentation, pare-feu, nettoyage, point de restauration, installation Ubuntu |
| [TP final — CDC 3 : réseau entre VM](01-module-1-materiel/06-tp-final-cdc3-reseau-entre-vm.md) | Relevé IP, tests ping Windows ↔ Ubuntu, diagnostic, mode réseau |

## 02 — PPE 1 : support poste de travail
| Note | Contenu |
|---|---|
| [01 — Mise en service d'un poste Windows 11](02-ppe1-support-poste-de-travail/01-mise-en-service-poste-windows11.md) | ISO, VM, installation, compte standard, IP fixe |
| [02 — Salle informatique multi-utilisateurs](02-ppe1-support-poste-de-travail/02-salle-info-comptes-multi-utilisateurs.md) | Comptes, cloisonnement des dossiers, politique de sécurité |
| [03 — Migration et sauvegarde d'un poste](02-ppe1-support-poste-de-travail/03-migration-sauvegarde-poste.md) | Inventaire, sauvegarde, contrôle d'intégrité, restauration |
| [04 — Rapport d'intervention : dépannage](02-ppe1-support-poste-de-travail/04-rapport-intervention-depannage.md) | Méthodologie de diagnostic, hypothèses, résolution |
| [05 — Déploiement standardisé par image système](02-ppe1-support-poste-de-travail/05-deploiement-image-systeme.md) | Poste de référence, Sysprep, image, déploiement, comparaison des temps |

## 03 — Réseaux
| Note | Contenu |
|---|---|
| [Serveurs, protocoles et ports](03-reseaux/serveurs-protocoles-ports.md) | Matrice des services réseau et de leurs ports |
| [ICND1 — interconnexion des sites et sécurisation](03-reseaux/icnd1-interconnexion-sites-securisation.md) | Routage multi-sites, SSH vs Telnet analysé dans Wireshark |
| [ICND1 — CDC 1 : dossier d'architecture](03-reseaux/icnd1-cdc1-dossier-architecture.md) | LAN/WAN, flux, OSI vs TCP/IP, plan d'adressage VLSM |

## 04 — Virtualisation : guides
| Note | Contenu |
|---|---|
| [Hyper-V en VM imbriquée](04-virtualisation-guides/guide-hyper-v-imbrique.md) | Windows Server 2022, rôle Hyper-V, vSwitch, VM invitées |
| [ESXi sous VMware Workstation](04-virtualisation-guides/guide-esxi-sous-workstation.md) | Installation ESXi, DCUI, interface web, réseau virtuel |

## 05 — TP final virtualisation
| Note | Contenu |
|---|---|
| [Partie 1 — QCM](05-tp-final-virtualisation/partie-1-qcm.md) | Questions de cours virtualisation et cloud |
| [CDC 1 — dimensionnement de la plateforme](05-tp-final-virtualisation/cdc1-dimensionnement-plateforme.md) | Calculs de ressources et choix de la solution |
| [CDC 2 — POC ESXi + Proxmox](05-tp-final-virtualisation/cdc2-poc-esxi-proxmox.md) | Installation des deux hyperviseurs, VM de test, réseau |
| [CDC 3 — cluster Proxmox haute disponibilité](05-tp-final-virtualisation/cdc3-cluster-ha-proxmox.md) | Cluster 3 nœuds, réseaux dédiés, stockage partagé, HA |

## Projets documentés
| Dépôt | Contenu |
|---|---|
| [network-labs-gns3](https://github.com/drrdh23/network-labs-gns3) | PPE Networking Essentials, projets 1 à 4 |
| [multisite-ipsec-vpn-lab](https://github.com/drrdh23/multisite-ipsec-vpn-lab) | PPE Networking Essentials, projet 5 (VPN IPsec multi-sites) |
| [virtualization-labs](https://github.com/drrdh23/virtualization-labs) | PPE Virtualisation, projets 1 à 5 |
| [ccna-lab-routing-nat-ssh](https://github.com/drrdh23/ccna-lab-routing-nat-ssh) | Lab CCNA : VLSM, routage, NAT, SSH |
