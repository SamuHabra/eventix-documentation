# Services organisationnels Eventix

## Objectif

Ce dossier décrit, du point de vue de l'organisateur, les familles de services qu'Eventix rend possibles par l'intermédiaire de sa plateforme. Il explique les bénéficiaires, la valeur métier et les limites de chaque famille, en amont de la formalisation détaillée des exigences.

Le terme « service » désigne ici une capacité de l'offre Eventix. Il ne signifie pas qu'Eventix fournit automatiquement du personnel, du conseil, des prestations événementielles sur place ou un service géré.

## Périmètre et frontières

- **Marketing** : promouvoir une offre événementielle et mesurer l'origine des ventes, dans les limites des capacités documentées.
- **Finance** : rendre lisibles les ventes, les commissions et le règlement des organisateurs, sans présumer de conditions financières qui restent à définir.
- **Opérations événementielles** : outiller l'organisateur pour configurer et suivre son événement sur Eventix ; ce dossier ne couvre pas l'exploitation technique de la plateforme ni la logistique physique assurée par des tiers.
- **Sécurité et confiance métier** : contribuer à la fiabilité des événements, des billets et des accès ; la cybersécurité du système Eventix est traitée séparément dans [`../cybersecurite-eventix/`](../cybersecurite-eventix/README.md).

## Documents

| Famille | Document | Ancrage métier principal |
|---|---|---|
| Marketing | [marketing.md](./marketing.md) | B36 — Promouvoir et mesurer les ventes |
| Finance | [finance.md](./finance.md) | B33–B34 — Règlement organisateur et commissions des points physiques |
| Opérations événementielles | [operations-evenementielles.md](./operations-evenementielles.md) | B01–B04, B08–B13, B21–B25, B35, B37 |
| Sécurité et confiance métier | [securite-et-confiance.md](./securite-et-confiance.md) | RM13–RM17, RM28–RM30 |

## Règles de rédaction et de traçabilité

1. Partir d'un besoin ou d'une situation métier, pas d'une solution technique.
2. Référencer les identifiants existants dans [`../besoins-metier.md`](../besoins-metier.md) et [`../regles-metier.md`](../regles-metier.md) ; ne pas reformuler ces sources comme de nouvelles décisions.
3. Distinguer ce qui est déjà documenté, ce qui est une hypothèse et ce qui reste à arbitrer.
4. Ne pas introduire de promesse de service, de règle métier ou de capacité MVP qui ne soit pas validée dans les documents de référence.
5. Laisser les exigences vérifiables à la phase 04, le modèle de domaine à la phase 05 et les choix de conception aux phases suivantes.
