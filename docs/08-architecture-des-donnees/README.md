# 08 — Architecture des données

## Objectif

Définir les données persistées par Eventix, leur propriétaire, leur classification, leurs flux, leurs règles de qualité et leur cycle de conservation. Cette phase prolonge les frontières logiques de la phase 07 sans choisir prématurément un moteur, un schéma physique ou une topologie de stockage.

## Principes de propriété

- Chaque donnée métier possède un module propriétaire unique ; un autre module ne modifie pas directement son état.
- Les échanges inter-modules exposent des contrats minimisés et des références opaques lorsque le détail de la donnée source n'est pas nécessaire.
- Les projections analytiques et tableaux de bord ne deviennent pas des sources de vérité métier.
- Les données de paiement, d'identité et de sécurité nécessitent une classification, des accès et des durées de conservation justifiés avant mise en production.
- Les signaux cyber, alertes, incidents et décisions consignés par MOD-13 ne donnent pas à ce module la propriété des données de l'actif observé. Les données financières demeurent sous la propriété de MOD-05, MOD-08 et MOD-09 selon le cycle concerné.
- Les journaux d'audit, historiques métier et télémétrie de sécurité ont des finalités et des politiques de conservation distinctes ; aucune durée n'est fixée ici.

## Données transversales à concevoir

| Ensemble | Propriétaire logique | Exigences à établir |
|---|---|---|
| Identité, comptes et habilitations | MOD-01 | Classification, moindre privilège, cycle de vie, accès administratif |
| Commandes, paiements, remboursements et soldes | MOD-03 à MOD-09 selon le cycle ; voir `modules.md` | Minimisation, intégrité, rapprochement, audit, conservation et séparation des opérations |
| Historique et faits métier | MOD-11 | Sources autorisées, intégrité, accès et durée de conservation |
| Signaux de sécurité, alertes, incidents et décisions cyber | MOD-13 | Provenance, minimisation, accès restreint, intégrité, rétention et purge |
| Projections et indicateurs | MOD-11 | Agrégation, finalité, anonymisation éventuelle et prévention des usages comme preuve financière |

## Livrables de conception à produire

1. Inventaire des entités, champs sensibles, propriétaires et finalités.
2. Flux de données et contrats entre modules, incluant les flux de supervision cyber décrits en phase 07.
3. Politique de classification, accès, rétention, archivage et suppression, validée avec les responsables compétents.
4. Modèle conceptuel et logique, aligné sur les entités et agrégats de la phase 05.
5. Stratégie d'intégrité, sauvegarde, restauration, continuité et preuve d'audit.
6. Vérification des exigences de protection des données applicables au marché et aux prestataires retenus.

## Statut

**Cadrage initial.** Les propriétaires logiques et principes ci-dessus sont établis ; l'inventaire physique, les durées, les schémas, les emplacements et les mécanismes restent à concevoir et à valider. Ne pas considérer cette phase comme une architecture de données prête à implémenter.
