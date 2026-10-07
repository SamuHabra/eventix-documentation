# Communication logique — Eventix

## Objectif

Décrire les contraintes de communication sans choisir de protocole ou de fournisseur. Le mode d'échange sera déterminé par les invariants, la tolérance à l'indisponibilité et les volumes connus.

## Échanges existants

Les contrats des modules sont listés dans [modules.md](./modules.md) et leurs frontières dans [interfaces.md](./interfaces.md). La documentation actuelle n'autorise pas à conclure que les échanges doivent être synchrones, asynchrones ou événementiels.

## Traitement des signaux cybersécurité

- Les signaux vers MOD-13 sont des faits minimisés et non des appels de décision.
- Leur collecte ne doit pas retarder ni faire échouer une vente, un paiement, une émission de billet ou un contrôle d'accès.
- Un signal doit permettre d'identifier sa source, sa fraîcheur et son éventuelle indisponibilité ; la couverture visible du tableau de bord ne peut être déduite du seul silence.
- Les doublons, retards, pertes détectées et indisponibilités de source doivent être traités explicitement dans la conception.
- Les décisions humaines prises dans MOD-13 sont transmises aux modules cibles au moyen des contrats autorisés ; le module cible demeure arbitre de l'action sur ses données.
- La disponibilité et la rétention des journaux/signaux ne sont pas présumées ; elles dépendent d'une analyse de risques et de décisions d'exploitation.

## Décisions en attente

Le choix entre appels synchrones, événements asynchrones, collecte par lots ou intégration à un outil spécialisé sera pris après validation des sources, du volume, des objectifs de délai, de la confidentialité, du coût et des modes de défaillance. Aucun bus, SIEM, agent ou outil n'est réputé choisi.
