# Ignavia-Tech

Plateforme CTF orientée **Blue Team** — scénario d'attaque/défense conçu et déployé de A à Z sur infrastructure réelle.

## Objectif

Simuler un environnement d'entreprise réaliste dans lequel un attaquant doit progresser latéralement pour atteindre un objectif final, pendant que la défense surveille, détecte et répond en temps réel.

## Démarche pédagogique

### Volet offensif

**Public et besoin.** Le parcours vise des joueurs débutants à intermédiaires : des bases, mais pas encore les réflexes. À ce niveau, le principal risque n'est pas la facilité, c'est le décrochage. D'où un principe directeur : le joueur ne doit jamais se retrouver sans piste à suivre.

**Une fiction au service de l'apprentissage.** Le CTF se déroule chez Ignavia-Tech, une entreprise fictive qui vend des chipsets « d'intelligence augmentée ». Son assistant commercial, NEXUS, est un LLM vaniteux qui tire sa fierté des produits qu'il vend. Discuter avec un vendeur est une situation familière : la barrière d'entrée baisse, et le joueur pratique sans s'en rendre compte.

**L'ingénierie sociale par l'expérience.** NEXUS fait découvrir la manipulation d'un agent conversationnel sans jamais nommer la technique. Orienté vers l'architecture du site, il s'emballe et laisse fuiter une information technique ; flatté sur son intelligence, il livre l'indice suivant. Le joueur apprend ainsi à cadrer une conversation et à exploiter l'ego de sa cible, le ressort le plus humain de l'ingénierie sociale. NEXUS invite lui-même discrètement à la flatterie, et ses réactions montent sur plusieurs échanges pour que la découverte soit méritée.

**Un rythme sans abandon.** Chaque effort produit une récompense ou une relance. Les indices se méritent mais restent accessibles : bloqué, le joueur peut toujours revenir vers NEXUS.

**Des impasses qui enseignent.** Un honeypot et des leurres dans l'Active Directory apprennent à reconnaître une fausse piste et à trier le signal du bruit, sans punir le joueur.

**Une vraie fin.** Le parcours se clôt sur un flag scénarisé : NEXUS, démasqué, prend la fuite.

### Volet défensif

**L'inversion de départ.** Le projet est d'abord né d'un besoin d'entraînement défensif. Sans attaque, un SIEM n'a rien à analyser ; avec une attaque mal pensée, il ne produit que du bruit. Le parcours offensif a donc été conçu comme un générateur d'incidents réalistes, dont chaque étape laisse une trace précise à détecter, interpréter et traiter.

**Corréler des sources hétérogènes.** Wazuh centralise trois familles de journaux : les commandes saisies dans le honeypot Cowrie, l'activité du poste Windows 11 via Sysmon (connexions, processus, accès aux fichiers, élévation de privilèges) et l'Active Directory (création de comptes, modifications de l'annuaire). Le défenseur doit passer de l'une à l'autre pour reconstituer le fil de l'attaque.

**Écrire et tester des règles de détection.** Les événements bruts ne remontaient pas avec une sévérité suffisante : les règles ont été personnalisées, puis validées par des tests, par exemple en créant des comptes de service factices pour vérifier le déclenchement des alertes. Une détection par seuil de transfert simule un contrôle DLP face à l'exfiltration de données.

**Suivre l'attaquant en temps réel.** Une console d'alerte distingue chaque type d'événement (honeypot, connexions RDP, exfiltration) pour situer à tout moment la progression de l'attaquant.

**Répondre à l'intrusion.** Une fois l'attaquant repéré, le défenseur bloque son adresse IP sur pfSense et désactive le compte Active Directory compromis.

**Défense en profondeur.** La segmentation du réseau (LAN, DMZ, administration), l'IDS/IPS Suricata et des canarytokens servant d'alerte précoce font qu'une couche qui tombe est rattrapée par la suivante. Le honeypot devient lui aussi un outil du défenseur, qui peut observer la session de l'attaquant en direct.

**Équilibrer le rapport de force.** Avec ce niveau de supervision, le défenseur aurait pu tout voir et arrêter l'attaquant à volonté, ce qui aurait vidé l'exercice de son intérêt. Des zones d'ombre ont donc été volontairement laissées hors surveillance : l'attaquant n'y est plus à découvert, et le défenseur doit reconstituer le puzzle après coup, comme dans un incident réel.

**Évolutions prévues (v2).** Réponse active automatisée (isolation réseau, dump mémoire, arrêt de processus avec Velociraptor) et wargame en temps réel opposant attaquants et défenseur, avec difficulté modulable en direct et statistiques de fin de partie.

## Ce que ce projet démontre

- Conception d'une expérience d'apprentissage adaptée à son public : scénario, progression, indices et impasses calibrés
- Conception et déploiement d'une infrastructure réseau segmentée en VLANs
- Routage inter-VLAN et règles firewall granulaires (permissives et restrictives) pour guider et contraindre la progression de l'attaquant
- Mapping réseau complet via pfSense avec politique de flux maîtrisée
- Tunnel VPN WireGuard pour l'accès sécurisé à l'infrastructure
- Déploiement et configuration d'un SIEM Wazuh avec scripts de détection automatisés, règles personnalisées et alerting
- IDS/IPS Suricata et canarytokens pour la détection précoce
- Mise en place d'un honeypot Cowrie intégré à la supervision
- Environnement Active Directory comme cible de mouvement latéral
- Conteneurisation et orchestration de services multi-couches
- Développement d'une interface web, d'un agent conversationnel (NEXUS) et d'un proxy API
- Gestion de la surface d'attaque et OPSEC défensif

## Stack technique

| Catégorie           | Technologies                                  |
| ------------------- | --------------------------------------------- |
| Réseau & Firewall   | pfSense, VLANs, WireGuard                     |
| SIEM & Détection    | Wazuh, Sysmon, Suricata, scripts et règles de détection custom |
| Deception           | Cowrie (honeypot), canarytokens               |
| Environnement cible | Active Directory, Windows                     |
| Conteneurisation    | Docker, Docker Compose                        |
| Backend             | Python                                        |
| Frontend            | HTML, CSS, JavaScript                         |
| Versioning          | Git, GitHub                                   |

## Architecture

Infrastructure multi-couches avec segmentation réseau stricte. Les règles de routage inter-VLAN définissent précisément les chemins accessibles — certaines volontairement permissives pour laisser des portes ouvertes, d'autres restrictives pour forcer des choix tactiques. La supervision centralisée via Wazuh couvre l'ensemble des segments avec détection automatisée des comportements suspects.

## Infrastructure

Plateforme déployée sur **4 machines virtuelles** et un **serveur VPS** :

| Rôle               | Environnement                        |
| ------------------ | ------------------------------------ |
| Firewall & routeur | VM pfSense                           |
| Poste attaquant    | VM Windows 11                        |
| Cible              | VM Active Directory / Windows Server |
| Services défensifs | VM Linux (Wazuh, Cowrie, Docker)     |
| Exposition web     | VPS distant (site + proxy API)       |

## Statut

Plateforme déployée et exploitée de mai 2026 à juillet 2026, aujourd'hui hors ligne. Documentation technique et pédagogique disponible sur demande.



---

*Projet personnel — conception, déploiement et maintenance assurés en autonomie complète.*
