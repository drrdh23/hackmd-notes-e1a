# Matrice des Serveurs, Protocoles et Ports Réseau


## Web & Internet

| Serveur | Protocole | Port(s) | Transport |
|---|---|---|---|
| Web HTTP | HTTP | 80 | TCP |
| Web HTTPS | HTTPS | 443 | TCP |
| Proxy | HTTP/HTTPS | 3128, 8080 | TCP |
| Reverse Proxy / Load Balancer | HTTP/HTTPS | 80, 443 | TCP |
| Serveur d'application | HTTP/AJP/RMI | 8080, 8009, 1099 | TCP |
| CDN / Cache | HTTP/HTTPS | 80, 443 | TCP |

##  Fichiers & Stockage

| Serveur | Protocole | Port(s) | Transport |
|---|---|---|---|
| Partage Windows | SMB/CIFS | 445 (139 legacy) | TCP |
| Partage Linux/Unix | NFS | 2049 | TCP/UDP |
| FTP | FTP | 21 (ctrl) / 20 (data) | TCP |
| FTPS | FTPS | 990 | TCP |
| SFTP | SSH | 22 | TCP |
| SAN | iSCSI | 3260 | TCP |
| SAN Fibre Channel | FCP | N/A (fibre optique dédiée) | — |
| Sauvegarde (Veeam, NetBackup...) | Propriétaire | 10000-10001, variable | TCP |
| Nextcloud / ownCloud | HTTPS | 443 | TCP |

##  Annuaire & Authentification

| Serveur | Protocole | Port(s) | Transport |
|---|---|---|---|
| Active Directory / LDAP | LDAP | 389 | TCP/UDP |
| Active Directory sécurisé | LDAPS | 636 | TCP |
| AD Global Catalog | LDAP GC | 3268 / 3269 (SSL) | TCP |
| Kerberos | Kerberos | 88 | TCP/UDP |
| Azure AD / Entra ID | SAML/OAuth/HTTPS | 443 | TCP |
| RADIUS | RADIUS | 1812 (auth) / 1813 (acct) | UDP |
| TACACS+ | TACACS+ | 49 | TCP |
| RSA SecurID | Propriétaire | 5500-5580 | TCP/UDP |
| MFA Cloud (Duo, Okta...) | HTTPS | 443 | TCP |

##  Bases de données

| Serveur | Protocole | Port(s) | Transport |
|---|---|---|---|
| MySQL / MariaDB | MySQL | 3306 | TCP |
| PostgreSQL | PostgreSQL | 5432 | TCP |
| SQL Server | TDS | 1433 (+1434 UDP browser) | TCP/UDP |
| Oracle | TNS | 1521 | TCP |
| MongoDB | Mongo Wire Protocol | 27017 | TCP |
| Redis | Redis | 6379 | TCP |
| Elasticsearch | HTTP/REST | 9200, 9300 | TCP |
| Cassandra | CQL | 9042 | TCP |

##  Messagerie

| Serveur | Protocole | Port(s) | Transport |
|---|---|---|---|
| SMTP (envoi) | SMTP | 25, 587 (soumission), 465 (SMTPS) | TCP |
| POP3 | POP3 | 110 / 995 (SSL) | TCP |
| IMAP | IMAP | 143 / 993 (SSL) | TCP |
| Exchange (MAPI/RPC) | MAPI over HTTP | 443 | TCP |
| Webmail | HTTPS | 443 | TCP |

##  Téléphonie / Communication

| Serveur | Protocole | Port(s) | Transport |
|---|---|---|---|
| IPBX (signalisation) | SIP | 5060 (5061 TLS) | UDP/TCP |
| IPBX (voix) | RTP | 10000-20000 | UDP |
| H.323 (visio) | H.323 | 1720 | TCP |
| Teams / Skype Entreprise | HTTPS/STUN/TURN | 443, 3478-3481 | TCP/UDP |

## Administration & Accès distant

