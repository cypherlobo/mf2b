# MF2B - Mini Fail 2 Ban

MF2B est un script bash conçu pour surveiller les fichiers de logs système, détecter les tentatives d'authentification échouées, et bannir automatiquement les adresses IP suspectes en utilisant nftables.

![mf2b](https://raw.githubusercontent.com/cypherlobo/mf2b/refs/heads/main/assets/mf2b.png)

## Fonctionnalités

- Surveillance en temps réel des fichiers de logs spécifiés
- Détection des tentatives d'authentification échouées basée sur des expressions régulières personnalisables
- Bannissement automatique des adresses IP après un certain nombre de tentatives dans une fenêtre de temps définie
- Utilisation de nftables pour le bannissement des IP
- Possibilité de débannir manuellement une IP
- Reconstruction de la structure nftables de base
- Gestion des verrous pour éviter les conflits d'accès aux fichiers

## Configuration

Le script utilise plusieurs variables de configuration :

- `LOG_FILES_REGEX` : Associe les fichiers de logs à surveiller avec leurs expressions régulières correspondantes
- `NFT_SET_NAME` : Nom de l'ensemble nftables pour les IP bannies
- `BAN_DURATION` : Durée du bannissement en secondes
- `MAX_ATTEMPTS` : Nombre maximal de tentatives avant bannissement
- `TIME_WINDOW` : Fenêtre de temps (en secondes) pour compter les tentatives
- `BAN_LOG` : Fichier de log pour les bannissements
- `LOCK_FILE` : Fichier de verrouillage
- `TRACKER_FILE` : Fichier de suivi des tentatives

## Utilisation

```bash
sudo mf2b [options]
```

Options :
- `-h, --help` : Affiche l'aide
- `-n, --nftables-rebuild` : Reconstruit la structure de base de nftables
- `-u, --unban IP` : Débannit l'adresse IP spécifiée

## Prérequis

- Bash
- Ntables
- Rsyslogs
- Privilèges root pour l'exécution

## Installation

1. Placez le script dans `/usr/local/bin/mf2b`
2. Rendez-le exécutable : `chmod +x /usr/local/bin/mf2b`
3. Assurez-vous que les dossiers et fichiers nécessaires existent et sont accessibles

## Fonctionnement

Le script surveille en continu les fichiers de logs spécifiés. Lorsqu'une tentative d'authentification échouée est détectée, l'adresse IP est enregistrée. Si le nombre de tentatives dépasse le seuil défini dans la fenêtre de temps spécifiée, l'IP est automatiquement bannie via nftables.

## Sécurité

- Le script doit être exécuté avec des privilèges root
- Utilise des verrous pour éviter les conflits d'accès aux fichiers partagés

## Logs

Les bannissements sont enregistrés dans le fichier spécifié par `BAN_LOG`.

## Avertissement

Assurez-vous de bien comprendre le fonctionnement du script avant de le déployer sur un système en production. Un mauvais paramétrage pourrait entraîner le bannissement d'adresses IP légitimes.
