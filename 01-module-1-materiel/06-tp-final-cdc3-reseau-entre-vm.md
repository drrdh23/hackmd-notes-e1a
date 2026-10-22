### Partie 2 - Cahier des charges 3 — Interconnexion des machines

Contexte : Les deux postes déployés (Windows 10 Pro et Ubuntu) doivent être interconnectés sur le même réseau local afin que le client puisse les administrer et échanger des fichiers entre eux.

Exigences :

- Configurer les deux machines sur le même segment réseau (adressage cohérent : IP fixe ou DHCP sous Vmware Workstation).
- Réaliser un test de connectivité bidirectionnel : ping de Windows vers Ubuntu, et d'Ubuntu vers Windows.
- (Option) Mettre en place un partage de fichiers accessible depuis les deux systèmes (partage SMB/Samba) et vérifier l'accès croisé.
- Documenter la configuration réseau des deux machines (adresse IP, masque, passerelle) sous forme de schéma ou de tableau.
- En cas d'échec de connectivité : diagnostiquer (pare-feu, adressage, câblage) et documenter la résolution appliquée.



*Pc portable hp envy x360*

---

## 1. Contexte et objectifs

 Cette page consiste à vérifier que les postes installés lors du cahier des charge n°2, tous deux en machine virtuelle sous VMware Workstation, communiquent correctement sur le même réseau via un test de ping bidirectionnel.

Objectifs pédagogiques :

- Configurer le bon mode réseau.
- Relever les adresses IP et tester le ping dans les deux sens.
- Diagnostiquer un problème réseau simple.

## 2. Prérequis

- VM Windows 10 installée et fonctionnelle.
- VM Ubuntu installée et fonctionnelle.
- VMware Workstation.

## 3. Relevé des adresses IP

### 3.1 VM Windows 10

Commande utilisée :ipconfig: 

Adresse IPv4 relevée : [192.168.209.134]

![image](https://hackmd.io/_uploads/SkkbXTZKzx.png)
### 4.2 VM Ubuntu

Commande utilisée :

```bash
ip a
```

Adresse IP relevée : [192.168.209.129]


![image](https://hackmd.io/_uploads/rkU5eAWtzx.png)



### 4.3 Vérification de l'appartenance au même sous-réseau

Les deux adresses relevées appartiennent au même sous-réseau (mêmes premiers blocs d'adresse selon le masque, ex. `/24` → 3 premiers blocs identiques), confirmant que les deux VM sont bien positionnées sur le même réseau local.

## 5. Test de ping — Windows vers Ubuntu

Commande exécutée depuis la VM Windows 11 : ping: 

![image](https://hackmd.io/_uploads/HkRZWCbFzg.png)


 Windows-> Ubuntu à bien fonctionner 

## 6. Test de ping — Ubuntu vers Windows

Commande exécutée depuis la VM Ubuntu :

```bash
ping <IP_Windows>
```

Ping ![image](https://hackmd.io/_uploads/HJI1bRZKzg.png)



## 7. Diagnostic (en cas d'échec initial)

Un premier test de ping depuis Ubuntu vers Windows peut échouer par défaut (timeout, "Request timed out"), sans que cela signifie que les deux machines ne sont pas sur le même réseau. La cause la plus fréquente est le **pare-feu Windows Defender**, qui bloque par défaut les requêtes ICMP entrantes (le ping), même lorsque les deux machines sont correctement configurées en réseau.




### 7.1 Vérification et correction appliquée

Pour autoriser le ping entrant sur la VM Windows, la règle de pare-feu correspondante a été activée :

- Via l'interface graphique : Pare-feu Windows Defender avec fonctions avancées de sécurité > Règles de trafic entrant > activation des règles "Fichiers et imprimantes partagés > Clic droit activer la règle

![IMG_9938](https://hackmd.io/_uploads/ryWC0DeKGe.jpg)



![image](https://hackmd.io/_uploads/HJ91yOetfg.jpg)

## Verifier que les VM sont bien sur le même reseau : 

## 8. Configuration du mode réseau

Avant de relever les adresses IP, il est nécessaire de configurer le mode réseau des deux VM afin qu'elles puissent communiquer sur le même sous-réseau virtuel.

### 8.1 Mode réseau sélectionné

Les deux VM (Windows 10 et Ubuntu) ont été configurées en mode **NAT** (Network Address Translation), qui permet à chaque VM de partager l'adresse IP de la machine hôte tout en appartenant au même sous-réseau virtuel VMware.

**Étapes suivies pour chaque VM :**
1. Clic droit sur la VM dans la bibliothèque VMware Workstation > **Settings**.
2. Sélection de **Network Adapter** dans le panneau de gauche.
3. Sélection de l'option **NAT: Used to share the host's IP address**.
4. Validation par **OK**.

![image](https://hackmd.io/_uploads/S1cfdhGFMx.png)


### 8.2 Vérification sur la VM Windows 10

![image](https://hackmd.io/_uploads/rJT4OhzKMx.png)


La capture ci-dessus confirme que l'adaptateur réseau de la VM Windows 10 est bien configuré en mode NAT.

### 8.3 Vérification sur la VM Ubuntu

![image](https://hackmd.io/_uploads/B10vu3GYfe.png)

La capture ci-dessus confirme que l'adaptateur réseau de la VM Ubuntu est bien configuré en mode NAT.



