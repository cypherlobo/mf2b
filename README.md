<p align="center">
  <img src="https://raw.githubusercontent.com/cypherlobo/mf2b/refs/heads/main/assets/hh.png" width="300">
</p>

# MF2B | Mini-Fail2Ban

[![Open Source Love](https://badges.frapsoft.com/os/v1/open-source.svg?v=102)](https://github.com/cypherlobo?tab=repositories)

[Version : 1.0]

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

## Prérequis

- Bash
- Ntables
- Rsyslogs
- Privilèges root pour l'exécution

## Installation Automatique 

1. Recuperer le paquet `mf2b-vX.Y.deb` dans les [Releases](https://github.com/cypherlobo/mf2b/releases)
2. Puis executer l'install du paquet:
```sh
sudo dpkg -i mf2b.*.deb
```
3. Modifier les valeur comme vous le souhaitez.
4. Activer le service et le demarrer avec cette commande:
```sh
sudo systemctl enable --now mf2b
```
5. Verifier le status du mf2b :
```sh
sudo systemctl status mf2b
```

## Installation Manuelle
1. Recuperer le repository:
```sh
git clone https://github.com/cypherlobo/mf2b.git
```
2. Effacer le fichier README.MD et le dossier assets:
```sh
rm -r mf2b/README.md mf2b/assets/
```
3.Puis compiler le paquet:
```sh
sudo dpkg-deb build mf2b
```
4. Puis executer l'install du paquet:
```sh
sudo dpkg -i mf2b.*.deb
```

5. Modifier les valeur comme vous le souhaitez.
6.  Activer le service et le demarrer avec cette commande:
```sh
sudo systemctl enable --now mf2b
```
7. Verifier le status du mf2b :
```sh
sudo systemctl status mf2b
```

## Utilisation

```bash
sudo mf2b [options]
```

Options :
- `-h, --help` : Affiche l'aide
- `-n, --nftables-rebuild` : Reconstruit la structure de base de nftables
- `-u, --unban IP` : Débannit l'adresse IP spécifiée


## Fonctionnement

Le script surveille en continu les fichiers de logs spécifiés. Lorsqu'une tentative d'authentification échouée est détectée, l'adresse IP est enregistrée. Si le nombre de tentatives dépasse le seuil défini dans la fenêtre de temps spécifiée, l'IP est automatiquement bannie via nftables.

## Sécurité

- Le script doit être exécuté avec des privilèges root
- Utilise des verrous pour éviter les conflits d'accès aux fichiers partagés

## Ajout des regles de Regex et l'analyse des logs:
Pour ajouter les analyses de logs et des regles
1. Modifier le fichier `/usr/local/bin/mf2b`
2. Ajouter la ligne qui corresponde a celle ci:
```sh
LOG_FILES_REGEX["/path/to/file.log"]="*REGEX*" 
LOG_FILES_REGEX["/path/to/file.log"]="(Failed password|authentication failure|password check failed).*? from ([0-9]+\.[0-9]+\.[0-9]+\.[0-9]+)"
```

## Logs

Les bannissements sont enregistrés dans le fichier spécifié par `BAN_LOG`.

## Troubleshooting
- SI pour une raison la structure de base de nftables est cassé vous pouvez la reconstruire avec la flag : `-n, --nftables-rebuild`.
- Si l'installation n'est pas abouti c'est que vous ne respectez les prerequis: rsyslog, nftables, whiptail.

## Avertissement

Assurez-vous de bien comprendre le fonctionnement du script avant de le déployer sur un système en production. Un mauvais paramétrage pourrait entraîner le bannissement d'adresses IP légitimes.
