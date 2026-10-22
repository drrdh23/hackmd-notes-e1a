# TP FINAl Partie 1 
## QCM 
1. c'est l'entreprise IBM qui a inventé le premier concept de la VM sur des ordinateurs appelés mainframes
2. Les serveurs sont devenus bons marchés et donc les entreprises préferent avoir un bureau pour chaque machine 
3. Le produit Vmware ESX est sorti 
4. La virtualisation est une simulation de plusieurs systèmes physiques que l'on peut faire tourner sur une seule machine (pc).
5. L'hôte c'est l'hyperviseur  celui qui va exécuter la virtualisation alors que le guest c'est la VM 
6. Template c'est une vm qu'on prépare configure et que l'on peut cloner rapidement.
7. Le snapshot ne peut pas car lui il sert a restaurer le systeme (VM) en revenant en arrière alors que pour faire une sauvagarde il faut cloner toutes les données vers un espace externe 
8. 
- Car c'est beaucoup plus rapide de créer une vm que de refaire tout une machine 
- MOins chers notamment sur le point matériel et éléctricité.
- si une vm ne fonctionne plus elle affecte pas les autres et on peut la refaire 
- nous pouvons ouvrir plusieurs vm sur une seule et même machine virtuelle 
- ON peut faire de la maintenance sans couper les services grace aux snapshot et à la migration à chaud 

