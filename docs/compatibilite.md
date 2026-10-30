# Compatibilité — Wordfence et WP Rocket

Exigence (30.10.2026) : le plugin fonctionne **avec Wordfence et WP Rocket installés sur
chaque site**. Ce sont les deux extensions du socle standard. Le plugin ne doit ni les
contredire, ni refaire leur travail.

## Partage des rôles

| Fonction | Responsable | Rôle de security-suite |
|---|---|---|
| Pare-feu applicatif (WAF), règles d'attaque | **Wordfence** | Aucun. Vérifier seulement qu'il est actif et le signaler sinon |
| Analyse des fichiers (malware, fichiers du cœur modifiés) | **Wordfence** | Aucun |
| Limite d'essais de connexion, verrouillage, force brute | **Wordfence** | Aucun : pas de second compteur (double verrouillage, messages contradictoires) |
| Double authentification (2FA) | **Wordfence Login Security** | Vérifier qu'elle est **obligatoire** pour administrateurs et gestionnaires ; jamais pour les clients WooCommerce |
| Énumération des comptes, mots de passe d'application | **Wordfence** (réglages dédiés) | Vérifier que les réglages attendus sont actifs |
| Cache, minification, préchargement | **WP Rocket** | Aucun |
| Réglages de sécurité par site, non modifiables depuis l'admin | **security-suite** | Socle du plugin |
| Contrôle des installations d'extensions (`file_mod_allowed`) + alerte e-mail | **security-suite** | Wordfence ne bloque pas une installation faite par un admin connecté |
| Protection des sites de dev (mot de passe HTTP, identifiants par client) | **security-suite** | |
| Règles `.htaccess` propres au site (robots abusifs, en-têtes de sécurité) + contrôle d'intégrité | **security-suite** | Dans son propre bloc |
| Point de santé + version installée | **security-suite** | |
| **Contrôle de conformité** : le site respecte-t-il le socle standard ? | **security-suite** | Voir plus bas |

## Règles de cohabitation

### `.htaccess`
- Wordfence et WP Rocket écrivent chacun leur bloc (`# BEGIN WP Rocket`, `# Wordfence WAF`).
  security-suite écrit **uniquement entre ses propres marqueurs**, via `insert_with_markers()`,
  et ne modifie, ne déplace et ne supprime **jamais** un autre bloc.
- Ordre : les règles qui refusent un accès (`[F]`, mot de passe HTTP) doivent agir quelle que
  soit la réécriture de cache de WP Rocket. Les tester sur une page en cache ET sur un fichier
  statique (image).
- Après chaque écriture : relire le fichier et vérifier que les blocs WP Rocket et Wordfence
  sont toujours présents et intacts.

### Wordfence
- **Adresse IP du visiteur** : Wordfence a son propre réglage (« How does Wordfence get IPs »).
  Il doit être sur **`REMOTE_ADDR`** quand l'hébergeur transmet la vraie IP du visiteur (vérifié sur notre hébergement), comme security-suite. Jamais
  `X-Forwarded-For` (falsifiable). Le contrôle de conformité le vérifie.
- WAF chargé par `auto_prepend_file` (`.user.ini` → `wordfence-waf.php`) : security-suite n'y
  touche pas, et ne pose pas son propre `auto_prepend_file`.
- Pas de second système de blocage d'IP : en cas de besoin, utiliser les listes de Wordfence.

### WP Rocket
- Rien de ce que security-suite affiche ne doit dépendre de l'utilisateur sur une page publique
  (sinon WP Rocket le met en cache et le sert à tout le monde).
- Le point de santé utilise une chaîne de requête : vérifier qu'il n'est **jamais mis en cache**
  (en-tête `Cache-Control: no-store` + exclusion WP Rocket si nécessaire).
- Les en-têtes de sécurité (CSP en mode rapport) doivent tolérer la minification et le
  chargement différé du JavaScript de WP Rocket.
- `robots.txt` : WP Rocket ne le gère pas ; selon l'hébergeur, un fichier physique peut être indispensable.

## Contrôle de conformité (« socle standard »)

La page d'état (lecture seule) et la commande `status` de l'outil d'exploitation indiquent pour
chaque site :

- Wordfence actif, WAF en mode « protection étendue », IP lue sur `REMOTE_ADDR`
- 2FA Wordfence obligatoire pour administrateurs et gestionnaires
- Énumération des comptes bloquée, mots de passe d'application désactivés (sauf exception
  déclarée)
- WP Rocket actif, et **aucune autre extension de cache** active (conflits)
- Pas d'autre extension de sécurité en doublon
- Version de security-suite installée
