# 07 — Architecture logique

| Propriété | Valeur |
|---|---|
| **Phase** | 07 — Architecture logique |
| **Périmètre** | MVP Eventix |
| **Statut** | Architecture logique documentée ; décisions à valider avant réalisation |

## Objectif

Définir les responsabilités logiques, les frontières de modules, les contrats et les dépendances, sans choisir les technologies ni le mode de déploiement.

## Documents

| Document | Rôle |
|---|---|
| [Principes architecturaux](./principes-architecturaux.md) | Règles structurantes et contraintes de modularité |
| [Décomposition fonctionnelle](./decomposition-fonctionnelle.md) | Treize blocs logiques et traçabilité des 147 exigences |
| [Modules](./modules.md) | Propriété des données, responsabilités des modules et dépendances |
| [Responsabilités](./responsabilites.md) | Décisions et services hébergés par module |
| [Interfaces](./interfaces.md) | Contrats logiques entre modules |
| [Communication](./communication.md) | Modes d'échange à définir selon les contraintes métier |
| [Dépendances](./dependances.md) | Graphe logique et règles de dépendance |
| [Flux métier](./flux-metier.md) | Orchestration des parcours inter-modules |
| [Décisions architecturales](./decisions-architecturales.md) | Arbitrages, statut et justification |

## Décision de frontière cybersécurité

La supervision cybersécurité MVP relève de **BF-13 / BC-13 / MOD-13**, capacité logique interne distincte des douze contextes métier historiques. MOD-13 possède les alertes, les dossiers d'incident et les décisions humaines de réponse consignées. MOD-10 conserve les décisions Trust & Safety ; MOD-11 conserve l'analytique et l'historique métier. Les modules propriétaires exécutent toute action portant sur leurs propres données.

MOD-13 est une frontière logique, pas une décision de microservice ou d'hébergement séparé. Les mécanismes d'ingestion, les contrats d'action, la rétention, les habilitations détaillées et la couverture de détection restent à valider avant implémentation.

## Principes de cohérence

- Une alerte n'est pas une preuve d'attaque et ne déclenche aucune mesure automatiquement.
- Les responsabilités et sources de vérité métier des douze modules existants sont préservées.
- Les signaux partagés sont minimisés ; MOD-13 ne lit pas directement les bases des autres modules.
- Une décision cyber n'équivaut pas à une mesure métier : la qualification et l'exécution restent dans leurs domaines respectifs.
- La supervision ne doit pas devenir une dépendance synchrone des parcours de vente, paiement ou contrôle d'accès.
