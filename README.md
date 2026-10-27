
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

**Statut :** Projet en cours  
**Dernière mise à jour :** 27/10/2026

## Un projet dans la continuité du premier homelab

Lors de mon [premier projet de virtualisation KVM](https://github.com/Sauvard-Bryan/Homelab-kvm-serveur-dhcp), j'ai construit une infrastructure réseau virtualisée avec Windows Server, Windows 11 et OPNsense.

J'ai notamment configuré un serveur DHCP, plusieurs sous-réseaux et le routage entre ces réseaux.

Une fois l'infrastructure fonctionnelle, je me suis posé de nouvelles questions : quels services sont accessibles sur les machines, quelles communications sont autorisées entre les sous-réseaux et comment mieux protéger cette infrastructure ?

Ce deuxième projet est consacré à ces questions.

### Compétences déjà abordées

- Linux, KVM et Virt-Manager.
- Création et configuration de machines virtuelles.
- Adressage IP, DHCP et routage.
- Windows Server 2022.
- OPNsense comme routeur et relais DHCP.

### Nouvelles compétences à découvrir

- Ports TCP et UDP et services réseau.
- Utilisation de Nmap.
- Analyse des communications réseau.
- Règles de pare-feu OPNsense.
- Identification des risques de configuration.
- Méthodes de base d'un audit de sécurité.

Comme pour mon premier homelab, je souhaite comprendre les manipulations réalisées et documenter les résultats, les difficultés rencontrées et les solutions apportées.

## Objectif du projet

Réaliser un premier audit de sécurité de mon infrastructure virtualisée, identifier les éventuels problèmes de configuration et améliorer progressivement la protection du réseau.

L'infrastructure ayant déjà été construite et testée dans mon premier projet, je vais m'appuyer sur cet environnement pour établir un **état initial de sa sécurité**.

Le projet suivra trois grandes étapes :

1. **Audit initial :** identifier les ports, les services accessibles et les communications possibles entre les sous-réseaux.
2. **Analyse et sécurisation :** étudier les résultats, identifier les accès inutiles et adapter les règles de pare-feu.
3. **Contre-audit :** reproduire les tests pour vérifier les modifications et comparer les résultats.

L'objectif est de découvrir les bases de la sécurité réseau à partir d'une infrastructure que j'ai moi-même construite.

## 1. Infrastructure utilisée

Je réutilise l'architecture réalisée dans mon premier homelab.

### Machines virtuelles

| VM | Système | Rôle |
|---|---|---|
| SERVER-DHCP | Windows Server 2022 | Serveur DHCP et DNS |
| ROUTAGE | OPNsense | Routeur et relais DHCP |
| COMPTA-DEVICE1 | Windows 11 | Poste client |

### Réseaux configurés

| Réseau | Sous-réseau | Passerelle |
|---|---|---|
| Serveur | `192.168.1.0/24` | `192.168.1.254` |
| Comptabilité | `192.168.2.0/24` | `192.168.2.1` |
| Commercial | `192.168.3.0/24` | `192.168.3.1` |

Le serveur Windows possède l'adresse fixe `192.168.1.1`.

OPNsense assure le routage entre les trois réseaux et le relais DHCP vers le serveur Windows.

Mon PC hôte dispose de 8 Go de RAM. Cette contrainte sera prise en compte dans le choix des outils et des machines utilisées pour l'audit.

## 2. Audit initial du réseau

La première étape sera consacrée à l'analyse des machines et des services accessibles sur le réseau.

Je souhaite notamment découvrir **Nmap**, un outil permettant d'analyser les ports et d'identifier certains services réseau.

Les tests permettront d'étudier :

- Les ports et services accessibles sur les différentes machines.
- Les communications possibles entre les réseaux Serveur, Comptabilité et Commercial.
- Les accès qui pourraient être inutiles ou présenter un risque.

Les résultats obtenus constitueront un état initial de la sécurité du réseau, avant toute modification.

## 3. Analyse et sécurisation

À partir des résultats de l'audit, je chercherai à comprendre les risques liés aux configurations existantes.

Je souhaite notamment approfondir l'utilisation d'OPNsense en tant que pare-feu, après l'avoir utilisé pour le routage et le relais DHCP dans mon premier projet.

L'objectif sera de limiter les communications non nécessaires, tout en conservant l'accès aux services utiles.

Les corrections seront documentées à partir des problèmes réellement identifiés.

## 4. Contre-audit et résultats

Après les éventuelles modifications, les tests seront reproduits afin de vérifier leur efficacité.

| Élément contrôlé | Avant sécurisation | Après sécurisation |
|---|---|---|
| Ports et services accessibles | À mesurer | À mesurer |
| Communications entre sous-réseaux | À tester | À tester |
| Règles de pare-feu | À analyser | À vérifier |
| Fonctionnement des services utiles | À vérifier | À vérifier |

Ce tableau sera complété avec les résultats réels des tests.

## 5. Documentation du projet

Comme pour mon premier homelab, les manipulations seront documentées dans des chapitres séparés.

| Chapitre | Contenu |
|---|---|
| 01 - Audit initial du réseau | Préparation des outils et découverte des ports et services avec Nmap |
| 02 - Analyse des communications | Tests entre les différents sous-réseaux |
| 03 - Analyse des risques | Étude des résultats et identification des configurations à améliorer |
| 04 - Sécurisation avec OPNsense | Analyse et modification des règles de pare-feu |
| 05 - Contre-audit | Nouveaux tests et comparaison des résultats |
| 06 - Bilan | Difficultés rencontrées et compétences découvertes |

Chaque chapitre présentera les manipulations réalisées, les commandes utilisées, les résultats observés et les problèmes rencontrés.

Les fichiers seront ajoutés progressivement à mesure que le projet avance.

## 6. État du projet

*Dernière mise à jour : 27/10/2026*

- [x] Définition du sujet et des objectifs
- [x] Choix de réutiliser le premier homelab
- [x] Confirmation de la présence des VM et des réseaux virtuels
- [x] Vérification des adresses des interfaces OPNsense
- [ ] Choix et préparation des outils d'audit
- [ ] Premiers tests avec Nmap
- [ ] Identification des ports et services accessibles
- [ ] Analyse des communications entre sous-réseaux
- [ ] Analyse des risques et des règles OPNsense
- [ ] Application des éventuelles corrections
- [ ] Réalisation du contre-audit
- [ ] Documentation des résultats
- [ ] Rédaction du bilan final

**Statut : projet en cours.**

## 7. Bilan

Cette section sera complétée à la fin du projet.

Je souhaite notamment revenir sur les outils découverts, les risques réellement identifiés, les difficultés rencontrées et les corrections apportées.

L'objectif de ce projet est de poursuivre mon apprentissage des systèmes et réseaux en découvrant comment analyser et améliorer la sécurité d'une infrastructure existante.

Les tests seront réalisés uniquement sur les machines et les réseaux de mon propre laboratoire.

## 8. Projet précédent

Ce projet fait suite à :

[Lab de virtualisation KVM : serveur DHCP et poste comptabilité](https://github.com/Sauvard-Bryan/Homelab-kvm-serveur-dhcp)

Le premier projet était consacré à la construction et au fonctionnement du réseau. Celui-ci est consacré à son audit et à sa sécurisation.
