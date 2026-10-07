# Core-domaines — Eventix

| Champ | Valeur |
|---|---|
| **Phase** | 05 — Domain-Driven Design |
| **Projet** | Eventix |
| **Périmètre** | MVP |
| **Marché initial** | Cameroun |
| **Statut** | Version de référence — à valider par l'équipe |

---

## Table des matières

1. [Objectif](#1-objectif)
2. [Sources de référence](#2-sources-de-référence)
3. [Méthode de classification](#3-méthode-de-classification)
4. [Classification par domaine](#4-classification-par-domaine)
5. [Sous-domaines cœur et promesses associées](#5-sous-domaines-cœur-et-promesses-associées)
6. [Écart de couverture — pilier Interaction](#6-écart-de-couverture--pilier-interaction)
7. [Lecture transverse](#7-lecture-transverse)
8. [Statut](#8-statut)

---

# 1. Objectif

Ce document identifie, parmi les sous-domaines définis dans `sous-domaines.md`, ceux qui constituent le cœur différenciant d'Eventix au regard de la proposition de valeur établie dans `proposition-de-valeur.md`. Les autres sont classés comme sous-domaines de soutien ou génériques.

Il ne redéfinit ni les sous-domaines, ni les promesses : il établit exclusivement le lien entre les deux et hiérarchise l'attention à leur porter. La supervision cybersécurité ajoutée comme capacité MVP relève du soutien et ne constitue pas une promesse différenciante destinée aux organisateurs.

---

# 2. Sources de référence

Ce document est dérivé de :

- `05-domain-driven-design/sous-domaines.md` — découpage en sous-domaines (référence des identifiants `SD-XX-Y`)
- `02-.../proposition-de-valeur.md` — promesses et piliers de différenciation

Toute définition ou règle métier mentionnée implicitement renvoie à ces documents sources.

---

# 3. Méthode de classification

Trois catégories sont utilisées :

| Catégorie | Critère |
|---|---|
| **Cœur** | Le sous-domaine porte directement une promesse de la proposition de valeur ou l'un de ses cinq piliers de différenciation |
| **Soutien** | Indispensable au fonctionnement du cœur, mais comparable d'une plateforme à l'autre |
| **Générique** | Standard du marché, interchangeable, sans lien avec une promesse |

Un sous-domaine cœur reçoit deux attentions particulières : sa conception est protégée contre la banalisation, et son ordonnancement dans la construction du MVP est priorisé.

---

# 4. Classification par domaine

| Domaine | Sous-domaines | Catégorie | Justification |
|---|---|---|---|
| IDENTITY | SD-01-1 à SD-01-4 | Générique | Préalable à tout parcours, sans lien avec une promesse |
| CATALOG | SD-02-1, SD-02-2, SD-02-3 | **Cœur** | Portent le pilier « gestion des événements » : centralisation et contrôle promis aux organisateurs |
| CATALOG | SD-02-4 | Soutien | Conditionne la conservation et la confiance, sans être visible |
| DISCOVERY | SD-03-2, SD-03-3 | **Cœur** | Portent le pilier « découverte » : recherche par intérêts et localisation, accès simple à l'information |
| DISCOVERY | SD-03-1 | Soutien | Nécessaire mais sans valeur distinctive propre |
| BOOKING | SD-04-1 à SD-04-4 | Soutien | Conditionne l'achat fluide, sans distinguer Eventix d'un concurrent |
| PAYMENT | SD-05-1 à SD-05-3 | Générique | Le Mobile Money est le moyen imposé par le marché, non un choix distinctif |
| PAYMENT | SD-05-4 | **Cœur (local)** | La réconciliation des paiements tardifs répond à une réalité du marché camerounais ; aucune promesse explicite ne la nomme, elle est inférée du contexte |
| TICKETING | SD-06-1, SD-06-2 | **Cœur** | Portent la promesse « obtenir leurs billets simplement » |
| TICKETING | SD-06-3 | Soutien | Le transfert prolonge la gestion des accès sans la fonder |
| TICKETING | SD-06-4 | Soutien | Consultation attendue par tout utilisateur |
| ACCESS | SD-07-2 | **Cœur** | Porte la promesse de contrôle des accès pour l'organisateur |
| ACCESS | SD-07-3, SD-07-4 | **Cœur (local)** | Le fonctionnement dégradé et sa réintégration répondent aux contraintes de connectivité du marché ; inféré du contexte, non nommé dans la proposition de valeur |
| ACCESS | SD-07-1 | Soutien | Lecture technique sans valeur propre |
| FINANCE | SD-08-1 à SD-08-3 | Soutien | Standard attendu de toute billetterie, prolonge la visibilité promise sans la différencier |
| REFUND | SD-09-1 à SD-09-3 | Soutien | Garantie de confiance comparable au marché |
| TRUST & SAFETY | SD-10-1 à SD-10-3 | Générique | Attendu de toute plateforme ouverte |
| OBSERVATION | SD-11-2 | **Cœur** | Porte le pilier « analyse des événements » : la promesse « mieux comprendre » des organisateurs |
| OBSERVATION | SD-11-1, SD-11-3 | Soutien | Alimentent le cœur sans être distinctifs |
| COMMUNICATION | SD-12-1 à SD-12-3 | Générique | Canaux standards, interchangeables |
| CYBERSECURITY OPERATIONS | SD-13-1 à SD-13-3 | Soutien | Capacité interne de protection et de réponse, nécessaire au fonctionnement sûr du système mais distincte de la proposition de valeur commerciale |

---

# 5. Sous-domaines cœur et promesses associées

| Sous-domaine cœur | Pilier / promesse de la proposition de valeur |
|---|---|
| SD-02-1 | Gestion — « mieux gérer leurs événements » |
| SD-02-2 | Gestion — contrôle des capacités et des ventes |
| SD-02-3 | Gestion — centralisation de la configuration |
| SD-03-2 | Découverte — recherche selon intérêts et localisation |
| SD-03-3 | Découverte — accès facile à l'information |
| SD-05-4 | Cohérence locale — traitement réaliste des paiements (contexte camerounais) |
| SD-06-1 | Billetterie — obtention simple des billets |
| SD-06-2 | Billetterie — émission fiable du titre d'accès |
| SD-07-2 | Accès — contrôle des accès promis à l'organisateur |
| SD-07-3 | Accès — continuité de service malgré l'infrastructure locale |
| SD-07-4 | Accès — cohérence retrouvée après interruption |
| SD-11-2 | Analyse — « mieux comprendre leurs événements » |

**Bilan : douze sous-domaines cœur**, dont trois classés « cœur local » (SD-05-4, SD-07-3, SD-07-4) — différenciants par inférence du contexte camerounais et à confirmer explicitement par l'équipe, puisqu'aucun n'est nommé dans la proposition de valeur.

---

# 6. Écart de couverture — pilier Interaction

La proposition de valeur établit cinq piliers de différenciation, dont « l'interaction avec les participants » (quiz en direct, votes en temps réel, sondages) et la promesse de vivre l'événement, formulée pour les participants.

**Le découpage en sous-domaines ne couvre aucun de ces deux éléments.** Aucun des treize domaines n'est chargé de l'expérience interactive pendant l'événement ; le domaine cybersécurité ajouté est indépendant de cette promesse.

Trois interprétations sont possibles, à trancher par l'équipe :

1. **Hors MVP** : le pilier interaction relève d'une évolution postérieure ; la proposition de valeur le présente déjà comme progressif (« réunissant progressivement »). Le découpage actuel est alors complet et cet écart est assumé.
2. **Hors périmètre fonctionnel** : l'interaction est maintenue dans la promesse mais exclue du MVP sans trace dans la modélisation ; le découpage reste complet mais la proposition de valeur sur-vend le MVP.
3. **Manquement** : l'interaction appartient au MVP ; le découpage doit alors être complété par un domaine dédié avant la fin de la phase 05.

---

# 7. Lecture transverse

- **Le parcours participant** (découverte → achat → accès) est entièrement porté par des sous-domaines cœur : SD-03-2, SD-03-3, SD-06-1, SD-06-2, SD-07-2. C'est la chaîne de valeur la plus dense du MVP.
- **Le parcours organisateur** associe cœur (gestion : SD-02-1 à SD-02-3 ; analyse : SD-11-2) et soutien (finance, remboursements) : la promesse de gestion et de compréhension repose sur la qualité du cœur, pas sur l'exhaustivité des soutiens.
- **La fiabilité locale** (SD-05-4, SD-07-3, SD-07-4) forme un troisième bloc distinct : invisible dans la proposition de valeur, mais potentiellement le plus difficile à copier pour un concurrent importé.
- **Aucun sous-domaine générique ne doit recevoir d'effort de conception spécifique** : IDENTITY, TRUST & SAFETY, COMMUNICATION et l'essentiel de PAYMENT doivent rester résolument standard.

---

# 8. Statut

| Champ | Valeur |
|---|---|
| **Document** | `core-domaines.md` |
| **Version** | 1.0 |
| **Statut** | À valider par l'équipe |
| **Périmètre** | MVP Eventix |
| **Marché** | Cameroun |

| Principe | État |
|---|---|
| Classification cœur / soutien / générique | ✅ APPLIQUÉE |
| Lien explicite avec les promesses | ✅ ÉTABLI |
| Redéfinition des sous-domaines | ❌ AUCUNE — référence à la source |
| Redéfinition des promesses | ❌ AUCUNE — référence à la source |
| Écart de couverture signalé | ⚠️ PILIER INTERACTION SANS TRAITEMENT |

---

## Notes de rédaction

### Cohérence avec les sources

Ce document ne répète ni les responsabilités des sous-domaines ni le libellé des promesses : il s'y réfère par identifiants et par piliers. Toute formulation reprise mot pour mot d'une source serait un doublon à éliminer lors de la validation.

### Portée de l'inférence locale

Les trois sous-domaines classés « cœur local » le sont par inférence du marché camerounais, non par un texte explicite. C'est la seule portion du document où un jugement dépasse la lettre des sources ; il est signalé comme tel et reste soumis à validation.