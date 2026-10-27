
# 01 - Préparation du laboratoire

## Objectif

Ce deuxième projet s'appuie sur l'infrastructure virtualisée réalisée dans mon premier homelab.

Avant de commencer les tests de sécurité, il est nécessaire de vérifier les machines virtuelles, les réseaux et le fonctionnement des principaux services.

Cette préparation permettra de disposer d'un environnement fonctionnel et d'identifier précisément les éléments à auditer.

## 1. Rappel de l'infrastructure

L'environnement repose sur Debian, KVM et Virt-Manager.

L'architecture a été mise en place dans mon premier projet :

[Lab de virtualisation KVM : serveur DHCP et poste comptabilité](https://github.com/Sauvard-Bryan/Homelab-kvm-serveur-dhcp)

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

OPNsense assure le routage entre les trois sous-réseaux et le relais DHCP vers le serveur Windows.

## 2. Vérification des machines virtuelles

La première vérification consiste à contrôler la présence et l'état des machines virtuelles dans KVM.

Depuis le terminal Debian, la commande suivante permet d'obtenir la liste des machines enregistrées dans libvirt :

```bash
sudo virsh list --all
```

Cette commande utilise `virsh`, l'outil de gestion des machines virtuelles de libvirt.

L'option `--all` permet d'afficher les machines démarrées et arrêtées.

### Résultat

À compléter après exécution de la commande.

## 3. Vérification des réseaux virtuels

La deuxième vérification concerne les réseaux virtuels utilisés par les machines.

La commande suivante permet d'afficher les réseaux enregistrés dans libvirt :

```bash
sudo virsh net-list --all
```

L'option `--all` permet également d'afficher les réseaux inactifs.

### Résultat

À compléter après exécution de la commande.

## 4. Vérification des communications réseau

Une fois les machines et les réseaux identifiés, des tests permettront de vérifier les communications entre les différents équipements.

Cette étape servira notamment à contrôler le fonctionnement du routage et du service DHCP avant les tests de sécurité.

### Résultats

À compléter après les vérifications.

## 5. Définition du périmètre d'audit

L'audit sera limité aux machines et aux réseaux de mon propre laboratoire.

Il portera principalement sur :

- Les ports et services accessibles sur les machines virtuelles.
- Les communications entre les réseaux Serveur, Comptabilité et Commercial.
- Les règles de filtrage du pare-feu OPNsense.

La machine utilisée pour réaliser les tests sera déterminée en fonction des ressources disponibles et des outils nécessaires.

## 6. Problèmes rencontrés

À compléter si des difficultés apparaissent pendant la préparation du laboratoire.

## 7. Bilan

À compléter une fois les vérifications terminées.
