# Ignavia-Tech

Plateforme CTF orientée **Blue Team** — scénario d'attaque/défense conçu et déployé de A à Z sur infrastructure réelle.

## Objectif

Simuler un environnement d'entreprise réaliste dans lequel un attaquant doit progresser latéralement pour atteindre un objectif final, pendant que la défense surveille, détecte et répond en temps réel.

## Ce que ce projet démontre

- Conception et déploiement d'une infrastructure réseau segmentée en VLANs
- Routage inter-VLAN et règles firewall granulaires (permissives et restrictives) pour guider et contraindre la progression de l'attaquant
- Mapping réseau complet via pfSense avec politique de flux maîtrisée
- Tunnel VPN WireGuard pour l'accès sécurisé à l'infrastructure
- Déploiement et configuration d'un SIEM Wazuh avec scripts de détection automatisés et alerting
- Mise en place d'un honeypot Cowrie intégré à la supervision
- Environnement Active Directory comme cible de mouvement latéral
- Conteneurisation et orchestration de services multi-couches
- Développement d'une interface web et d'un proxy API
- Gestion de la surface d'attaque et OPSEC défensif

## Stack technique

| Catégorie | Technologies |
|---|---|
| Réseau & Firewall | pfSense, VLANs, WireGuard |
| SIEM & Détection | Wazuh, scripts de détection custom |
| Honeypot | Cowrie |
| Environnement cible | Active Directory, Windows |
| Conteneurisation | Docker, Docker Compose |
| Backend | Python |
| Frontend | HTML, CSS, JavaScript |
| Versioning | Git, GitHub |

## Architecture

Infrastructure multi-couches avec segmentation réseau stricte. Les règles de routage inter-VLAN définissent précisément les chemins accessibles — certaines volontairement permissives pour laisser des portes ouvertes, d'autres restrictives pour forcer des choix tactiques. La supervision centralisée via Wazuh couvre l'ensemble des segments avec détection automatisée des comportements suspects.

## Infrastructure

Plateforme déployée sur **4 machines virtuelles** et un **serveur VPS** :

| Rôle | Environnement |
|---|---|
| Firewall & routeur | VM pfSense |
| Poste attaquant | VM Windows 11 |
| Cible | VM Active Directory / Windows Server |
| Services défensifs | VM Linux (Wazuh, Cowrie, Docker) |
| Exposition web | VPS distant (site + proxy API) |

Documentation technique et accès disponibles sur demande.

## Statut

🟢 Plateforme active — détails de connexion transmis aux participants sur demande.

---

*Projet personnel — conception, déploiement et maintenance assurés en autonomie complète.*
