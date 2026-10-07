# Décomposition fonctionnelle — Eventix

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
3. [Méthode de décomposition](#3-méthode-de-décomposition)
4. [Vue d'ensemble des blocs fonctionnels](#4-vue-densemble-des-blocs-fonctionnels)
5. [Famille 1 — Chaîne d'achat participant](#5-famille-1--chaîne-dachat-participant)
6. [Famille 2 — Cycle de vie de l'événement](#6-famille-2--cycle-de-vie-de-lévénement)
7. [Famille 3 — Boucle financière](#7-famille-3--boucle-financière)
8. [Famille 4 — Capacités transversales](#8-famille-4--capacités-transversales)
9. [Lecture temporelle : avant, pendant, après](#9-lecture-temporelle--avant-pendant-après)
10. [Traçabilité blocs ↔ exigences fonctionnelles](#10-traçabilité-blocs--exigences-fonctionnelles)
11. [Frontières et exclusions explicites](#11-frontières-et-exclusions-explicites)
12. [Incohérence connue et arbitrages en attente](#12-incohérence-connue-et-arbitrages-en-attente)
13. [Ce que la décomposition ne préjuge pas](#13-ce-que-la-décomposition-ne-préjuge-pas)
14. [Résumé](#14-résumé)
15. [Critères de qualité du document](#15-critères-de-qualité-du-document)
16. [Statut](#16-statut)

---

# 1. Objectif

Ce document découpe le système Eventix en **blocs fonctionnels** : des unités de capacité métier décrites du point de vue de ce que le système fait, indépendamment de la manière dont il sera construit.

Il répond à la question :

> **Quelles grandes fonctions Eventix doit-il assumer, comment se regroupent-elles, et où passent les frontières entre elles ?**

La décomposition fonctionnelle constitue le premier document opérationnel de la phase 07 : elle traduit les bounded contexts validés en phase 05 en structure fonctionnelle, prépare la définition des modules (`modules.md`) et alimentera les interfaces et flux métier des documents suivants.

Il ne définit **pas encore** :

- les modules logiciels ni leur granularité de déploiement (→ `modules.md`) ;
- les contrats d'interface entre blocs (→ `interfaces.md`) ;
- les modes de communication sync/async (→ `communication.md`) ;
- les technologies, bases de données ou infrastructure (→ phases 08 et suivantes).

---

# 2. Sources de référence

Ce document est dérivé de :

- `07-architecture-logique/principes-architecturaux.md` — principes P1 à P10 opposables aux décisions de découpage ;
- `05-domain-driven-design/bounded-contexts.md` — les treize bounded contexts, leurs responsabilités et leur catégorie (cœur / soutien / générique) ;
- `05-domain-driven-design/context-map.md` — les relations dirigées entre contextes, qui deviennent les entrées et sorties des blocs ;
- `06-modelisation-uml/diagrammes-de-composants.md` — le grain retenu (un composant par bounded context) et la table de traçabilité des interfaces ;
- `04-analyse-des-besoins/exigences-fonctionnelles.md` — les exigences EF-001 à EF-147, qui fondent la traçabilité de la section 10.

Toute responsabilité, relation ou exigence mentionnée implicitement renvoie à ces documents sources.

---

# 3. Méthode de décomposition

## 3.1. Un bloc fonctionnel par bounded context

Le grain de la décomposition reprend celui arrêté en phase 06 : **un bloc fonctionnel `BF-nn` correspond exactement au bounded context `BC-nn`**. Cette correspondance un-à-un garantit :

- la continuité de traçabilité depuis les sous-domaines jusqu'à l'architecture ;
- l'alignement du découpage fonctionnel sur les frontières de langage ubiquitaire ;
- l'absence de découpage arbitraire supplémentaire.

Le choix inverse (regrouper ou scinder les contexts à ce stade) aurait créé un écart entre la modélisation UML validée et l'architecture logique, sans justification nouvelle.

## 3.2. Un regroupement en familles fonctionnelles

Au-dessus des blocs, la décomposition introduit **quatre familles fonctionnelles** qui rassemblent les blocs selon leur rôle dans le parcours global :

| Famille | Rôle | Blocs |
|---|---|---|
| **F1 — Chaîne d'achat participant** | De la découverte d'un événement au billet en main | BF-03, BF-04, BF-05, BF-06, BF-12 |
| **F2 — Cycle de vie de l'événement** | Configurer, publier, opérer et clore un événement | BF-02, BF-07 |
| **F3 — Boucle financière** | Clôturer, rembourser, régler | BF-08, BF-09 |
| **F4 — Capacités transversales** | Servir tous les parcours sans y appartenir | BF-01, BF-10, BF-11, BF-13 |

Les familles sont un **regroupement de lecture et de conception** : elles n'introduisent pas de nouvelle frontière technique et ne préjugent pas d'un déploiement commun des blocs d'une même famille.

## 3.3. Une description par capacités tracées

Chaque bloc est décrit par :

- son **rôle fonctionnel** (une phrase) ;
- ses **capacités** (ce qu'il permet de faire), chacune rattachée aux exigences fonctionnelles qu'elle satisfait ;
- ses **entrées et sorties**, héritées des relations de la context map ;
- les **principes architecturaux dominants** qui s'y appliquent.

## 3.4. Des principes opposables

Chaque décision de ce document est vérifiable contre les principes de `principes-architecturaux.md`. La matrice de la section 10 relie les blocs, les exigences et les principes.

---

# 4. Vue d'ensemble des blocs fonctionnels

| Bloc | Nom | Famille | Catégorie DDD | Rôle en une phrase |
|---|---|---|---|---|
| `BF-01` | Identity & Access | F4 | Générique | Gérer comptes, capacités organisateur et organisations |
| `BF-02` | Event Catalog | F2 | Cœur | Référentiel central des événements et de leur configuration |
| `BF-03` | Event Discovery | F1 | Cœur | Exposer les événements publiés aux participants |
| `BF-04` | Booking & Availability | F1 | Soutien | Arbitrer l'attribution temporaire des disponibilités |
| `BF-05` | Payment Processing | F1 | Générique | Traiter les paiements Mobile Money et leurs aléas |
| `BF-06` | Ticketing & Fulfillment | F1 | Cœur | Émettre, détenir et transférer les billets |
| `BF-07` | Access Control | F2 | Cœur | Contrôler les entrées, y compris en mode dégradé |
| `BF-08` | Financial Settlement | F3 | Soutien | Clôturer financièrement et tenir le solde organisateur |
| `BF-09` | Refund Management | F3 | Soutien | Déterminer et exécuter les remboursements |
| `BF-10` | Trust & Safety | F4 | Générique | Analyser les signalements et décider les mesures |
| `BF-11` | Analytics & Observability | F4 | Cœur | Collecter les faits, produire statistiques et historique |
| `BF-12` | Communication | F1 | Générique | Transmettre billets et notifications aux participants |
| `BF-13` | Cybersecurity Operations | F4 — transversal interne | Soutien | Recevoir des signaux minimisés, qualifier les incidents cyber et tracer la décision humaine |

```text
                         ┌──────────────────────────────────────────┐
                         │            F4 — TRANSVERSAL              │
                         │  BF-01 Identity   BF-10 Trust   BF-11    │
                         │       & Access        & Safety  Analytics│
                         └───────▲──────────────▲──────────▲────────┘
                                 │              │          │
        ╔════════════════════════╪══════════════╪══════════╪═══════╗
        ║                        │              │          │       ║
        ║   F1 — CHAÎNE D'ACHAT  │              │          │       ║
        ║                                                        ║
        ║   BF-03 ──► BF-04 ──► BF-05 ──► BF-06 ──► BF-12       ║
        ║  Découverte Réservation Paiement Billetterie Distribution║
        ╚═══════════════════════════════════════════╤═════════════╝
                                                    │
        ╔════════════════════════════════════════════╪═════════════╗
        ║   F2 — CYCLE DE VIE DE L'ÉVÉNEMENT          ▼             ║
        ║                                                          ║
        ║   BF-02 (référentiel, avant) ──► BF-07 (contrôle, pendant)║
        ╚════════════════════════════════════════════╤═════════════╝
                                                    │
        ╔════════════════════════════════════════════╪═════════════╗
        ║   F3 — BOUCLE FINANCIÈRE (après)             ▼             ║
        ║                                                          ║
        ║   BF-09 Refund ──► BF-08 Settlement ──► Organisateur     ║
        ╚══════════════════════════════════════════════════════════╝
```

Le flux dominant se lit de haut en bas : le participant découvre et achète (F1), l'événement se déroule (F2), l'argent est réglé (F3), pendant que les capacités transversales (F4) servent toutes les étapes.

---

# 5. Famille 1 — Chaîne d'achat participant

Cette famille porte le parcours commercial complet : **découvrir → réserver → appliquer une réduction éventuelle → payer → obtenir son billet → le recevoir**. L'exigence EF-118 (vendre un billet en ligne) est une exigence de bout en bout portée par la famille entière, pas par un bloc isolé. Un billet gratuit sans don conserve le parcours de confirmation interne à 0 XAF sans appel au prestataire ; un don positif ajoute un paiement via BF-05. Les billets peuvent autoriser un accès physique, en ligne ou hybride.

## 5.1. BF-03 — Event Discovery

| Champ | Valeur |
|---|---|
| **Bounded Context source** | BC-03 — Event Discovery |
| **Catégorie** | Cœur |
| **Rôle fonctionnel** | Porte d'entrée du parcours participant : exposer le catalogue des événements publiés |
| **Entrées** | Événements publiés et disponibilités (BF-02) ; critères de recherche du participant |
| **Sorties** | Parcours de découverte (participant) ; demande de réservation (BF-04) |
| **Principes dominants** | P1 (alignement produit), P10 (optimisation mobile) |

**Capacités fonctionnelles :**

- Rechercher des événements (EF-025)
- Consulter les événements disponibles (EF-026) et leur détail (EF-027)
- Filtrer selon les intérêts et la localisation (EF-028)
- Présenter la disponibilité visible (EF-029)

BF-03 ne génère pas de données propres : il transforme et expose celles de BF-02. La frontière est nette : **BF-02 gère, BF-03 expose.**

## 5.2. BF-04 — Booking & Availability

| Champ | Valeur |
|---|---|
| **Bounded Context source** | BC-04 — Booking & Availability |
| **Catégorie** | Soutien |
| **Rôle fonctionnel** | Arbitre unique de l'attribution temporaire des disponibilités |
| **Entrées** | Demande de réservation (BF-03) ; catégories et disponibilités (BF-02) |
| **Sorties** | Réservation valide à payer (BF-05) ; libération à l'expiration |
| **Principes dominants** | P2 (séparation des cycles), P4 (fiabilité avant débit) |

**Capacités fonctionnelles :**

- Créer une réservation temporaire (EF-030) et bloquer la disponibilité (EF-031)
- Limiter la réservation à cinq minutes (EF-032)
- Libérer la disponibilité à l'expiration (EF-033)
- Empêcher la double attribution d'une même disponibilité (EF-034)
- Centraliser la disponibilité des ventes (EF-119) — la configuration source restant détenue par BF-02
- Réserver une place numérotée de façon exclusive avec la commande (EF-143)

BF-04 matérialise le principe P2 : la réservation est un cycle distinct du paiement, avec sa propre expiration, indépendante de l'état du paiement.

## 5.3. BF-05 — Payment Processing

| Champ | Valeur |
|---|---|
| **Bounded Context source** | BC-05 — Payment Processing |
| **Catégorie** | Générique |
| **Rôle fonctionnel** | Traiter les paiements Mobile Money et leurs aléas (échecs, retards, doublons) |
| **Entrées** | Réservation valide (BF-04) ; accusés du prestataire Mobile Money (externe) |
| **Sorties** | Paiement confirmé (BF-06) ; paiement à rembourser (BF-09) |
| **Principes dominants** | P2, P6 (idempotence), P10 (Mobile Money) |

**Capacités fonctionnelles :**

- Initier un paiement (EF-035) via Mobile Money, seul moyen du MVP (EF-036)
- Suivre l'état d'un paiement (EF-037)
- Traiter une confirmation avec effet unique (EF-038, EF-039)
- Gérer un paiement échoué (EF-040)
- Réconcilier un paiement tardif (EF-041) — déterminer billet ou remboursement sans jamais ignorer le paiement
- Intégrer le don positif au montant total à payer et l'enregistrer séparément du prix du billet (EF-135, EF-136)
- Valider les codes promotionnels configurés et intégrer leur réduction au montant à payer (EF-144)

## 5.4. BF-06 — Ticketing & Fulfillment

| Champ | Valeur |
|---|---|
| **Bounded Context source** | BC-06 — Ticketing & Fulfillment |
| **Catégorie** | Cœur |
| **Rôle fonctionnel** | Cœur de la promesse billetterie : émettre les billets, gérer leur propriété et leurs transferts |
| **Entrées** | Paiement confirmé (BF-05) ; événement et catégorie (BF-02) ; achat gratuit (participant) |
| **Sorties** | Billets à contrôler (BF-07) ; billets à distribuer (BF-12) ; billets consultables (participant) ; achats finalisés (BF-08) ; billets concernés (BF-09) |
| **Principes dominants** | P2, P5 (traçabilité des transferts), P6 (émission unique) |

**Capacités fonctionnelles :**

- Finaliser un achat (EF-042)
- Émettre un billet avec son identifiant de contrôle (EF-043, EF-044), l'associer à son événement (EF-045) et à son propriétaire (EF-046)
- Garantir l'unicité du propriétaire actif (EF-047)
- Permettre la consultation (EF-048) et le téléchargement (EF-049) des billets
- Émettre un billet pour un événement gratuit (EF-051) sans recréation en cas d'échec technique (EF-052)
- Émettre un billet avec le quota d'entrées configuré pour un pass multi-jours (EF-131)
- Gérer les transferts : transférer (EF-053), modifier le propriétaire actif (EF-054), conserver l'historique (EF-055)
- Invalider les billets concernés par une annulation d'événement (EF-074), sur décision de BF-02

## 5.5. BF-12 — Communication

| Champ | Valeur |
|---|---|
| **Bounded Context source** | BC-12 — Communication |
| **Catégorie** | Générique |
| **Rôle fonctionnel** | Canal de transmission vers les participants : billets et notifications |
| **Entrées** | Billets émis (BF-06) ; événements annulés ou reportés (BF-02) |
| **Sorties** | Communications (participant) |
| **Principes dominants** | P9 (simplicité — canal interchangeable), P10 (réseau intermittent) |

**Capacités fonctionnelles :**

- Mettre le billet à disposition du participant (EF-121)
- Distribuer le billet par email (EF-050, EF-122)
- Communiquer aux détenteurs éligibles les informations d'accès au direct ou à la VOD (EF-139)
- Informer les participants des annulations (EF-075) et des changements importants (EF-123)

---

# 6. Famille 2 — Cycle de vie de l'événement

Cette famille couvre la trajectoire de l'événement lui-même : sa configuration en amont (BF-02) et son déroulement sur site (BF-07). Elle est le point de rencontre des parcours organisateur et participant.

## 6.1. BF-02 — Event Catalog

| Champ | Valeur |
|---|---|
| **Bounded Context source** | BC-02 — Event Catalog |
| **Catégorie** | Cœur |
| **Rôle fonctionnel** | Référentiel central : créer, configurer et piloter le cycle de vie des événements |
| **Entrées** | Organisateur autorisé (BF-01) ; décisions de sécurité (BF-10) ; statut des billets utilisés (BF-07 — direction héritée des sources, voir §12) |
| **Sorties** | Événements publiés (BF-03) ; catégories et disponibilités (BF-04) ; événement et catégorie pour émission (BF-06) ; événement et points d'entrée (BF-07) ; annulations et reports (BF-12) |
| **Principes dominants** | P5 (historique non réécrit), P2, P8 (évolutivité contrôlée) |

**Capacités fonctionnelles :**

- Créer (EF-008), enregistrer en brouillon (EF-009), configurer (EF-010, EF-138) et modifier (EF-011) un événement
- Soumettre à vérification (EF-012), vérifier (EF-013) et publier un événement validé (EF-016)
- Piloter les états : consultation (EF-017), arrêt des ventes (EF-018, EF-019), archivage (EF-020)
- Décrire les espaces, zones et places (EF-021), associer catégories et disponibilités (EF-022), gérer les capacités (EF-023) sans réduction incompatible avec les billets attribués (EF-024), configurer le plan interactif (EF-142)
- Annuler (EF-072) en bloquant les nouvelles ventes (EF-073) ; reporter (EF-077) en conservant les billets (EF-078) et en gérant les incompatibilités (EF-079)
- Interdire la suppression d'un événement ayant des opérations irréversibles (EF-124), la remplacer par une gestion d'état (EF-125)
- Configurer les passes multi-jours (validité et quota d'entrées, EF-130) et activer les dons optionnels avec leurs montants suggérés (EF-134)
- Configurer les codes promotionnels et leurs conditions d'application (EF-140)

## 6.2. BF-07 — Access Control

| Champ | Valeur |
|---|---|
| **Bounded Context source** | BC-07 — Access Control |
| **Catégorie** | Cœur |
| **Rôle fonctionnel** | Garantir l'entrée légitime et unique : contrôle des billets à l'entrée, y compris en mode dégradé |
| **Entrées** | Billets émis (BF-06) ; événement et points d'entrée (BF-02) |
| **Sorties** | Événements de présence (BF-11) ; statut des billets utilisés (BF-02 — voir §12) |
| **Principes dominants** | P4 (mode dégradé), P6 (validation unique), P10 (connectivité variable) |

**Capacités fonctionnelles :**

- Affecter un contrôleur à un point d'entrée (EF-057)
- Scanner un billet (EF-058) et vérifier son authenticité (EF-059), son événement (EF-060) et son état (EF-061)
- Refuser un billet déjà utilisé (EF-062), annulé (EF-063) ou d'un autre événement (EF-064) ; autoriser un billet valide (EF-065)
- Marquer le billet comme utilisé (EF-066) avec une seule validation réussie possible (EF-067) et enregistrement du contexte du contrôle (EF-068)
- Consommer une entrée par contrôle accepté d'un pass et refuser un pass épuisé ou hors validité (EF-132, EF-133)
- Autoriser plusieurs scanners quand l'état partagé est fiable (EF-069)
- Basculer en mode mono-scanner dégradé (EF-070) et reprendre la synchronisation après resynchronisation (EF-071)

Le mode dégradé (EF-070) et sa réintégration (EF-071) sont des sous-domaines cœur locaux hérités des contraintes camerounaises : BF-07 est le bloc où le principe P4 s'applique le plus littéralement.

---

# 7. Famille 3 — Boucle financière

Cette famille opère après l'achat : déterminer ce qui doit être restitué (BF-09) et ce qui revient à l'organisateur (BF-08).

## 7.1. BF-09 — Refund Management

| Champ | Valeur |
|---|---|
| **Bounded Context source** | BC-09 — Refund Management |
| **Catégorie** | Soutien |
| **Rôle fonctionnel** | Déterminer, calculer et exécuter les remboursements |
| **Entrées** | Paiement à rembourser (BF-05) ; billets concernés (BF-06) ; éligibilité après annulation (déclenchée par le cycle d'annulation de BF-02) |
| **Sorties** | Remboursements traités (BF-08) |
| **Principes dominants** | P6 (remboursement unique), P8 (traitement progressif à l'échelle) |

**Capacités fonctionnelles :**

- Déterminer qu'un remboursement est requis (EF-080), y compris l'éligibilité après annulation (EF-076)
- Créer la demande de remboursement unique (EF-081) et calculer le montant de référence (EF-082)
- Suivre l'état d'un remboursement (EF-083), retenter après échec (EF-084)
- Garantir l'idempotence (EF-085) et le traitement progressif (EF-086)
- Ne pas rembourser automatiquement un changement d'avis (EF-087)

## 7.2. BF-08 — Financial Settlement

| Champ | Valeur |
|---|---|
| **Bounded Context source** | BC-08 — Financial Settlement |
| **Catégorie** | Soutien |
| **Rôle fonctionnel** | Clôturer financièrement les événements et tenir le solde retirable |
| **Entrées** | Achats finalisés (BF-06) ; remboursements traités (BF-09) |
| **Sorties** | Solde disponible (organisateur) |
| **Principes dominants** | P5, P6 |

**Capacités fonctionnelles :**

- Préparer la clôture financière (EF-105) et déterminer le montant net organisateur (EF-106)
- Clôturer financièrement un événement (EF-107) et rendre le solde disponible (EF-108)
- Permettre la consultation du solde (EF-109) et les demandes de retrait (EF-110) dans la limite du solde (EF-111)
- Suivre l'état des retraits (EF-112), restituer un retrait échoué au solde (EF-113), autoriser plusieurs retraits successifs (EF-114)

---

# 8. Famille 4 — Capacités transversales

Ces trois blocs servent tous les parcours sans y appartenir. Conformément à la simplification de présentation retenue en phase 06, leurs relations vers chacun des neuf autres blocs ne sont pas toutes dessinées : la table de traçabilité de `diagrammes-de-composants.md` §7 reste la référence exhaustive.

## 8.1. BF-01 — Identity & Access

| Champ | Valeur |
|---|---|
| **Bounded Context source** | BC-01 — Identity & Access Management |
| **Catégorie** | Générique |
| **Rôle fonctionnel** | Fournisseur transversal : identité des acteurs, capacités organisateur, organisations |
| **Entrées** | Mesures de sécurité (BF-10) ; inscriptions et authentifications des acteurs |
| **Sorties** | Identité des acteurs (tous les blocs) |
| **Principes dominants** | P7 (moindre privilège) |

**Capacités fonctionnelles :**

- Créer un compte participant avec un minimum d'informations (EF-001, EF-002)
- Authentifier (EF-003) et fournir les historiques (EF-004, EF-007)
- Gérer l'accès aux capacités d'organisateur (EF-005), un même compte pouvant être participant et organisateur (EF-006)
- Conduire le cycle de vérification des organisations (EF-014, EF-015)

## 8.2. BF-10 — Trust & Safety

| Champ | Valeur |
|---|---|
| **Bounded Context source** | BC-10 — Trust & Safety |
| **Catégorie** | Générique |
| **Rôle fonctionnel** | Réceptionner les signalements, analyser le risque, décider et tracer les mesures |
| **Entrées** | Signalements (participant) ; événements métier (tous les blocs) |
| **Sorties** | Mesures de sécurité (BF-01) ; décisions de sécurité (BF-02) |
| **Principes dominants** | P5 (décisions traçables), P7 |

**Capacités fonctionnelles :**

- Enregistrer les signalements d'événements (EF-088) et d'organisateurs (EF-089) avec motif (EF-090) et description (EF-091)
- Analyser un signalement (EF-092) et évaluer le niveau de risque (EF-093)
- Traçabiliser les décisions de sécurité (EF-094)
- Bannir une organisation (EF-095), bloquer son activité (EF-096), évaluer ses événements existants (EF-097) et appliquer les mesures aux événements concernés (EF-098)

## 8.3. BF-11 — Analytics & Observability

| Champ | Valeur |
|---|---|
| **Bounded Context source** | BC-11 — Analytics & Observability |
| **Catégorie** | Cœur |
| **Rôle fonctionnel** | Consommateur universel : collecter les faits métier, produire statistiques et journal |
| **Entrées** | Événements métier (tous les blocs) |
| **Sorties** | Statistiques et historique (organisateur, pilotage Eventix) |
| **Principes dominants** | P5 (traçabilité native) |

**Capacités fonctionnelles :**

- Suivre les ventes (EF-099) et les analyser par contexte (EF-100)
- Comparer les visites, commandes et montants attribués aux liens de suivi (EF-141)
- Suivre les entrées (EF-101) et les participants (EF-102)
- Consulter l'activité de l'événement (EF-103) en lecture seule stricte (EF-104)
- Conserver le journal des opérations importantes (EF-115), associées à leur contexte (EF-116), préservé après modification (EF-117)
- Distinguer les montants des dons de ceux des billets dans les rapports de vente et de suivi financier (EF-137)

---

# 9. Lecture temporelle : avant, pendant, après

La vision produit cadre Eventix **avant, pendant et après** l'événement. La décomposition s'y aligne :

| Phase | Blocs actifs | Moments clés |
|---|---|---|
| **Avant** | BF-01, BF-02, BF-03, BF-04, BF-05, BF-06, BF-12, BF-10, BF-11 (suivi des ventes) | Configuration, publication, découverte, achat, distribution |
| **Pendant** | BF-07, BF-11 (suivi des entrées), BF-12 (communications) | Contrôle d'accès, présence, mode dégradé éventuel |
| **Après** | BF-08, BF-09, BF-11 (analyse), BF-02 (archivage) | Clôture, remboursements, statistiques, historique |

Cette lecture confirme l'objectif produit n°2 (gérer avant, pendant et après) et montre qu'aucune phase du cycle ne reste sans bloc fonctionnel dédié.

---

# 10. Traçabilité blocs ↔ exigences fonctionnelles

| Bloc | Exigences couvertes | Exigences de bout en bout et transversales |
|---|---|---|
| BF-01 | EF-001 → EF-007, EF-014, EF-015 | — |
| BF-02 | EF-008 → EF-013, EF-016 → EF-024, EF-072, EF-073, EF-077 → EF-079, EF-124, EF-125, EF-130, EF-134, EF-138, EF-140, EF-142 | — |
| BF-03 | EF-025 → EF-029 | — |
| BF-04 | EF-030 → EF-034, EF-119, EF-143 | — |
| BF-05 | EF-035 → EF-041, EF-135, EF-136, EF-144 | — |
| BF-06 | EF-042 → EF-049, EF-051 → EF-055, EF-074, EF-131 | — |
| BF-07 | EF-057 → EF-071, EF-132, EF-133 | — |
| BF-08 | EF-105 → EF-114 | — |
| BF-09 | EF-076, EF-080 → EF-087 | — |
| BF-10 | EF-088 → EF-098 | — |
| BF-11 | EF-099 → EF-104, EF-115 → EF-117, EF-137, EF-141 | — |
| BF-12 | EF-050, EF-075, EF-121 → EF-123, EF-139 | — |
| **BF-13 — Supervision cybersécurité** | EF-145 → EF-147 | — |
| **Famille F1 entière** | — | EF-118 (vendre en ligne) |
| **Architecture (tous blocs)** | — | EF-126 → P2, EF-127 → P6, EF-128 → P4, EF-129 → P3 |

Les quatre exigences transversales sont portées par l'architecture elle-même et reliées aux principes correspondants :

| Exigence transversale | Principe architectural | Vérification |
|---|---|---|
| EF-126 — Préserver l'indépendance des cycles métier | P2 — Séparation stricte des cycles | Aucun bloc ne stocke l'état d'un cycle voisin |
| EF-127 — Garantir l'unicité des effets métier critiques | P6 — Idempotence | Chaque effet critique a un identifiant unique et un détecteur de doublon |
| EF-128 — Garantir une source de vérité métier cohérente | P4 — Fiabilité avant débit | Un seul arbitre par ressource critique (BF-02 pour la configuration, BF-04 pour l'attribution, BF-06 pour le billet, BF-07 pour la validation) |
| EF-129 — Ne pas exposer les responsabilités internes d'un domaine | P3 — Faible couplage | Les échanges passent par des capacités nommées, jamais par le détail interne |

**Couverture fonctionnelle : les 147 exigences sont tracées** — 140 affectées aux blocs (dont BF-13 pour EF-145 → EF-147), 1 portée par la famille F1 (EF-118), 4 portées par l'architecture (EF-126 → EF-129) et 2 traitées comme exclusions (EF-056, EF-120 — voir §11).

---

# 11. Frontières et exclusions explicites

## 11.1. Exclusions du MVP assumées par des exigences

| Exclusion | Exigence | Conséquence fonctionnelle |
|---|---|---|
| Revente de billets | EF-056 | BF-06 ne propose que le transfert contrôlé, pas de marché de revente |
| Vente en points physiques | EF-120 | Aucun bloc de vente physique ; l'inventaire central (BF-04) reste conçu pour l'accueillir ultérieurement |
| Remboursement sur changement d'avis | EF-087 | BF-09 ne traite que les obligations (annulation, réconciliation), pas la convenance |

## 11.2. Frontières internes renforcées

Les frontières suivantes sont des décisions structurantes, héritées de la phase 05 et réaffirmées ici :

- **BF-02 / BF-03** : gérer ≠ exposer. BF-03 ne possède aucune donnée d'événement.
- **BF-04 / BF-05** : réserver ≠ payer. L'expiration de la réservation est indépendante de l'état du paiement.
- **BF-05 / BF-06** : payer ≠ être titulaire. Le billet n'existe qu'à l'émission, jamais à la confirmation.
- **BF-06 / BF-07** : posséder ≠ entrer. Le statut d'utilisation relève du contrôle, pas de la possession.
- **BF-06 / BF-08** : vendre ≠ encaisser. La clôture financière consolide des achats finalisés, elle ne les crée pas.

---

# 12. Incohérence connue et arbitrages en attente

## 12.1. Flux « statut des billets utilisés » (BF-07 → BF-02)

`bounded-contexts.md` §5.4 indique que BC-07 « fournit à BC-02 » le statut des billets utilisés. Or `diagrammes-de-classes.md` §7 établit que l'état d'un billet appartient à l'agrégat Billet, dans BC-06 — pas dans BC-02. `diagrammes-de-composants.md` §8 a reproduit la relation telle quelle et demandé l'arbitrage.

**Position de ce document :** la relation est conservée dans les entrées/sorties de BF-02 et BF-07 par fidélité aux sources, mais la correction la plus probable est **BF-07 → BF-06** (le contrôle informe le billet de son utilisation). Si BF-02 a réellement besoin d'une donnée dérivée (compteur de billets utilisés par événement), l'interface devra être renommée en conséquence.

**Arbitrage attendu du product owner avant `interfaces.md`** — ce flux y sera défini comme contrat nommé.

## 12.2. Questions héritées

Les questions ouvertes de la phase 06 restent vives pour la suite de la phase 07 :

| Question | Impact sur la phase 07 |
|---|---|
| Déploiement des blocs transversaux (BF-01, BF-10, BF-11, BF-13) : service partagé unique ou répliqué | Modes de communication (`communication.md`) et flux (`flux-metier.md`) ; aucune topologie n'est présumée |

## 12.3. Supervision cybersécurité (BF-13)

La supervision est une responsabilité MVP inscrite par EF-145 à EF-147. D-ARCH-02 tranche sa frontière logique : BF-13/BC-13/MOD-13 possèdent les alertes et incidents cyber. Les signaux proviennent des modules ou sources explicitement autorisés sous forme minimisée ; une personne habilitée décide des mesures, exécutées par le propriétaire de la ressource. Aucune réponse automatique n'est déclenchée par une alerte.

Cette décision ne choisit ni outil, ni source active, ni transport, ni déploiement. Ces éléments restent à spécifier dans les phases 07 à 10.

---

# 13. Ce que la décomposition ne préjuge pas

La décomposition fonctionnelle ne préjuge pas :

- des **modules logiciels** : un bloc fonctionnel sera implémenté par un ou plusieurs modules, décision relevant de `modules.md` ;
- de la **granularité de déploiement** : monolithe modulaire, services séparés ou combinaison — décision relevant des phases 09 et 10 ;
- des **technologies**, bases de données, protocoles ou infrastructure ;
- de l'**ordre d'implémentation** des blocs, qui relève de la roadmap (phase 17).

---

# 14. Résumé

Ce document découpe Eventix en **treize blocs fonctionnels**, dont douze blocs de chaîne métier et un bloc transversal de supervision cybersécurité, alignés sur leurs bounded contexts. Les blocs métier sont regroupés en **quatre familles** : chaîne d'achat participant, cycle de vie de l'événement, boucle financière, capacités transversales. Les 147 exigences sont tracées, dont EF-145 à EF-147 affectées à BF-13. Une incohérence de flux héritée (BF-07 → BF-02) reste à arbitrer dans les contrats.

---

# 15. Critères de qualité du document

- chaque bloc possède un rôle, des capacités et des frontières explicites ;
- la correspondance BF-nn ↔ BC-nn est vérifiable ;
- chaque capacité est rattachée à au moins une exigence fonctionnelle ;
- les 147 exigences sont affectées ou classées dans la table de couverture, y compris EF-145 à EF-147 dans BF-13 ;
- les entrées/sorties sont héritées de la context map sans invention ;
- les principes architecturaux sont cités par numéro et opposables ;
- aucune décision technique n'est prise ou implicite.

---

# 16. Statut

| Champ | Valeur |
|---|---|
| **Document** | `decomposition-fonctionnelle.md` |
| **Version** | 1.0 |
| **Statut** | Proposition — à valider par l'équipe |
| **Périmètre** | MVP Eventix |
| **Marché** | Cameroun |
| **Document suivant** | `modules.md` |

| Élément | État |
|---|---|
| Blocs fonctionnels définis | ✅ 13/13 (`bounded-contexts.md`) |
| Familles fonctionnelles | ✅ 4 définies |
| Traçabilité EF | ✅ 147/147 couvertes (140 affectées aux blocs, 1 famille, 4 architecture, 2 exclusions) |
| Entrées/sorties | ✅ Héritées de la context map |
| Incohérence BF-07 → BF-02 | ⚠️ Signalée — arbitrage en attente |
| Décisions techniques | ⏳ NON PRÉJUGÉES |

---

## Notes de rédaction

### Valeur ajoutée par rapport aux sources

Ce document ne redéfinit ni les bounded contexts ni leurs relations. Il ajoute : le regroupement en familles fonctionnelles, la description par capacités tracées vers les exigences EF, la lecture temporelle avant/pendant/après alignée sur la vision produit, et la table de couverture des 147 exigences. BF-13 isole la supervision cybersécurité des fonctions de confiance métier et d'analytique.

### Sur la correspondance un-à-un

Le choix de ne pas regrouper ni scinder les blocs par rapport aux bounded contexts est délibéré : la phase 06 a déjà arbitré ce grain pour les composants UML, et rouvrir cet arbitrage sans élément nouveau ajouterait du risque sans bénéfice. La question se reposera naturellement dans `modules.md`, cette fois avec les contraintes d'implémentation en main.