| Serveur | Protocole | Port(s) | Transport |
|---|---|---|---|
| RDP | RDP | 3389 | TCP (+UDP optionnel) |
| RDS / RD Gateway | RDP / HTTPS | 3389 / 443 | TCP |
| SSH | SSH | 22 | TCP |
| Telnet (déconseillé) | Telnet | 23 | TCP |
| VNC | RFB | 5900+ | TCP |
| WinRM | WS-Management | 5985 (HTTP) / 5986 (HTTPS) | TCP |
| SNMP | SNMP | 161 (requêtes) / 162 (traps) | UDP |

##  Infrastructure réseau

| Serveur | Protocole | Port(s) | Transport |
|---|---|---|---|
| DNS | DNS | 53 | UDP (TCP si >512 octets ou zone transfer) |
| DHCP | DHCP | 67 (serveur) / 68 (client) | UDP |
| NTP (synchro horaire) | NTP | 123 | UDP |
| Syslog | Syslog | 514 | UDP (601/6514 TCP/TLS) |
| TFTP | TFTP | 69 | UDP |
| PXE Boot | TFTP/DHCP | 67/68/69 | UDP |

##  Sécurité

| Serveur | Protocole | Port(s) | Transport |
|---|---|---|---|
| Antivirus (cloud/mise à jour) | HTTPS | 443 | TCP |
| Antivirus (console de gestion) | Propriétaire | 8443, 4118, 13000 (variable) | TCP |
| Firewall (management) | HTTPS/SSH | 443, 22 | TCP |
| VPN IPSec | IKE / ESP | 500 (UDP), 4500 (NAT-T) | UDP |
| VPN SSL | HTTPS/OpenVPN | 443, 1194 | TCP/UDP |
| SIEM (collecte de logs) | Syslog/HTTPS | 514, 443 | UDP/TCP |
| PKI / Certificats (CRL/OCSP) | HTTP/HTTPS | 80, 443 | TCP |

##  Impression & Divers

| Serveur | Protocole | Port(s) | Transport |
|---|---|---|---|
| Impression réseau | IPP / LPD | 631 (IPP) / 515 (LPD) | TCP |
| Print Server Windows | SMB | 445 | TCP |
| Supervision (Nagios, Zabbix, PRTG) | HTTP/SNMP | 443, 161 | TCP/UDP |
| Réplication AD | RPC dynamique | 135 + plage dynamique | TCP |

---

##  Résumé — Les 16 ports/services les plus importants à retenir

| # | Serveur | Protocole | Port(s) | Transport |
|---|---|---|---|---|
| 1 | Web sécurisé | HTTPS | 443 | TCP |
| 2 | Web standard | HTTP | 80 | TCP |
| 3 | DNS | DNS | 53 | UDP/TCP |
| 4 | DHCP | DHCP | 67/68 | UDP |
| 5 | SSH / SFTP | SSH | 22 | TCP |
| 6 | RDP | RDP | 3389 | TCP |
| 7 | FTP | FTP | 21/20 | TCP |
| 8 | Active Directory / LDAP | LDAP / LDAPS | 389 / 636 | TCP/UDP |
| 9 | Kerberos | Kerberos | 88 | TCP/UDP |
| 10 | SMTP (mail) | SMTP | 25 / 587 | TCP |
| 11 | Partage fichiers Windows | SMB/CIFS | 445 | TCP |
| 12 | Partage fichiers Unix | NFS | 2049 | TCP/UDP |
| 13 | Base de données | MySQL/PostgreSQL/SQL Server/Oracle | 3306/5432/1433/1521 | TCP |
| 14 | RADIUS (2FA/RSA) | RADIUS | 1812/1813 | UDP |
| 15 | VoIP / IPBX | SIP / RTP | 5060 / 10000-20000 | UDP |
| 16 | SNMP (supervision) | SNMP | 161/162 | UDP |

** Astuce mémo** :
- **UDP** = services "légers" et sans connexion → DNS (requêtes), DHCP, RADIUS, SNMP, NTP, RTP (voix)
- **TCP** = tout ce qui nécessite fiabilité → Web, bases de données, fichiers, RDP, SSH, mail
- **443** revient partout (HTTPS, Azure AD, proxy, VPN SSL, antivirus...) → à retenir en priorité absolue