9. L'hyperviseur va consommer des couches de de virtualisation et c'est ce qui va faire perde de la performance en % 
10. Car etant donner que c'est le serveur qui hébérgent toutes les VMs et bien si il tombent elles vont toutes tomber en même temps et on ne pourra le remplacer par un autre h'ote il faut donc mettre en place des clusters) 
11. Intel = VTx AMD= AMD-V
12. Leurs rôles est de permettrent au CPU de gérer la traduction d'adresses mémoire virtuelles en physiques.
13. egrep -c '(vmx|svm)' /proc/cpuinfo // INtel=vmx et AMD= svm
14. Gestionnaire des tâches > performances > Virtualisation > Activé 
15. Dans le Bios/ Uefi de la machine.
16. C'est quand plusieurs VMs sollicitent le même disque en même temps. Elles se retrouvent en concurrence pour lire et écrire, ce qui provoque des ralentissements pour tout
17. Plusieurs VMs accèdent au même disque en même temps, donc la vitesse d'accès est cruciale. Les NVMe sont beaucoup plus rapides et ont une latence bien plus faible que les HDD mécaniques, ce qui limite la contention d'E/S.
18. Il permet à une seule carte réseau physique d'être partagée directement entre plusieurs VMs, avec des performances presque similaires a une carte réseau physique
19. C'est faire tourner un hyperviseur à l'intérieur d'une VM. Dans un lab, elle permet de tester plusieurs hyperviseurs (Proxmox, ESXi, Hyper-V) ou de simuler un cluster sur un seul PC, sans avoir besoin de plusieurs serveurs physiques. Il faut pour ça exposer les extensions VT-x/AMD-V à la VM (l'option « Virtualize Intel VT-x/EPT or AMD-V/RVI » dans VMware Workstation, par exemple).
20. Parce qu'un seul serveur physique héberge le trafic de dizaines de VMs à la fois. C'est pour ça que la bande passante est passée de 1 Gbps à 10, 25 ou 100 Gbps.
21. UN VDI est une façon de pouvoir gérer son poste de travail bureau qui est sur un serveur à distance depuis un terminal léger faisable sur une tablette /  Windows 365 
22. Ce sont des fonctions réseaux recréer en logciel plutÔt qu'en matériel physique/ VMware NSX
23. La virtualisation de stockage c'est le regroupements de plusieurs périphériques de stockage physiques présentés comme un seul espace de stockage logique cela peut servir a simplifier la gestion de gros volumes de données on a : CEPH ou VMware vSan
24. La virtualisation applicative permet de faire cohabiter deux versions de logiciel tels que java, et donc de déployer des applications sur des postes sans pour autant les installer  car chacun est dans sa bulle avec ses propres composants 
25. Une machine virtuelle reproduit un ordinateur complet. Elle fonctionne grâce à un hyperviseur et possède son propre système d'exploitation, avec son propre noyau. On peut donc faire tourner des OS différents sur un même serveur comme windows ou linux et chaque VM est très bien isolée des autres. En contrepartie, elle est lourde : elle occupe plusieurs Go et met une à plusieurs minutes à démarrer. Un conteneur, lui, n'a pas de système d'exploitation propre. Il partage le noyau de la machine hôte, par l'intermédiaire d'un moteur comme Docker, et n'embarque que l'application et ses dépendances. Il est donc beaucoup plus léger, démarre en quelques secondes, et on peut en faire tourner bien plus sur un même serveur. En contrepartie, l'isolation est plus faible, et tous les conteneurs doivent utiliser le même type d'OS que l'hôte. En résumé, on choisit une VM quand on a besoin d'un OS différent ou d'une isolation forte, et un conteneur pour déployer rapidement de nombreuses applications légères.
26. Un conteneur démarre plus vite parce qu'il n'a pas de système d'exploitation à démarrer il utilise le noyau de l'hôte, déjà en marche, et lance simplement un processus isolé.
27. Avec la VDI, les bureaux Windows des 150 employés sont hébergés sur des serveurs, dans le datacenter de l'entreprise ou dans le cloud. Chaque employé se connecte à son bureau à distance, depuis n'importe quel appareil (PC, Mac, tablette, client léger), et retrouve toujours le même environnement, avec ses fichiers et ses applications. De plus l'appareil de l'utilisateur ne sert qu'à afficher le bureau, tout s'exécute sur le serveur, les données restent dans le datacenter. Si un portable est perdu ou volé, aucune donnée n'est dessus et les logiciels et les sauvegardes sont gérés une seule fois, sur les serveurs, et pas sur 150 postes.
28. Dans proxmox les LXC  sont des conteneurs 
29. Broadcom a racheté VMware fin de l'année 2023.
30. La conséquence principale, c'est la fin des licences perpétuelles au profit d'abonnements, depuis le 11 décembre 2023. Les produits ont été regroupés en packs, donc les entreprises qui n'avaient besoin que d'un ou deux produits devaient prendre des bundles beaucoup plus chers. Des membres du CISPE parlent de hausses de 800 % à 1 500 %
31. Proxmox VE repose sur KVM (et LXC pour les conteneurs), sur une base Debian.
32. C'est Hyper-V. Il est intégré à Windows Server, à Windows 10/11 Pro et Entreprise, et à Azure Local (ex-Azure Stack HCI).
33. Je citerais Infomaniak et Exoscale en Suisse, ou OVHcloud en France.
34. Kubernetes orchestre les conteneurs : déploiement, mise à l'échelle, redémarrage automatique et répartition sur plusieurs nœuds.
35. XCP-ng repose sur Xen (c'est un fork de Citrix XenServer).
36. Les x86-64 sont des architectures dominante dans les serveurs et PC c'est à dire que quasi toutes les plateformes de virtualisation d'entreprise tournent autour d'elle, Alors que l'ARM est une architecture dominante sur mobiles qui progresse dans les srrveurs et  cloud sutout dans les puces apple.
37. Car leurs jeux d'instructions sont incompatibles un os x86 ne pourra pas s'executer sur un ARM.
38. UN coeur est une unité de calcul physique , un thread est le résultat du coeur c'est à dire qu'un coeur traite deux flux avec plus ou moins +20 à 30% et un vcpu signifie Virtual CPU donc c'est un processeur virtuel qu'on ajoute a une VM par l'hyperviseur.
39. Le threading apporte un gain de performances de environ 20 à 30%.
40. l'Overcommit c'est le fait d'ajouter plusieurs Vcpu a une Vm que sa capacité de threads en esperents que quelles sont pas toutes à 100% 
41. Pour des charges générales on peut faire du 3:1 ou 4:1 mais pour du critiques il faut faire soit 1:1 ou 2:1.
42. on a 2x32= 64 threads et 64x4 =256vCPU avec 256/4=64VMs maximum 
43. 60x2=120vCPU et 120/3=40 threads au minimum 
44. Si les vms demandent plus de ram que ce qui est disponible alors l'hôte doit swaper sur disques sinon la vm risque de soit s'effondrer soit planter.
45. Le TDP c'est la puissance thermique aue le processeur dissipe cela va directement impacter la facture d'élécricité et de refroidissement.
46. Pour 40 petites VMs je peux privilégié le nombre de coeurs et pour une appli mono thread je privilégie la fréquence. 
47. Le N+1 c'est prévoir un noeud de plus que nécéssaire comme ca on ameliore la sécurité donc si un noued tombe les autres vms sont absorber par un autre noeud
48. Un hyperviseur est celui qui va crée et faire tourner les VMs en partageant les ressources physiques entre elles.
49. Le type 1 sert a s'installer directement sur le matériel alors que le type 2 lui s'installe sur l'OS existant comme une application.
50. VMware ESXi proxmos VE et Microsoft HyperV
51. VmWAre Workstation, Virtual Box ET Parallels Destop.
52. Si il y moins de couches il y a de meilleures performences, stabilité et une surface d'attaque réduite.
53. KVM pour les VMs et LXC pour les conteneurs
54. Hyper V car il est inclus dans Windows serveur et s'intègre avec Active directory ystem Center et Azure et en plus de ça les équipes et entreprises le connaissent déjà.
55. Je choisirai Proxmox VE car il est gratuit open Source et contient des clusters HA ainsi que des sauvegardes avec un support payant qui est opptionnel.
56. L'interface web Proxmox VE est accessible par le port :8006
57. son nom par default est vmbr0
58. Il relie les VMs au réseau physique via la carte reseau de l'hôte comme un switch virtuel.
59. Pour réaliser une sauvegarde il faut taper la commande vzdump
60. pvecm create <nom_du_cluster>
61. pvecm add <IP_d'un_nœud_du_cluster>
62. pvecm status
63. Le quorum désigne la majorité de nœuds nécessaire pour que le cluster prenne des décisions. Avec 3 nœuds, si un tombe, les 2 restants ont encore la majorité. Avec 2, on ne peut pas départager.
64. Corosync
65. Ceph est un stockage distribué intégré à Proxmox il utilise les disques de tous les noeuds pour fournir un stockage partagé et répliqué
66. https://IP_de_l'hôte/ui
67. vCenter centralise la gestion de plusieurs hôtes ESXi. Il permet aussi le cluster, vMotion, HA et DRS.
68. Le vSwitch est le commutateur virtuel relié aux cartes physiques. Le Port Group est un groupe de ports sur ce vSwitch avec une config commune (VLAN, sécurité) sur lequel on branche les VMs.
69. Le port VMkernel sert au trafic de l'hôte lui-même : management, vMotion, stockage iSCSI/NFS, vSAN.
70. VMFS est le système de fichiers en cluster de VMware. Il permet à plusieurs hôtes d'accéder au même datastore.
71. Un disque Thin prend de la place au fur et à mesure qu'on écrit dedans. Un disque Thick réserve tout l'espace dès la création.
72. VMware utilise VMDK, Hyper-V utilise VHDX (ou VHD), et RHV utilise QCOW2 (ou RAW).
73. Le commutateur externe relie les VMs au réseau physique. Le commutateur interne permet la communication entre les VMs et l'hôte. Le commutateur privé ne permet la communication qu'entre VMs.
74. La Génération 1 utilise un BIOS classique avec émulation IDE. La Génération 2 utilise l'UEFI avec Secure Boot, démarre en SCSI ou sur le réseau, et ne prend en charge que les OS 64 bits récents.
75. Le checkpoint standard sauvegarde aussi l'état de la mémoire. Le checkpoint de production utilise VSS dans la VM pour un état cohérent au niveau des applications, sans la RAM. C'est celui qu'on utilise en prod.
76. Chez VMware, c'est vMotion. Chez Hyper-V, c'est Live Migration.
77. C'est le Failover Clustering (cluster de basculement Windows).
78. Un VLAN découpe un réseau physique en plusieurs réseaux logiques isolés. En virtualisation, ça permet de séparer les flux (VMs, management, stockage) sur les mêmes câbles.
79. IEEE 802.1Q.
80. L'agrégation de liens combine plusieurs cartes réseau pour avoir plus de débit et de la redondance si un lien tombe.
81. Pour que l'accès à l'administration des hôtes ne soit pas exposé aux VMs ou aux utilisateurs, ce qui limite les attaques.
82. Pour éviter qu'un flux gourmand (stockage, migration) sature les autres. Un heartbeat de cluster perdu peut déclencher un faux basculement. On y gagne aussi en sécurité.
83. Il faut que les VMs soient dans le même sous-réseau (ou qu'un routage existe), que le VLAN soit bien propagé entre les deux hôtes, et que les pare-feux autorisent l'ICMP.
84. Le pare-feu Windows bloque l'ICMP entrant par défaut. Pour le corriger proprement, j'active la règle « Partage de fichiers et d'imprimantes (Demande d'écho – ICMPv4 entrant) » au lieu de désactiver tout le pare-feu.
85. Il faut exposer la virtualisation matérielle à la VM : « Expose hardware assisted virtualization » dans VMware, type CPU host dans Proxmox, ou ExposeVirtualizationExtensions dans Hyper-V. Côté réseau, il faut souvent aussi activer le mode promiscuous et les forged transmits.
86. Un SAN fournit du stockage en mode bloc sur un réseau dédié, et le serveur le voit comme un disque local. Un NAS partage des fichiers via le réseau.
87. En bloc : iSCSI (ou Fibre Channel). En fichier : NFS (ou SMB).
88. Pendant la migration, seules la RAM et l'état de la VM bougent. Les disques doivent donc être accessibles aux deux hôtes en même temps
89. La migration à chaud est planifiée et ne cause aucune coupure. Le basculement HA arrive après une panne, la VM redémarre sur un autre hôte, donc il y a une courte interruption.
90. Le heartbeat est un signal envoyé régulièrement entre les nœuds pour vérifier qu'ils sont toujours en vie.
91. On garde 3 copies des données, sur 2 supports différents, dont 1 hors site.
92. Le snapshot protège contre une mauvaise manip ou une mise à jour ratée. La HA protège contre la panne d'un hôte. La sauvegarde protège contre la perte de données (panne de stockage, ransomware, sinistre).
93. Le fichier delta grossit en continu, ce qui remplit le stockage, dégrade les performances et rend la fusion longue et risquée.
94. Le split-brain, c'est quand le cluster se coupe en deux et que chaque moitié se croit seule et lance les mêmes VMs, ce qui corrompt les données. Avec le quorum, seul le groupe majoritaire a le droit d'agir.
95. Sans stockage partagé, les disques des VMs sont sur le nœud en panne et l'autre ne peut pas les redémarrer. Et avec 2 nœuds, il n'y a pas de quorum en cas de coupure.
96. Une VM Escape, c'est quand un attaquant sort de sa VM pour accéder à l'hyperviseur, donc potentiellement à toutes les autres VMs. 
97. Je citerais : appliquer les mises à jour rapidement, désactiver les services inutiles (SSH, etc.) et isoler le réseau de management avec des accès restreints.
98. Parce que quelqu'un qui a accès à la console contrôle toute l'infrastructure. Le MFA empêche qu'un simple mot de passe volé suffise.
99. Le cloud hybride combine une infra on-premise et un cloud public. Le multi-cloud utilise plusieurs fournisseurs cloud en même temps.
100. Le vendor lock-in, c'est être prisonnier d'un fournisseur parce qu'en sortir coûte trop cher ou est trop compliqué. Le rachat de VMware en est un bon exemple. Avec des formats et des outils compatibles avec plusieurs plateformes, on peut migrer ses VMs ailleurs si les conditions changent.
