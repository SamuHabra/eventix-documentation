# Dépendances logiques — Eventix

## Règles

1. Un module ne lit ni ne modifie directement les données d'un autre module.
2. Chaque ressource métier a un propriétaire unique.
3. Les modules métier n'attendent pas MOD-13 pour réaliser leurs parcours nominaux.
4. MOD-13 consomme des signaux de sécurité minimisés et ne devient pas une dépendance synchrone des opérations critiques.
5. Toute décision d'action cyber est humaine ; l'application d'une transition reste sous l'autorité du module propriétaire.
6. MOD-10 et MOD-13 sont deux frontières distinctes : mesures de confiance métier d'un côté, qualification des incidents du système de l'autre.
7. MOD-11 reste une capacité de statistiques et faits métier ; il n'est pas la source autoritaire des alertes cyber ni le journal d'audit de MOD-13.

## Vue dépendances cyber

```text
MOD-01..MOD-12 ── SignauxDeSécurité (faits minimisés) ──> MOD-13
MOD-01 ── IdentitéEtHabilitations ──> MOD-13
Responsable humain habilité ── décision tracée ──> MOD-13
MOD-13 ── DécisionDeRéponseCyber autorisée ──> module propriétaire
Module propriétaire ── RésultatDeRéponseCyber ──> MOD-13

MOD-10 (Trust & Safety métier)   ╳   MOD-13 (cybersécurité système)
MOD-11 (analytics/observabilité métier) n'est pas la source de vérité cyber
```

Les flèches représentent des dépendances de contrat, pas une topologie de déploiement ou un ordre de transport.

## Modes de défaillance à couvrir

La phase de conception doit définir le comportement à la perte d'un signal, au retard ou au doublon, à l'indisponibilité de MOD-13, à une décision devenue obsolète et à l'échec d'exécution par le module cible. La vente et le contrôle ne doivent pas être implicitement bloqués par l'indisponibilité du tableau de bord.
