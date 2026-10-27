
# Lab de sécurité réseau : audit et sécurisation d'une infrastructure virtualisée

![Linux](https://img.shields.io/badge/Linux-Niveau%202%20%7C%20Pratique-16A34A)
![Virtualisation KVM](https://img.shields.io/badge/Virtualisation%20KVM-Niveau%202%20%7C%20Pratique-16A34A)
![Réseaux IP](https://img.shields.io/badge/Réseaux%20IP-Niveau%202%20%7C%20Pratique-16A34A)
![Windows Server](https://img.shields.io/badge/Windows%20Server-Niveau%202%20%7C%20Pratique-16A34A)
![OPNsense](https://img.shields.io/badge/OPNsense-Niveau%202%20%7C%20Pratique-16A34A)
![Sécurité réseau](https://img.shields.io/badge/Sécurité%20réseau-Niveau%201%20%7C%20Découverte-2563EB)
![Nmap](https://img.shields.io/badge/Nmap-Niveau%201%20%7C%20Découverte-2563EB)
![Audit réseau](https://img.shields.io/badge/Audit%20réseau-Niveau%201%20%7C%20Découverte-2563EB)

<sub>Échelle : Niveau 1 — Découverte / Apprentissage · Niveau 2 — Pratique · Niveau 3 — Débutant · Niveau 4 — Intermédiaire · Niveau 5 — Expert</sub>

## Un projet dans la continuité du premier homelab

Lors de mon [premier projet de virtualisation KVM](https://github.com/Sauvard-Bryan/Homelab-kvm-serveur-dhcp), j'ai construit une petite infrastructure réseau avec un serveur Windows, un poste client et un routeur OPNsense.

J'ai commencé par faire fonctionner un serveur DHCP sur un réseau isolé, avant de faire évoluer l'architecture vers trois sous-réseaux reliés par OPNsense.

Une fois l'infrastructure fonctionnelle, je me suis demandé si les communications entre les machines étaient correctement contrôlées et quels services étaient réellement accessibles.

C'est le point de départ de ce nouveau projet.

Ce que j'ai déjà découvert :

- Linux, KVM et Virt-Manager.
- La création et la configuration de machines virtuelles.
- Les adresses IP, le DHCP et le routage.
- Windows Server 2022.
- OPNsense comme routeur et relais DHCP.

Ce que je souhaite découvrir :

- Les ports TCP et UDP et les services réseau.
- Les outils d'analyse comme Nmap.
- Le rôle des règles de pare-feu.
- L'identification des risques de configuration.
- Les premières méthodes d'audit et de sécurisation.

Comme pour mon premier homelab, je souhaite avancer progressivement et documenter mes manipulations, les problèmes rencontrés et les résultats obtenus.

## Objectif du projet

Réaliser un premier audit de sécurité sur mon propre réseau virtualisé, puis corriger les éventuels problèmes identifiés.

Le projet se déroulera en trois étapes :

1. **Analyser le réseau** pour identifier les machines, les ports et les services accessibles.
2. **Examiner et sécuriser les communications** entre les sous-réseaux, notamment avec les règles de pare-feu d'OPNsense.
3. **Vérifier les modifications** en reproduisant les tests et en comparant les résultats.

L'objectif est de comprendre comment contrôler les accès à une infrastructure que j'ai déjà appris à faire fonctionner.

## 1. Infrastructure de départ

Je repars de l'architecture réalisée dans mon premier homelab.

### Machines virtuelles

| VM | Système | Rôle |
|---|---|---|
| SERVER-DHCP | Windows Server 2022 | Serveur DHCP et DNS |
| ROUTAGE | OPNsense | Routeur et relais DHCP |
| COMPTA-DEVICE1 | Windows 11 | Poste client utilisé pour les tests |

### Réseaux configurés

| Réseau | Sous-réseau | Passerelle |
|---|---|---|
| Serveur | `192.168.1.0/24` | `192.168.1.254` |
| Comptabilité | `192.168.2.0/24` | `192.168.2.1` |
| Commercial | `192.168.3.0/24` | `192.168.3.1` |

Le serveur Windows conserve son adresse fixe `192.168.1.1`.

OPNsense assure le routage entre les trois réseaux et transmet les demandes DHCP vers le serveur Windows.

Mon PC hôte dispose de 8 Go de RAM. Comme dans le premier projet, je devrai donc tenir compte des ressources disponibles pour réaliser les tests.

L'utilisation d'une machine dédiée à l'audit, éventuellement sous Kali Linux, sera étudiée lors de la préparation du laboratoire.

## 2. Découverte et analyse du réseau

La première étape consistera à vérifier l'infrastructure existante, puis à découvrir comment observer les services accessibles sur les différentes machines.

Je souhaite notamment apprendre à utiliser **Nmap** pour identifier les ports ouverts et les services réseau.

Je chercherai à déterminer :

- Quels services sont accessibles sur le serveur et le poste client.
- Quelles communications sont possibles entre les trois sous-réseaux.
- Si certains accès sont inutiles ou présentent des risques.

Les premiers tests permettront d'établir un état initial du réseau avant toute modification.

## 3. Audit et sécurisation avec OPNsense

Lors du premier projet, j'ai principalement utilisé OPNsense pour le routage et le relais DHCP.

Je souhaite maintenant découvrir son fonctionnement en tant que pare-feu.

L'objectif sera d'examiner les règles existantes, de comprendre les communications qu'elles autorisent et de limiter les accès qui ne sont pas nécessaires.

Les éventuelles corrections seront appliquées progressivement, en vérifiant que les services utiles continuent de fonctionner.

## 4. Tests et résultats

Après les modifications, je reproduirai les tests initiaux pour vérifier les résultats.

| Élément contrôlé | Avant | Après |
|---|---|---|
| Ports et services accessibles | À vérifier | À vérifier |
| Communications entre sous-réseaux | À tester | À tester |
| Règles de pare-feu | À analyser | À vérifier |
| Fonctionnement des services utiles | À vérifier | À vérifier |

Ce tableau sera complété avec les résultats réellement obtenus.

## 5. Documentation du projet

Comme pour mon premier homelab, les manipulations seront documentées dans des chapitres séparés.

| Chapitre | Contenu |
|---|---|
| 01 - Préparation du laboratoire | Vérification de l'infrastructure et préparation des outils |
| 02 - Découverte avec Nmap | Analyse des machines, ports et services |
| 03 - Analyse des communications | Tests entre les différents sous-réseaux |
| 04 - Pare-feu OPNsense | Étude des règles et sécurisation des accès |
| 05 - Contre-audit | Nouveaux tests et comparaison des résultats |
| 06 - Bilan | Difficultés rencontrées et compétences découvertes |

Chaque chapitre présentera les manipulations réalisées, les commandes utilisées, les problèmes rencontrés et les résultats obtenus, avec des captures d'écran lorsque cela sera pertinent.

## 6. État du projet

*Dernière mise à jour : 27/10/2026*

- [x] Définition du sujet et des objectifs
- [x] Choix de réutiliser le premier homelab
- [ ] Vérification de l'infrastructure
- [ ] Préparation des outils d'analyse
- [ ] Premiers tests avec Nmap
- [ ] Identification des ports et services accessibles
- [ ] Analyse des communications entre sous-réseaux
- [ ] Étude des règles de pare-feu OPNsense
- [ ] Correction des éventuels problèmes identifiés
- [ ] Réalisation d'un contre-audit
- [ ] Documentation des résultats sur GitHub
- [ ] Rédaction du bilan final

**Statut : projet en préparation.**

## 7. Bilan

Cette section sera complétée à la fin du projet.

Je souhaite notamment revenir sur les outils découverts, les difficultés rencontrées, les problèmes identifiés et les corrections réalisées.

Ce projet doit me permettre de passer de la création d'une infrastructure réseau à une première compréhension de sa sécurité.

Tous les tests seront réalisés exclusivement sur les machines et les réseaux de mon propre laboratoire, dans un cadre pédagogique.
