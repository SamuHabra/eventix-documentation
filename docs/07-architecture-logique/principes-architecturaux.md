# Principes architecturaux — Eventix

| Champ | Valeur |
|---|---|
| **Phase** | 07 — Architecture logique |
| **Projet** | Eventix |
| **Périmètre** | MVP |
| **Marché initial** | Cameroun |
| **Statut** | Proposition — à valider par l'équipe |

---

## Table des matières

1. [Objectif](#1-objectif)
2. [Sources de référence](#2-sources-de-référence)
3. [Rôle des principes architecturaux](#3-rôle-des-principes-architecturaux)
4. [Principe 1 — Alignement sur les objectifs produit](#4-principe-1--alignement-sur-les-objectifs-produit)
5. [Principe 2 — Séparation stricte des cycles métier](#5-principe-2--séparation-stricte-des-cycles-métier)
6. [Principe 3 — Faible couplage, forte cohésion](#6-principe-3--faible-couplage-forte-cohésion)
7. [Principe 4 — Fiabilité avant débit](#7-principe-4--fiabilité-avant-débit)
8. [Principe 5 — Traçabilité native](#8-principe-5--traçabilité-native)
9. [Principe 6 — Idempotence des effets métier](#9-principe-6--idempotence-des-effets-métier)
10. [Principe 7 — Sécurité par conception](#10-principe-7--sécurité-par-conception)
11. [Principe 8 — Évolutivité contrôlée](#11-principe-8--évolutivité-contrôlée)
12. [Principe 9 — Simplicité opérationnelle](#12-principe-9--simplicité-opérationnelle)
13. [Principe 10 — Adaptation au contexte camerounais](#13-principe-10--adaptation-au-contexte-camerounais)
14. [Matrice de cohérence](#14-matrice-de-cohérence)
15. [Anti-principes](#15-anti-principes)
16. [Statut](#16-statut)

---

# 1. Objectif

Ce document définit les principes directeurs qui guideront les décisions d'architecture logique d'Eventix.

Il répond à la question :

> **Quelles règles fondamentales doivent structurer l'architecture pour servir les objectifs produit et respecter les contraintes du contexte camerounais ?**

Il ne définit **pas encore** :

- les composants techniques ;
- les frameworks ;
- les bases de données ;
- les protocoles de communication ;
- l'infrastructure cloud ou on-premise.

Ces éléments seront traités dans les phases ultérieures.

---

# 2. Sources de référence

Les principes sont dérivés de :

- `02-fondamentaux/objectifs-produit.md`
- `04-analyse-des-besoins/exigences-non-fonctionnelles.md`
- `04-analyse-des-besoins/exigences-fonctionnelles.md`

Les décisions prises au cours de l'analyse fonctionnelle sont prises en compte lorsqu'elles ont déjà été validées par l'équipe.

---

# 3. Rôle des principes architecturaux

Les principes architecturaux constituent des **règles de décision** qui :

- orientent les choix de conception sans les figer prématurément ;
- garantissent la cohérence entre les objectifs produit et la structure technique ;
- servent de référentiel pour arbitrer les compromis ;
- restent valables même si les technologies changent.

Ils sont **normatifs** : toute décision d'architecture qui les viole doit être explicitement justifiée et documentée.

---

# 4. Principe 1 — Alignement sur les objectifs produit

> **Toute décision d'architecture doit servir directement ou indirectement les objectifs produit définis.**

### Règle

Chaque choix architectural doit pouvoir être rattaché à au moins un objectif produit :

| Objectif produit | Implication architecturale |
|---|---|
| Faciliter la découverte | Performance de recherche, indexation efficace |
| Simplifier la gestion | Centralisation, unification des interfaces |
| Sécuriser les opérations | Traçabilité, contrôle d'accès, intégrité des données |
| Améliorer l'expérience | Disponibilité, réactivité, résilience |

### Application

- Avant d'adopter une solution technique, vérifier quel objectif elle sert.
- Si aucun objectif n'est servi, la solution est suspecte.
- Si plusieurs solutions servent le même objectif, privilégier celle qui en maximise l'impact.

---

# 5. Principe 2 — Séparation stricte des cycles métier

> **Les cycles métier fondamentaux doivent rester séparés dans l'architecture, même s'ils sont corrélés dans le temps.**

### Règle

L'architecture doit préserver les distinctions suivantes :

```text
Réservation
     ≠
Paiement
     ≠
Billet
     ≠
Présence
     ≠
Règlement financier
```

### Justification

Cette séparation :

- évite les effets de bord entre cycles ;
- permet d'évoluer chaque cycle indépendamment ;
- facilite la réconciliation en cas d'incohérence ;
- clarifie la responsabilité en cas d'anomalie.

### Application

- Ne jamais stocker l'état d'un cycle dans un autre.
- Ne jamais déduire l'état d'un cycle de celui d'un autre.
- Utiliser des références explicites entre cycles, jamais des imbrications.

---

# 6. Principe 3 — Faible couplage, forte cohésion

> **Les responsabilités métier doivent être clairement délimitées et regroupées par cohérence.**

### Règle

- Chaque responsabilité métier a une frontière claire.
- Les interactions entre responsabilités se font par des contrats explicites.
- Une responsabilité ne doit pas exposer ses détails internes.

### Justification

Le faible couplage :

- réduit l'impact des changements ;
- facilite les tests et la validation ;
- permet une évolution progressive ;
- limite les régressions.

### Application

- Définir des interfaces stables entre domaines.
- Éviter les dépendances circulaires.
- Préférer la composition à l'héritage.
- Isoler les règles métier des détails techniques.

> **Note :** Ce principe ne préjuge pas de la granularité technique (monolithe, modules, microservices). La décision sera prise ultérieurement.

---

# 7. Principe 4 — Fiabilité avant débit

> **Lorsque la cohérence ne peut plus être garantie, l'architecture doit privilégier la fiabilité sur le débit.**

### Règle

En cas de dégradation :

- réduire le débit plutôt que risquer l'incohérence ;
- basculer en mode dégradé contrôlé ;
- interdire les opérations concurrentes non synchronisées.

### Justification

Ce principe découle directement du cas de contrôle dégradé des exigences fonctionnelles. Il s'applique à tout domaine où la cohérence est critique :

- contrôle des billets ;
- gestion des disponibilités ;
- traitement des paiements ;
- émission des billets.

### Application

- Définir des modes dégradés explicites.
- Automatiser la détection de perte de cohérence.
- Prévoir la reprise après mode dégradé.
- Documenter les compromis acceptés.

---

# 8. Principe 5 — Traçabilité native

> **Toute opération métier importante doit laisser une trace persistante et consultable.**

### Règle

L'architecture doit permettre de :

- consigner chaque opération métier significative ;
- associer l'opération à son contexte (acteur, objet, date, résultat) ;
- préserver l'historique même après modification des données ;
- interroger l'historique sans impact sur les opérations courantes.

### Justification

La traçabilité est un objectif produit à part entière. Elle est indispensable pour :

- la sécurité et la lutte contre la fraude ;
- le suivi des opérations financières ;
- la résolution des litiges ;
- l'amélioration continue.

### Application

- Prévoir un mécanisme de journalisation métier dès la conception.
- Séparer les données courantes des données historiques.
- Garantir l'immuabilité des traces.
- Permettre l'interrogation transversale.

---

# 9. Principe 6 — Idempotence des effets métier

> **Les opérations critiques doivent produire un seul effet métier, même si elles sont reçues plusieurs fois.**

### Règle

Pour les opérations suivantes :

- confirmation de paiement ;
- émission de billet ;
- remboursement ;
- validation de billet ;

l'architecture doit garantir qu'une répétition ne produit pas d'effet supplémentaire.

### Justification

L'idempotence est indispensable dans un contexte où :

- les réseaux sont instables ;
- les confirmations peuvent arriver en double ;
- les retries sont nécessaires ;
- la confiance est fragile.

### Application

- Identifier chaque opération par un identifiant unique.
- Détecter les doublons avant traitement.
- Rendre les traitements réentrants.
- Consigner les tentatives de duplication.

---

# 10. Principe 7 — Sécurité par conception

> **La sécurité doit être intégrée dès la conception, pas ajoutée après coup.**

### Règle

- Appliquer le principe du moindre privilège.
- Séparer les données sensibles des données publiques.
- Protéger les points d'entrée critiques.
- Prévoir la détection et la réponse aux anomalies.

### Justification

Le contexte camerounais présente des risques spécifiques :

- fraude aux événements ;
- faux organisateurs ;
- billets contrefaits ;
- manipulation des ventes.

### Application

- Authentifier et autoriser systématiquement.
- Chiffrer les données sensibles.
- Journaliser les accès aux données critiques.
- Prévoir des mécanismes de suspension et de bannissement.

---

# 11. Principe 8 — Évolutivité contrôlée

> **L'architecture doit permettre l'évolution sans remise en cause globale.**

### Règle

- Isoler les variations derrière des interfaces stables.
- Prévoir les points d'extension identifiés.
- Éviter les dépendances rigides à des solutions externes.
- Permettre l'ajout de nouvelles fonctionnalités sans modifier le cœur.

### Justification

Eventix évoluera :

- nouveaux canaux de vente ;
- nouveaux moyens de paiement ;
- nouvelles fonctionnalités participatives ;
- extension géographique.

### Application

- Utiliser des adaptateurs pour les services externes.
- Définir des contrats de versionnement.
- Prévoir la migration des données comme cas normal.
- Documenter les points d'extension.

---

# 12. Principe 9 — Simplicité opérationnelle

> **L'architecture doit rester simple à comprendre, déployer et opérer.**

### Règle

- Préférer les solutions simples aux solutions complexes.
- Limiter le nombre de composants mobiles.
- Automatiser les opérations répétitives.
- Réduire les dépendances critiques.

### Justification

Le contexte opérationnel camerounais impose :

- des équipes techniques potentiellement réduites ;
- une maintenance à distance ;
- des ressources limitées ;
- une fiabilité réseau variable.

### Application

- Éviter la sur-ingénierie.
- Choisir des solutions éprouvées.
- Documenter les procédures d'exploitation.
- Prévoir l'observabilité dès la conception.

---

# 13. Principe 10 — Adaptation au contexte camerounais

> **L'architecture doit tenir compte des contraintes réelles du marché camerounais.**

### Règle

- Concevoir pour une connectivité intermittente.
- Optimiser pour les appareils mobiles modestes.
- Intégrer Mobile Money comme moyen de paiement principal.
- Prévoir des modes dégradés réalistes.

### Justification

Le marché camerounais se caractérise par :

- une pénétration smartphone croissante mais inégale ;
- une qualité de réseau variable ;
- une préférence pour Mobile Money ;
- des habitudes d'usage spécifiques.

### Application

- Minimiser les échanges réseau.
- Prévoir le cache et la reprise.
- Optimiser les temps de chargement.
- Tester dans des conditions réelles dégradées.

---

# 14. Matrice de cohérence

| Principe | Objectif produit servi | Exigences non-fonctionnelles concernées |
|---|---|---|
| Alignement sur les objectifs | Tous | Toutes |
| Séparation des cycles métier | Sécuriser, Simplifier | Intégrité, Maintenabilité |
| Faible couplage | Simplifier, Évolutivité | Maintenabilité, Testabilité |
| Fiabilité avant débit | Sécuriser, Expérience | Disponibilité, Cohérence |
| Traçabilité native | Sécuriser, Analyser | Auditabilité, Traçabilité |
| Idempotence | Sécuriser, Expérience | Cohérence, Fiabilité |
| Sécurité par conception | Sécuriser | Confidentialité, Intégrité |
| Évolutivité contrôlée | Découverte, Expérience | Extensibilité, Maintenabilité |
| Simplicité opérationnelle | Tous | Exploitabilité, Coût |
| Adaptation contexte | Découverte, Expérience | Performance, Résilience |

---

# 15. Anti-principes

Les choix suivants sont **explicitement exclus** :

| Anti-principe | Raison |
|---|---|
| Architecture distribuée prématurée | Complexité injustifiée pour le MVP |
| Base de données unique partagée | Couplage fort, perte de cohérence |
| Traitement synchrone de tout | Fragilité réseau, mauvaise expérience |
| Stockage de l'état métier dans le frontend | Perte de cohérence, risque de fraude |
| Absence de journalisation | Impossibilité d'audit et de réconciliation |
| Dépendance rigide à un prestataire | Verrouillage, perte d'évolutivité |

---

# 16. Statut

| Champ | Valeur |
|---|---|
| **Document** | `principes-architecturaux.md` |
| **Version** | 1.0 |
| **Statut** | Proposition — à valider par l'équipe |
| **Périmètre** | Architecture logique Eventix |
| **Marché** | Cameroun |

| Principe | État |
|---|---|
| Alignement sur les objectifs produit | ✅ DÉFINI |
| Séparation des cycles métier | ✅ DÉFINI |
| Faible couplage, forte cohésion | ✅ DÉFINI |
| Fiabilité avant débit | ✅ DÉFINI |
| Traçabilité native | ✅ DÉFINI |
| Idempotence des effets métier | ✅ DÉFINI |
| Sécurité par conception | ✅ DÉFINI |
| Évolutivité contrôlée | ✅ DÉFINI |
| Simplicité opérationnelle | ✅ DÉFINI |
| Adaptation au contexte camerounais | ✅ DÉFINI |

---

## Notes de rédaction

### Cohérence avec les sources

Ce document ne reprend pas les objectifs produit ni les exigences non-fonctionnelles. Il les **utilise** pour dériver des principes architecturaux directement opposables.

### Non-présomption technique

Aucun principe n'impose de choix technologique. Les principes sont formulés pour rester valables quelle que soit la stack retenue ultérieurement.

### Traçabilité

Chaque principe sera relié aux décisions d'architecture dans les documents suivants de la phase 07.
