# Décisions architecturales — Eventix

Les décisions ci-dessous fixent des frontières logiques. Elles ne choisissent ni technologie, ni transport, ni infrastructure, ni topologie de déploiement.

| ID | Décision | Justification | Portée / suites |
|---|---|---|---|
| D-ARCH-01 | Conserver MOD-01 à MOD-12 alignés sur les douze bounded contexts métier actuels. | Préserver les responsabilités, invariants et sources de vérité déjà documentés. | Ne modifie pas les douze contextes existants. |
| D-ARCH-02 | Créer BF-13 / BC-13 / MOD-13 `cybersecurity-operations` comme capacité logique interne dédiée à EF-145–EF-147. | Le traitement des alertes et incidents système doit être distinct de Trust & Safety (MOD-10) et de l'analytique/observabilité métier (MOD-11). | Cadrer ses entités, agrégats, contrats et flux en phases 05–08. |
| D-ARCH-03 | MOD-13 possède les alertes, dossiers d'incident cyber et décisions humaines enregistrées ; le module propriétaire d'une ressource reste seul habilité à changer son état métier. | Respecter RM40, la propriété des données et la règle d'absence de mesures automatiques déclenchées par une alerte. | Toute exécution à distance nécessite un contrat autorisé et une approbation humaine traçable ; aucun appel direct à une base ou à un état interne. |
| D-ARCH-04 | MOD-13 ingère des signaux minimisés par contrats, sans constituer une dépendance synchrone des parcours de vente, paiement et contrôle. | Une panne de supervision ne doit pas empêcher les opérations métier ; la collecte doit respecter le moindre privilège et la minimisation. | Définir les sources, couverture, disponibilité, rétention et alertes de perte de télémétrie avant réalisation. |
| D-ARCH-05 | Le déploiement de MOD-13 reste indéterminé. | Un module logique ne préjuge pas d'un service déployé séparément. | Décision dans les phases 09–10 selon sécurité, résilience, volume et coût. |
| D-ARCH-06 | La sécurité des paiements est portée par les propriétaires des opérations financières : MOD-05 (paiement), MOD-09 (remboursement), MOD-08 (retrait/règlement). MOD-13 peut recevoir des signaux de sécurité, mais ne devient ni processeur ni registre financier. | Préserver l'autorité financière de chaque contexte et limiter la collecte de données sensibles. | Voir [sécurité des paiements](../13-securite/securite-des-paiements.md) ; préciser le périmètre contractuel des prestataires avant intégration. |

## Décisions antérieures conservées

- MOD-10 reste propriétaire de l'analyse des signalements métier et des mesures de confiance.
- MOD-11 reste un consommateur en lecture pour statistiques et faits métier ; il ne décide pas et ne remplace pas le journal d'audit spécialisé.
- Les douze modules initiaux, leurs propriétaires de données et les contrats existants restent inchangés, sous réserve de l'ajout de contrats minimisés de signalement cyber.
