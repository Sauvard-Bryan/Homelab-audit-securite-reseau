
# Lab de sécurité réseau : audit et durcissement d'une infrastructure virtualisée

![Linux](https://img.shields.io/badge/Linux-Débutant-green)
![Réseaux](https://img.shields.io/badge/Réseaux-Débutant%20à%20Intermédiaire-blue)
![Sécurité réseau](https://img.shields.io/badge/Sécurité%20réseau-Débutant-green)
![Virtualisation](https://img.shields.io/badge/Virtualisation-Débutant%20à%20Intermédiaire-blue)
![Audit et diagnostic](https://img.shields.io/badge/Audit%20et%20diagnostic-Débutant-green)

**Statut :** Projet en préparation.

Les niveaux indiqués correspondent aux compétences abordées dans le projet. Ils sont indicatifs et ne constituent pas une certification.

## Un deuxième projet pour découvrir la sécurité réseau

Ce projet fait suite à mon premier homelab, dans lequel j'ai découvert Linux, la virtualisation avec KVM, la configuration d'un serveur DHCP Windows et le routage entre plusieurs sous-réseaux avec OPNsense.

Après avoir réussi à faire communiquer les différentes machines de mon laboratoire, je souhaite maintenant comprendre comment vérifier la sécurité de cette infrastructure.

Ce que je connais avant de commencer :

- Linux : j'ai installé Debian et appris à utiliser quelques commandes pour gérer mon environnement.
- Virtualisation : je sais créer et configurer des machines virtuelles avec KVM et Virt-Manager.
- Réseau : j'ai découvert les adresses IP, les sous-réseaux, le DHCP, les passerelles et le routage.
- OPNsense : je l'ai utilisé comme routeur et relais DHCP, mais je n'ai pas encore étudié en détail ses fonctions de pare-feu.
- Sécurité réseau : je pars pratiquement de zéro. Je connais certains termes, mais je n'ai encore jamais réalisé d'audit.

Ce que je souhaite découvrir :

- Le fonctionnement des ports TCP et UDP.
- Les services accessibles sur un réseau.
- Les bases de l'utilisation de Nmap.
- L'analyse des communications entre plusieurs sous-réseaux.
- Le fonctionnement des règles de pare-feu.
- Les risques liés à une mauvaise configuration.
- Les principes de durcissement d'une infrastructure.
- La réalisation d'un rapport d'audit simple.

Comme pour mon premier projet, j'apprendrai progressivement à partir de documentation technique, de tutoriels et de recherches personnelles.

L'objectif n'est pas seulement de reproduire des commandes, mais de comprendre les résultats obtenus et les raisons des modifications effectuées.

## Objectif du projet

Réaliser un premier audit de sécurité sur mon infrastructure virtualisée, identifier les services et communications accessibles, puis améliorer progressivement sa configuration.

Le projet sera organisé autour de plusieurs questions :

1. **Quelles machines et quels services sont accessibles sur mon réseau ?**
2. **Quelles communications sont autorisées entre les sous-réseaux ?**
3. **Certaines configurations présentent-elles des risques ?**
4. **Comment limiter les accès inutiles sans empêcher le réseau de fonctionner ?**
5. **Comment vérifier que les corrections apportées sont efficaces ?**

Je souhaite réaliser un audit initial, appliquer les corrections nécessaires, puis effectuer de nouveaux tests pour comparer les résultats.

Ce projet est une initiation à la sécurité réseau, et non un audit professionnel complet.

## 1. Infrastructure de départ

Je vais réutiliser l'environnement créé lors de mon premier projet.

L'infrastructure comprend trois machines virtuelles hébergées sur Debian avec KVM et Virt-Manager.

### Machines virtuelles existantes

| VM | Système | Rôle |
|---|---|---|
| SERVER-DHCP | Windows Server 2022 | Serveur DHCP et DNS |
| ROUTAGE | OPNsense | Routage et relais DHCP |
| COMPTA-DEVICE1 | Windows 11 | Poste client utilisé pour les tests |

### Réseaux existants

| Réseau | Sous-réseau | Passerelle |
|---|---|---|
| Serveur | `192.168.1.0/24` | `192.168.1.254` |
| Comptabilité | `192.168.2.0/24` | `192.168.2.1` |
| Commercial | `192.168.3.0/24` | `192.168.3.1` |

Le serveur Windows utilise l'adresse fixe `192.168.1.1`.

OPNsense possède trois interfaces réseau et assure le routage entre les sous-réseaux.

Le relais DHCP permet aux postes des réseaux Comptabilité et Commercial d'obtenir une adresse IP depuis le serveur Windows.

### Contraintes matérielles

Mon PC hôte dispose de 8 Go de RAM.

Lors du premier projet, j'avais donc utilisé un seul client Windows 11, déplacé successivement entre les réseaux Comptabilité et Commercial.

Pour ce deuxième projet, je souhaite conserver une infrastructure adaptée aux ressources disponibles.

Une machine Linux dédiée à l'audit pourra être ajoutée si nécessaire. Le choix du système et des outils sera effectué après vérification des ressources et de la configuration actuelle.

## 2. Préparation de l'audit

Avant d'effectuer des tests, je dois vérifier l'état de mon infrastructure et définir précisément ce que je souhaite analyser.

### Manipulations prévues

- [ ] Vérifier le fonctionnement des VM existantes.
- [ ] Identifier les réseaux virtuels configurés dans KVM.
- [ ] Vérifier les adresses IP des machines.
- [ ] Vérifier les communications réseau existantes.
- [ ] Déterminer depuis quelle machine réaliser les tests.
- [ ] Définir le périmètre de l'audit.
- [ ] Documenter l'architecture de départ.

L'objectif de cette étape est de disposer d'un état initial fiable avant de commencer les analyses.

## 3. Découverte des machines et des services

Je souhaite commencer par comprendre comment identifier les équipements et les services accessibles sur un réseau.

Pour cela, je prévois de découvrir **Nmap**, un outil permettant notamment d'analyser les ports et certains services réseau.

### Questions à étudier

- Qu'est-ce qu'un port réseau ?
- Quelle est la différence entre TCP et UDP ?
- Que signifie un port ouvert, fermé ou filtré ?
- Comment identifier les machines accessibles ?
- Comment reconnaître les services qui utilisent certains ports ?

### Manipulations prévues

- [ ] Installer ou rendre disponible Nmap dans le laboratoire.
- [ ] Découvrir les commandes de base.
- [ ] Identifier les machines accessibles sur un sous-réseau.
- [ ] Relever les ports ouverts sur les machines autorisées.
- [ ] Identifier les services détectés.
- [ ] Documenter et expliquer les résultats.

Les tests seront réalisés uniquement sur mes propres machines virtuelles.

Un port ouvert ne signifie pas automatiquement qu'un système est vulnérable. Je chercherai à comprendre le rôle du service et la nécessité de son accessibilité.

## 4. Analyse des communications entre sous-réseaux

Mon premier projet m'a permis de mettre en place le routage entre les réseaux Serveur, Comptabilité et Commercial.

Je souhaite maintenant vérifier quelles communications sont réellement possibles entre ces réseaux.

### Questions à étudier

- Un poste du réseau Comptabilité peut-il accéder au réseau Commercial ?
- Quels services du serveur Windows sont accessibles depuis les réseaux clients ?
- Les communications observées sont-elles nécessaires ?
- Le routage permet-il actuellement davantage d'accès que prévu ?

### Manipulations prévues

- [ ] Tester les communications depuis le réseau Comptabilité.
- [ ] Tester les communications depuis le réseau Commercial.
- [ ] Identifier les services accessibles sur le réseau Serveur.
- [ ] Comparer les résultats selon le sous-réseau utilisé.
- [ ] Documenter les communications autorisées et bloquées.

Les résultats serviront de base à l'analyse des règles de pare-feu.

## 5. Découverte du pare-feu OPNsense

J'ai déjà utilisé OPNsense pour le routage et le relais DHCP, mais je n'ai pas encore étudié précisément son fonctionnement en tant que pare-feu.

Je souhaite comprendre comment ses règles peuvent contrôler les communications entre mes différents réseaux.

### Notions à découvrir

- Le rôle d'un pare-feu.
- Les règles d'autorisation et de blocage.
- Les interfaces réseau.
- Les adresses sources et destinations.
- Les ports et protocoles.
- Le principe du moindre privilège.

### Manipulations prévues

- [ ] Examiner les règles existantes dans OPNsense.
- [ ] Comprendre leur fonctionnement.
- [ ] Identifier les communications réellement nécessaires.
- [ ] Préparer des règles de filtrage adaptées.
- [ ] Tester les règles progressivement.
- [ ] Vérifier que le DHCP et les autres services nécessaires continuent de fonctionner.

L'objectif est de comprendre comment mieux isoler les sous-réseaux tout en conservant les fonctionnalités utiles de l'infrastructure.

## 6. Identification des risques et durcissement

Après les premiers tests, je souhaite analyser les résultats pour identifier d'éventuelles configurations à améliorer.

Je ne connais pas encore les problèmes que je vais rencontrer.

Cette partie sera donc complétée à partir des observations réelles, sans supposer à l'avance que certaines machines sont vulnérables.

### Manipulations prévues

- [ ] Examiner les services accessibles.
- [ ] Identifier les accès éventuellement inutiles.
- [ ] Vérifier les configurations concernées.
- [ ] Décrire les risques réellement constatés.
- [ ] Définir des corrections adaptées.
- [ ] Appliquer les modifications progressivement.
- [ ] Vérifier le fonctionnement des services après correction.

Chaque modification sera accompagnée d'une explication : problème identifié, correction appliquée et résultat obtenu.

## 7. Contre-audit et comparaison

Une fois les modifications réalisées, je souhaite reproduire les tests initiaux pour vérifier leur efficacité.

### Manipulations prévues

- [ ] Relancer les tests réseau.
- [ ] Comparer les ports et services accessibles.
- [ ] Vérifier les communications entre sous-réseaux.
- [ ] Contrôler les règles de filtrage.
- [ ] Vérifier le fonctionnement des services nécessaires.
- [ ] Rédiger un bilan des améliorations observées.

### Tableau comparatif

Ce tableau sera complété à partir des résultats réels.

| Élément contrôlé | Avant correction | Après correction |
|---|---|---|
| Services accessibles | À mesurer | À mesurer |
| Communications entre réseaux | À tester | À tester |
| Règles de pare-feu | À analyser | À vérifier |
| Fonctionnement DHCP | À vérifier | À vérifier |
| Risques identifiés | À documenter | À réévaluer |

## 8. Documentation du projet

Comme pour mon premier homelab, je souhaite documenter les manipulations dans des chapitres séparés.

### Organisation prévue

| Chapitre | Contenu |
|---|---|
| 01 - Préparation de l'audit | Vérification de l'infrastructure et définition du périmètre |
| 02 - Découverte avec Nmap | Identification des machines, ports et services |
| 03 - Analyse des communications | Tests entre les différents sous-réseaux |
| 04 - Pare-feu OPNsense | Analyse et configuration des règles de filtrage |
| 05 - Durcissement | Identification des risques et corrections |
| 06 - Contre-audit | Comparaison des résultats avant et après |
| 07 - Bilan | Résultats, difficultés et compétences découvertes |

Les fichiers seront créés progressivement.

Chaque chapitre présentera :

- Les objectifs.
- Les notions découvertes.
- Les manipulations réalisées.
- Les commandes utilisées et leur rôle.
- Les résultats observés.
- Les problèmes rencontrés.
- Les corrections apportées.
- Les captures d'écran utiles.

## 9. État du projet

*Dernière mise à jour : 27/10/2026*

- [x] Définition du sujet du deuxième projet
- [x] Identification des objectifs pédagogiques
- [x] Choix de réutiliser le premier homelab
- [ ] Vérification de l'infrastructure existante
- [ ] Définition du périmètre d'audit
- [ ] Préparation des outils
- [ ] Premiers tests avec Nmap
- [ ] Analyse des communications réseau
- [ ] Audit des règles OPNsense
- [ ] Identification des risques
- [ ] Application des corrections
- [ ] Contre-audit
- [ ] Rédaction du rapport final
- [ ] Documentation complète sur GitHub

**Statut : projet en préparation.**

## 10. Bilan

Cette section sera complétée à la fin du projet.

Je souhaite notamment pouvoir expliquer :

- Ce que j'ai appris sur les ports et services réseau.
- Comment j'ai utilisé les outils d'analyse.
- Ce que j'ai découvert sur les règles de pare-feu.
- Les difficultés rencontrées pendant les tests.
- Les risques réellement identifiés.
- Les corrections effectuées.
- Les limites de mon audit.

L'objectif est de comprendre progressivement comment analyser et améliorer la sécurité d'une infrastructure, plutôt que de simplement appliquer des commandes sans en comprendre les résultats.

## 11. Projet précédent

Ce deuxième projet s'appuie sur l'infrastructure créée dans mon premier homelab :

[Lab de virtualisation KVM : serveur DHCP et poste comptabilité](https://github.com/Sauvard-Bryan/Homelab-kvm-serveur-dhcp)

Le premier projet était consacré à la construction et au fonctionnement du réseau.

Celui-ci sera consacré à son analyse et à sa sécurisation.
