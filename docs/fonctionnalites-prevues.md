# Fonctionnalités prévues

Demandes validées le 30.10.2026, à intégrer au plan de la base commune.

## Installation du socle depuis la page d'état

- Le contrôle de conformité indique, par extension du socle : présente / manquante /
  réglages différents de la référence.
- **Wordfence** (répertoire officiel WordPress) : bouton « Installer et activer », via
  `Plugin_Upgrader`, soumis à la même règle que toute installation (`file_mod_allowed` :
  administrateur + conditions du site).
- **WP Rocket** (extension commerciale, absente du répertoire) : installée par l'outil
  d'exploitation à partir du paquet téléchargé une fois depuis le compte de l'éditeur. La clé
  de licence n'est jamais saisie dans l'admin ni stockée dans ce dépôt public : constantes
  prévues par l'éditeur dans `wp-config.php`.
- **Réglages de référence** : Wordfence et WP Rocket savent exporter / importer leur
  configuration. Une configuration de référence est validée sur un site pilote, puis
  appliquée aux autres ; le contrôle de conformité signale les écarts.

## Avertissement des mises à jour en attente (autres extensions, thèmes, cœur)

- Le site lit les mises à jour connues de WordPress (`update_plugins`, `update_themes`,
  `update_core`) et les expose sur le point de santé authentifié.
- L'outil d'exploitation les **regroupe pour tous les sites** dans un récapitulatif unique
  (hebdomadaire), plutôt qu'une alerte par site.
- **Alerte immédiate seulement si urgent** : mise à jour corrigeant une faille connue — en
  s'appuyant sur ce que Wordfence signale déjà (extensions vulnérables), sans refaire sa base.
- Pas de répétition : alerte uniquement sur changement d'état.
- Option : mises à jour automatiques pour une liste d'extensions jugées sans risque (liste
  par site dans le dépôt d'exploitation).
