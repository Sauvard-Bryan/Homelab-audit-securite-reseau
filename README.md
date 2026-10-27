
# Lab de sécurité réseau : audit et durcissement d'une infrastructure virtualisée

## Compétences techniques

![Linux](https://img.shields.io/badge/Linux-Niveau%202%20%7C%20Pratique-16A34A)
![Virtualisation KVM](https://img.shields.io/badge/Virtualisation%20KVM-Niveau%202%20%7C%20Pratique-16A34A)
![Réseaux IP](https://img.shields.io/badge/Réseaux%20IP-Niveau%202%20%7C%20Pratique-16A34A)
![Windows Server](https://img.shields.io/badge/Windows%20Server-Niveau%202%20%7C%20Pratique-16A34A)
![OPNsense](https://img.shields.io/badge/OPNsense-Niveau%202%20%7C%20Pratique-16A34A)
![Sécurité réseau](https://img.shields.io/badge/Sécurité%20réseau-Niveau%201%20%7C%20Découverte-2563EB)
![Nmap](https://img.shields.io/badge/Nmap-Niveau%201%20%7C%20Découverte-2563EB)
![Audit et diagnostic](https://img.shields.io/badge/Audit%20et%20diagnostic-Niveau%201%20%7C%20Découverte-2563EB)

<sub>Échelle : Niveau 1 — Découverte / Apprentissage · Niveau 2 — Pratique · Niveau 3 — Débutant · Niveau 4 — Intermédiaire · Niveau 5 — Expert</sub>

**Statut :** Projet en préparation.

## Un deuxième projet pour découvrir la sécurité réseau

Ce projet fait suite à mon premier homelab, dans lequel j'ai découvert Linux, la virtualisation avec KVM, la configuration d'un serveur DHCP Windows et le routage entre plusieurs sous-réseaux avec OPNsense.

Après avoir réussi à faire communiquer les différentes machines de mon laboratoire, je souhaite maintenant comprendre comment vérifier la sécurité de cette infrastructure.

Ce que j'ai déjà découvert dans mon premier projet :

- Linux : installation de Debian et utilisation de commandes pour gérer mon environnement.
- Virtualisation : création et configuration de machines virtuelles avec KVM et Virt-Manager.
- Réseau : adresses IP, sous-réseaux, DHCP, passerelles et routage.
- Windows Server : installation et configuration d'un serveur DHCP et DNS.
- OPNsense : configuration du routage et du relais DHCP entre plusieurs sous-réseaux.

Ces technologies ne sont donc plus totalement nouvelles pour moi. Je souhaite maintenant les réutiliser dans un contexte différent.

Ce que je découvre avec ce deuxième projet :

- Le fonctionnement des ports TCP et UDP.
- Les services accessibles sur un réseau.
- L'utilisation de Nmap.
- L'analyse des communications entre plusieurs sous-réseaux.
- Les règles de filtrage d'un pare-feu.
- Les risques liés aux configurations réseau.
- Les principes de durcissement.
- La réalisation d'un premier rapport d'audit.

J'apprends progressivement à partir de documentation technique, de tutoriels et de recherches personnelles.

Comme pour mon premier homelab, je souhaite comprendre les manipulations réalisées, les problèmes rencontrés et les résultats obtenus, plutôt que de simplement reproduire des commandes.

## Objectif du projet

Réaliser un premier audit de sécurité sur mon infrastructure virtualisée, identifier les machines et les services accessibles, puis améliorer progressivement sa configuration.

Le projet repose sur plusieurs questions :

1. **Quelles machines et quels services sont accessibles sur mon réseau ?**
2. **Quelles communications sont possibles entre les sous-réseaux ?**
3. **Certaines configurations présentent-elles des risques ?**
4. **Comment limiter les accès inutiles sans empêcher le réseau de fonctionner ?**
5. **Comment vérifier que les modifications apportées améliorent réellement la sécurité ?**

Je souhaite réaliser un audit initial, appliquer les corrections nécessaires, puis reproduire les tests pour comparer les résultats.

L'objectif est de découvrir une méthode simple d'analyse et de sécurisation d'une infrastructure réseau.

Ce projet constitue une initiation à la sécurité réseau et non un audit professionnel complet.

## 1. Infrastructure de départ

Je vais réutiliser l'environnement créé lors de mon premier projet.

L'infrastructure comprend trois machines virtuelles hébergées sur Debian avec KVM et Virt-Manager.

### Machines virtuelles existantes

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

OPNsense dispose de trois interfaces réseau, une pour chaque sous-réseau.

Le relais DHCP permet aux postes des réseaux Comptabilité et Commercial de recevoir une adresse IP depuis le serveur Windows.

### Contraintes matérielles

Mon PC hôte dispose de 8 Go de RAM.

Dans le premier projet, je n'avais donc pas créé plusieurs clients Windows. J'avais réutilisé la même VM, COMPTA-DEVICE1, en la connectant successivement aux réseaux Comptabilité et Commercial.

Je souhaite conserver cette approche pour le deuxième projet.

Une machine Linux dédiée aux tests pourra être ajoutée si nécessaire, selon les ressources disponibles.

## 2. Préparation de l'audit

Avant de commencer les tests, je dois vérifier que mon infrastructure fonctionne toujours et définir précisément ce que je souhaite analyser.

### Manipulations prévues

- [ ] Vérifier les machines virtuelles existantes.
- [ ] Vérifier les réseaux virtuels configurés dans KVM.
- [ ] Contrôler les adresses IP des machines.
- [ ] Vérifier les communications réseau.
- [ ] Déterminer depuis quelle machine effectuer les tests.
- [ ] Définir le périmètre de l'audit.
- [ ] Documenter l'architecture de départ.

Cette première étape doit me permettre de disposer d'un état initial fiable avant toute modification.

## 3. Découverte des machines et des services avec Nmap

Je souhaite commencer par comprendre comment identifier les équipements et les services accessibles sur un réseau.

Pour cela, je vais découvrir **Nmap**, un outil permettant notamment d'analyser les ports et certains services réseau.

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
- [ ] Documenter les commandes et leurs résultats.

Les tests seront réalisés uniquement sur mes propres machines virtuelles.

Un port ouvert ne constitue pas automatiquement une vulnérabilité. Je chercherai d'abord à comprendre le rôle du service et pourquoi il est accessible.

## 4. Analyse des communications entre sous-réseaux

Dans mon premier projet, j'ai configuré OPNsense pour permettre le routage entre les réseaux Serveur, Comptabilité et Commercial.

Je souhaite maintenant comprendre quelles communications sont réellement possibles entre ces réseaux.

### Questions à étudier

- Un poste du réseau Comptabilité peut-il accéder au réseau Commercial ?
- Quels services du serveur Windows sont accessibles depuis les réseaux clients ?
- Toutes les communications autorisées sont-elles nécessaires ?
- Certaines communications devraient-elles être limitées ?

### Manipulations prévues

- [ ] Tester les communications depuis le réseau Comptabilité.
- [ ] Tester les communications depuis le réseau Commercial.
- [ ] Identifier les services accessibles sur le réseau Serveur.
- [ ] Comparer les résultats selon le sous-réseau utilisé.
- [ ] Documenter les communications autorisées et bloquées.

Les résultats serviront de base à l'analyse du pare-feu OPNsense.

## 5. Découverte du pare-feu OPNsense

J'ai déjà utilisé OPNsense pour le routage et le relais DHCP, mais je n'ai pas encore étudié précisément son fonctionnement en tant que pare-feu.

Je souhaite découvrir comment ses règles permettent d'autoriser ou de bloquer les communications entre différents réseaux.

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
- [ ] Identifier les communications nécessaires.
- [ ] Préparer des règles de filtrage adaptées.
- [ ] Tester progressivement les modifications.
- [ ] Vérifier que le DHCP et les autres services utiles continuent de fonctionner.

L'objectif est de mieux isoler les sous-réseaux sans empêcher le fonctionnement normal de l'infrastructure.

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

Chaque modification sera documentée en expliquant le problème identifié, la correction appliquée et le résultat obtenu.

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

- Les objectifs de l'étape.
- Les notions techniques découvertes.
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
