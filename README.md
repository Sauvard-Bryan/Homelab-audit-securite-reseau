
# Lab de sécurité réseau : audit et sécurisation d'une infrastructure virtualisée

## Compétences techniques

![Linux](https://img.shields.io/badge/Linux-Niveau%202%20%7C%20Pratique-16A34A)
![Virtualisation KVM](https://img.shields.io/badge/Virtualisation%20KVM-Niveau%202%20%7C%20Pratique-16A34A)
![Réseaux IP](https://img.shields.io/badge/Réseaux%20IP-Niveau%202%20%7C%20Pratique-16A34A)
![Windows Server](https://img.shields.io/badge/Windows%20Server-Niveau%202%20%7C%20Pratique-16A34A)
![OPNsense](https://img.shields.io/badge/OPNsense-Niveau%202%20%7C%20Pratique-16A34A)
![Sécurité réseau](https://img.shields.io/badge/Sécurité%20réseau-Niveau%201%20%7C%20Découverte-2563EB)
![Nmap](https://img.shields.io/badge/Nmap-Niveau%201%20%7C%20Découverte-2563EB)
![Audit réseau](https://img.shields.io/badge/Audit%20réseau-Niveau%201%20%7C%20Découverte-2563EB)

<sub>Échelle : Niveau 1 — Découverte / Apprentissage · Niveau 2 — Pratique · Niveau 3 — Débutant · Niveau 4 — Intermédiaire · Niveau 5 — Expert</sub>

## Un deuxième projet de découverte

Après avoir créé un premier homelab autour de KVM, Windows Server et OPNsense, je souhaite maintenant découvrir les bases de la sécurité réseau.

Mon premier projet m'a permis de comprendre le fonctionnement d'un réseau avec plusieurs sous-réseaux, un serveur DHCP et un routeur.

Mais je ne me suis pas encore intéressé à sa sécurité.

Je souhaite donc réutiliser cette infrastructure pour découvrir comment analyser les services accessibles, contrôler les communications entre les réseaux et améliorer les configurations existantes.

Ce que je connais déjà :

- Linux et la virtualisation avec KVM et Virt-Manager.
- La configuration d'un serveur DHCP sous Windows Server.
- Les adresses IP, les sous-réseaux et le routage.
- L'utilisation d'OPNsense comme routeur et relais DHCP.

Ce que je découvre avec ce projet :

- Les ports TCP et UDP et les services réseau.
- L'utilisation de Nmap pour analyser un réseau.
- Les règles de pare-feu dans OPNsense.
- L'identification des risques de configuration.
- Les premières méthodes d'audit et de sécurisation.

Comme pour mon premier projet, j'apprends progressivement en m'appuyant sur des ressources en ligne et en documentant mes manipulations.

## Objectif du projet

Réaliser un premier audit de sécurité sur mon homelab, puis appliquer des corrections pour mieux protéger les machines et limiter les communications inutiles.

Le projet se déroulera en trois grandes étapes :

1. **Observer l'infrastructure** : identifier les machines, les ports et les services accessibles.
2. **Analyser et sécuriser** : examiner les communications entre sous-réseaux et les règles du pare-feu OPNsense.
3. **Vérifier les résultats** : reproduire les tests pour comparer la situation avant et après les modifications.

L'objectif est de comprendre les bases d'un audit réseau à travers un environnement que j'ai moi-même construit.

## 1. Infrastructure de départ

Je réutilise l'infrastructure créée lors de mon premier projet.

[Consulter le premier homelab : virtualisation KVM et serveur DHCP](https://github.com/Sauvard-Bryan/Homelab-kvm-serveur-dhcp)

### Machines virtuelles

| VM | Système | Rôle |
|---|---|---|
| SERVER-DHCP | Windows Server 2022 | Serveur DHCP et DNS |
| ROUTAGE | OPNsense | Routeur et relais DHCP |
| COMPTA-DEVICE1 | Windows 11 | Poste client utilisé pour les tests |

### Réseaux existants

| Réseau | Sous-réseau | Passerelle |
|---|---|---|
| Serveur | `192.168.1.0/24` | `192.168.1.254` |
| Comptabilité | `192.168.2.0/24` | `192.168.2.1` |
| Commercial | `192.168.3.0/24` | `192.168.3.1` |

Le serveur Windows possède l'adresse fixe `192.168.1.1`.

OPNsense assure le routage entre les trois réseaux et transmet les demandes DHCP au serveur Windows.

Mon PC hôte disposant de 8 Go de RAM, je vais chercher à réutiliser les machines existantes plutôt qu'à multiplier les VM.

## 2. Découverte et audit du réseau

La première étape consistera à vérifier l'infrastructure, puis à découvrir les outils permettant d'observer son fonctionnement.

Je souhaite notamment utiliser **Nmap** pour identifier les ports et les services accessibles sur les différentes machines.

Je chercherai ensuite à comprendre :

- Quels services sont accessibles sur le serveur Windows et le poste client ?
- Quelles communications sont possibles entre les réseaux Comptabilité, Commercial et Serveur ?
- Les accès autorisés sont-ils tous nécessaires ?

Les résultats de ces tests constitueront l'état initial de mon audit.

## 3. Sécurisation avec OPNsense

Lors de mon premier projet, j'ai utilisé OPNsense pour faire communiquer plusieurs sous-réseaux.

Cette fois, je souhaite découvrir son rôle de pare-feu.

L'objectif sera d'examiner les règles existantes et de comprendre comment autoriser uniquement les communications nécessaires.

Les modifications seront effectuées progressivement, en vérifiant que les services utiles, notamment le DHCP, continuent de fonctionner.

Les éventuels problèmes identifiés et les corrections réalisées seront documentés.

## 4. Tests et résultats

Après les modifications, je reproduirai les tests réalisés au début du projet.

L'objectif sera de comparer les services accessibles et les communications possibles avant et après la sécurisation.

| Élément | Avant | Après |
|---|---|---|
| Ports et services accessibles | À vérifier | À vérifier |
| Communications entre sous-réseaux | À tester | À tester |
| Règles de pare-feu | À analyser | À vérifier |
| Fonctionnement des services utiles | À vérifier | À vérifier |

Ce tableau sera complété avec les résultats réellement obtenus.

## 5. Documentation du projet

Comme pour mon premier homelab, chaque étape sera documentée dans un fichier séparé, avec les commandes utilisées, les captures d'écran, les problèmes rencontrés et les résultats.

| Chapitre | Contenu |
|---|---|
| 01 - Préparation du laboratoire | Vérification de l'infrastructure et du périmètre de test |
| 02 - Découverte avec Nmap | Identification des machines, ports et services |
| 03 - Analyse réseau | Tests de communication entre les sous-réseaux |
| 04 - Pare-feu OPNsense | Analyse des règles et sécurisation |
| 05 - Contre-audit | Nouveaux tests et comparaison des résultats |
| 06 - Bilan | Résultats, difficultés et apprentissages |

Les chapitres seront ajoutés progressivement à mesure que le projet avance.

## 6. État du projet

*Dernière mise à jour : 27/10/2026*

- [x] Définition du sujet et des objectifs
- [x] Choix de réutiliser le premier homelab
- [ ] Vérification de l'infrastructure
- [ ] Découverte et utilisation de Nmap
- [ ] Identification des ports et services accessibles
- [ ] Analyse des communications entre sous-réseaux
- [ ] Analyse des règles OPNsense
- [ ] Sécurisation des accès réseau
- [ ] Réalisation des tests de vérification
- [ ] Documentation des résultats sur GitHub
- [ ] Rédaction du bilan final

**Statut : projet en préparation.**

## 7. Bilan

Cette section sera complétée à la fin du projet.

Je souhaite notamment revenir sur les outils découverts, les difficultés rencontrées, les éventuels problèmes de sécurité identifiés et les solutions appliquées.

L'objectif de ce deuxième homelab est de poursuivre mon apprentissage des systèmes et réseaux en découvrant une nouvelle dimension : **la sécurité d'une infrastructure informatique**.

Tous les tests seront réalisés exclusivement sur les machines de mon propre laboratoire, dans un cadre pédagogique.
