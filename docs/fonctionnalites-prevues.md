# Fonctionnalités prévues

Demandes validées le 30.10.2026, à intégrer au plan de la base commune.

## Contrôle du socle (lecture seule)

- La page d'état indique, pour **Wordfence** et **WP Rocket** : installée ? activée ?
- Le même état remonte par le point de santé, pour l'outil d'exploitation (vue de tous les
  sites) et le récapitulatif.
- **Aucune installation ni réglage automatique** depuis le plugin (décision du 30.10.2026 :
  on reste simple). L'installation se fait à la main, comme d'habitude.
- Évolution possible plus tard, non engagée : comparer les réglages à une configuration de
  référence (Wordfence et WP Rocket savent exporter / importer leurs réglages).

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
