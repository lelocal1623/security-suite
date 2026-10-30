# Security Suite

Module de sécurité WordPress commun à un ensemble de sites gérés par le même
exploitant.

> **Statut** : en construction. Il remplacera les modules de sécurité propres à
> chaque site.

## Principes

- **Un seul code pour tous les sites.** Ce qui diffère d'un site à l'autre passe par
  la configuration du site, jamais par une copie modifiée du code.
- **Les réglages ne se modifient pas depuis l'administration.** Un compte
  administrateur volé ne doit pas pouvoir couper les protections. La page d'admin
  affiche l'état en lecture seule.
- **Le plugin ne peut pas être désactivé depuis l'administration.** Un petit
  chargeur placé dans `mu-plugins` le maintient actif.
- **Chaque version publiée est signée.** Un site refuse toute mise à jour dont la
  signature ne correspond pas à la clé publique livrée avec le plugin.

## Structure

```
security-suite/
├── security-suite.php   point d'entrée du plugin
├── src/                 protections (connexion, durcissement, alertes, .htaccess…)
├── loader/              chargeur à placer dans wp-content/mu-plugins/
└── docs/                documentation
```

## Ce qui n'est PAS dans ce dépôt

Ce dépôt est public. N'y figurent jamais : la liste des sites, leurs adresses, leurs
réglages, des adresses IP, des secrets, ni la clé privée de signature.

## Licence

Copyright (C) 2026 Le Local - Patrick Yersin.

Ce programme est un logiciel libre ; vous pouvez le redistribuer et/ou le modifier
selon les termes de la GNU General Public License telle que publiée par la Free
Software Foundation, version 2 ou (à votre choix) toute version ultérieure.
Voir le fichier [LICENSE](LICENSE).
