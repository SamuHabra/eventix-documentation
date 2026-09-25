# Context Map — Eventix

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
2. [Source de référence](#2-source-de-référence)
3. [Méthode de cartographie](#3-méthode-de-cartographie)
4. [Types de relations utilisés](#4-types-de-relations-utilisés)
5. [Carte globale des contextes](#5-carte-globale-des-contextes)
6. [Relations détaillées par contexte](#6-relations-détaillées-par-contexte)
7. [Flux métier transversaux](#7-flux-métier-transversaux)
8. [Points de friction identifiés](#8-points-de-friction-identifiés)
9. [Alignement avec le core domain](#9-alignement-avec-le-core-domain)
10. [Ce que la context map ne préjuge pas](#10-ce-que-la-context-map-ne-préjuge-pas)
11. [Résumé](#11-résumé)
12. [Critères de qualité du document](#12-critères-de-qualité-du-document)
13. [Statut](#13-statut)

---

# 1. Objectif

Ce document établit la carte des relations entre les douze bounded contexts définis dans `bounded-contexts.md`. Pour chaque relation, il précise :

- le type de relation (partenariat, client-fournisseur, conformiste, etc.) ;
- la direction de la dépendance ;
- la nature des échanges (événements, commandes, requêtes, données) ;
- les contraintes de cohérence associées.

Il ne redéfinit ni les bounded contexts, ni les sous-domaines, ni les règles métier déjà établies dans la source. Ces éléments sont utilisés par référence et supposés connus du lecteur.

---

# 2. Source de référence

Ce document est dérivé exclusivement de :

- `05-domain-driven-design/bounded-contexts.md`

Toute définition, frontière ou classification mentionnée implicitement renvoie à ce document source.

---

# 3. Méthode de cartographie

La cartographie suit la méthode de Eric Evans et suit les principes suivants :

- **Chaque relation est dirigée** : elle part d'un contexte source vers un contexte cible.
- **Chaque relation est typée** : elle appartient à une catégorie reconnue du DDD.
- **Chaque relation est justifiée** : elle correspond à un besoin métier explicite.
- **Chaque relation est minimale** : elle ne transporte que ce qui est nécessaire.

La carte ne représente pas les flux techniques (API, messages, fichiers) mais les dépendances logiques entre contextes.

---

# 4. Types de relations utilisés

| Type | Symbole | Définition | Usage dans Eventix |
|---|---|---|---|
| **Partenariat** | `⇄` | Deux contextes collaborent étroitement, avec des cycles de vie liés | Relations entre contextes cœur du même parcours |
| **Client-Fournisseur** | `→` | Le contexte client dépend du contexte fournisseur, qui définit le contrat | Relations entre contextes de niveaux de différenciation différents |
| **Conformiste** | `⇒` | Le contexte client adopte le modèle du fournisseur sans négociation | Relations avec des contextes génériques standardisés |
| **Anti-corruption Layer** | `⇢` | Le contexte client traduit le modèle du fournisseur pour protéger son propre modèle | Relations où les modèles divergent significativement |
| **Événement Publié** | `↠` | Le contexte source publie des événements métier consommés par le contexte cible | Communication asynchrone entre contextes |
| **Service Hébergé** | `⊃` | Le contexte source héberge ou expose une capacité utilisée par le contexte cible | Exposition de fonctionnalités transversales |

---

# 5. Carte globale des contextes

```text
                              ┌─────────────┐
                              │    BC-01    │
                              │  IDENTITY   │
                              │  (Générique)│
                              └──────┬──────┘
                                     │
                                     │ Service Hébergé
                                     │ (identité des acteurs)
                                     ▼
┌─────────────┐              ┌─────────────┐              ┌─────────────┐
│    BC-10    │              │    BC-02    │              │    BC-03    │
│  TRUST &    │              │   CATALOG   │              │  DISCOVERY  │
│  SAFETY     │              │   (Cœur)    │              │   (Cœur)    │
│ (Générique) │              └──────┬──────┘              └──────┬──────┘
└──────┬──────┘                     │                            │
       │                            │                            │
       │ Client-Fournisseur         │ Client-Fournisseur         │ Client-Fournisseur
       │ (décisions de sécurité)    │ (événements publiés)       │ (catalogue exposé)
       │                            │                            │
       │                            ▼                            ▼
       │                     ┌─────────────┐              ┌─────────────┐
       │                     │    BC-04    │              │  PARTICIPANT│
       │                     │  BOOKING    │              │  (hors BC)  │
       │                     │  (Soutien)  │              └─────────────┘
       │                     └──────┬──────┘
       │                            │
       │                            │ Client-Fournisseur
       │                            │ (réservation valide)
       │                            ▼
       │                     ┌─────────────┐
       │                     │    BC-05    │
       │                     │  PAYMENT    │
       │                     │ (Générique) │
       │                     └──────┬──────┘
       │                            │
       │                            │ Événement Publié
       │                            │ (paiement confirmé)
       │                            ▼
       │                     ┌─────────────┐              ┌─────────────┐
       │                     │    BC-06    │              │    BC-09    │
       │                     │  TICKETING  │              │   REFUND    │
       │                     │   (Cœur)    │              │  (Soutien)  │
       │                     └──────┬──────┘              └──────┬──────┘
       │                            │                            │
       │                            │ Client-Fournisseur         │ Client-Fournisseur
       │                            │ (billets émis)             │ (remboursements)
       │                            ▼                            ▼
       │                     ┌─────────────┐              ┌─────────────┐
       │                     │    BC-07    │              │    BC-08    │
       │                     │   ACCESS    │              │  FINANCE    │
       │                     │   (Cœur)    │              │  (Soutien)  │
       │                     └──────┬──────┘              └──────┬──────┘
       │                            │                            │
       │                            │ Événement Publié           │ Service Hébergé
       │                            │ (présence enregistrée)     │ (solde disponible)
       │                            ▼                            ▼
       │                     ┌─────────────┐              ┌─────────────┐
       │                     │    BC-11    │              │ ORGANISATEUR│
       │                     │  ANALYTICS  │              │  (hors BC)  │
       │                     │   (Cœur)    │              └─────────────┘
       │                     └─────────────┘
       │
       │                     ┌─────────────┐
       └────────────────────►│    BC-12    │
                             │COMMUNICATION│
                             │ (Générique) │
                             └─────────────┘

6. Relations détaillées par contexte
6.1. BC-01 — Identity & Access Management
Table
Relation	Type	Direction	Nature de l'échange	Justification
BC-01 → tous les BC	Service Hébergé	Sortante	Identité des acteurs (utilisateur, organisateur, organisation)	Tout contexte a besoin de connaître l'identité de ses acteurs
BC-10 → BC-01	Client-Fournisseur	Entrante	Mesures de sécurité (suspension, bannissement)	Les décisions de TRUST & SAFETY s'appliquent aux comptes et organisations
Rôle dans la carte : BC-01 est un fournisseur transversal. Il ne dépend d'aucun autre contexte dans le cycle principal et fournit une capacité indispensable à tous.
6.2. BC-02 — Event Catalog
Table
Relation	Type	Direction	Nature de l'échange	Justification
BC-01 → BC-02	Client-Fournisseur	Entrante	Organisateur autorisé	La création d'événement nécessite un compte organisateur
BC-10 → BC-02	Client-Fournisseur	Entrante	Décisions de sécurité (maintien, suspension, annulation)	Les mesures de TRUST & SAFETY portent sur les événements
BC-02 → BC-03	Client-Fournisseur	Sortante	Événements publiés et disponibilités	DISCOVERY expose ce que CATALOG publie
BC-02 → BC-04	Client-Fournisseur	Sortante	Catégories de billets et disponibilités	BOOKING réserve sur la base de la configuration CATALOG
BC-02 → BC-06	Client-Fournisseur	Sortante	Événement et catégorie pour émission	TICKETING émet des billets pour des événements CATALOG
BC-02 → BC-07	Client-Fournisseur	Sortante	Événement et points d'entrée	ACCESS contrôle l'entrée d'événements CATALOG
BC-02 → BC-12	Événement Publié	Sortante	Événements annulés ou reportés	COMMUNICATION informe les participants des changements
Rôle dans la carte : BC-02 est le référentiel central. Il fournit la vérité métier sur les événements à la plupart des autres contextes.
6.3. BC-03 — Event Discovery
Table
Relation	Type	Direction	Nature de l'échange	Justification
BC-02 → BC-03	Client-Fournisseur	Entrante	Événements publiés et disponibilités	DISCOVERY expose ce que CATALOG publie
BC-03 → PARTICIPANT	Service Hébergé	Sortante	Parcours de découverte	Le participant consulte le catalogue sans compte
Rôle dans la carte : BC-03 est la porte d'entrée du parcours participant. Il ne génère pas de données propres mais transforme et expose celles de BC-02.
6.4. BC-04 — Booking & Availability
Table
Relation	Type	Direction	Nature de l'échange	Justification
BC-03 → BC-04	Client-Fournisseur	Entrante	Demande de réservation	Le participant initie une réservation depuis la découverte
BC-02 → BC-04	Client-Fournisseur	Entrante	Catégories et disponibilités	BOOKING réserve sur la base de la configuration CATALOG
BC-04 → BC-05	Client-Fournisseur	Sortante	Réservation valide à payer	PAYMENT démarre sur la base d'une réservation BOOKING
Rôle dans la carte : BC-04 est un pont entre la découverte et le paiement. Il garantit l'unicité d'attribution pendant le blocage temporaire.
6.5. BC-05 — Payment Processing
Table
Relation	Type	Direction	Nature de l'échange	Justification
BC-04 → BC-05	Client-Fournisseur	Entrante	Réservation valide	PAYMENT démarre sur la base d'une réservation BOOKING
BC-05 → BC-06	Événement Publié	Sortante	Paiement confirmé	TICKETING émet le billet sur confirmation du paiement
BC-05 → BC-09	Événement Publié	Sortante	Paiement à rembourser	REFUND traite les paiements à rembourser (réconciliation tardive)
Rôle dans la carte : BC-05 est un contexte générique qui traite les opérations financières. Il publie des événements plutôt que de fournir des requêtes synchrones.
6.6. BC-06 — Ticketing & Fulfillment
Table
Relation	Type	Direction	Nature de l'échange	Justification
BC-05 → BC-06	Événement Publié	Entrante	Paiement confirmé	TICKETING émet le billet sur confirmation du paiement
BC-02 → BC-06	Client-Fournisseur	Entrante	Événement et catégorie	TICKETING émet des billets pour des événements CATALOG
BC-06 → BC-07	Client-Fournisseur	Sortante	Billets émis	ACCESS contrôle les billets émis par TICKETING
BC-06 → BC-12	Client-Fournisseur	Sortante	Billets à distribuer	COMMUNICATION distribue les billets émis
BC-06 → PARTICIPANT	Service Hébergé	Sortante	Billets consultables	Le participant consulte ses billets depuis son compte
Rôle dans la carte : BC-06 est le cœur de la promesse billetterie. Il consolide les confirmations de paiement et les données d'événement pour émettre les billets.
6.7. BC-07 — Access Control
Table
Relation	Type	Direction	Nature de l'échange	Justification
BC-06 → BC-07	Client-Fournisseur	Entrante	Billets émis	ACCESS contrôle les billets émis par TICKETING
BC-02 → BC-07	Client-Fournisseur	Entrante	Événement et points d'entrée	ACCESS contrôle l'entrée d'événements CATALOG
BC-07 → BC-11	Événement Publié	Sortante	Présence enregistrée	ANALYTICS agrège les données de présence
BC-07 → BC-02	Événement Publié	Sortante	Statut des billets utilisés	CATALOG connaît l'état d'utilisation de ses billets
Rôle dans la carte : BC-07 est le cœur de la promesse de contrôle d'accès. Il opère dans un environnement contraint (connectivité variable) et publie des événements de présence.
6.8. BC-08 — Financial Settlement
Table
Relation	Type	Direction	Nature de l'échange	Justification
BC-06 → BC-08	Client-Fournisseur	Entrante	Achats finalisés	FINANCE clôture sur la base des achats TICKETING
BC-09 → BC-08	Client-Fournisseur	Entrante	Remboursements traités	FINANCE clôture sur la base des remboursements REFUND
BC-08 → ORGANISATEUR	Service Hébergé	Sortante	Solde disponible	L'organisateur consulte et retire son solde
Rôle dans la carte : BC-08 est un contexte de soutien qui consolide les flux financiers. Il ne génère pas de données propres mais agrège celles de TICKETING et REFUND.
6.9. BC-09 — Refund Management
Table
Relation	Type	Direction	Nature de l'échange	Justification
BC-05 → BC-09	Événement Publié	Entrante	Paiement à rembourser	REFUND traite les paiements à rembourser (réconciliation tardive)
BC-06 → BC-09	Client-Fournisseur	Entrante	Billets concernés	REFUND identifie les billets à rembourser
BC-09 → BC-08	Client-Fournisseur	Sortante	Remboursements traités	FINANCE clôture sur la base des remboursements REFUND
Rôle dans la carte : BC-09 est un contexte de soutien qui traite les obligations de remboursement. Il reçoit des déclenchements de PAYMENT et fournit des résultats à FINANCE.
6.10. BC-10 — Trust & Safety
Table
Relation	Type	Direction	Nature de l'échange	Justification
PARTICIPANT → BC-10	Événement Publié	Entrante	Signalements	TRUST & SAFETY reçoit les alertes des participants
BC-10 → BC-01	Client-Fournisseur	Sortante	Mesures de sécurité	Les décisions s'appliquent aux comptes et organisations
BC-10 → BC-02	Client-Fournisseur	Sortante	Mesures de sécurité	Les décisions s'appliquent aux événements
Rôle dans la carte : BC-10 est un contexte générique transversal. Il reçoit des signaux de tous les contextes et émet des décisions qui s'appliquent à IDENTITY et CATALOG.
6.11. BC-11 — Analytics & Observability
Table
Relation	Type	Direction	Nature de l'échange	Justification
Tous les BC → BC-11	Événement Publié	Entrante	Événements métier	ANALYTICS agrège les faits marquants de tous les contextes
BC-11 → ORGANISATEUR	Service Hébergé	Sortante	Statistiques et historique	L'organisateur consulte ses données d'activité
Rôle dans la carte : BC-11 est un contexte cœur transversal. Il consomme les événements de tous les contextes et produit des agrégats en lecture seule.
6.12. BC-12 — Communication
Table
Relation	Type	Direction	Nature de l'échange	Justification
BC-06 → BC-12	Client-Fournisseur	Entrante	Billets à distribuer	COMMUNICATION distribue les billets émis
BC-02 → BC-12	Événement Publié	Entrante	Événements annulés ou reportés	COMMUNICATION informe les participants des changements
BC-12 → PARTICIPANT	Service Hébergé	Sortante	Communications	Le participant reçoit ses billets et notifications
Rôle dans la carte : BC-12 est un contexte générique de transmission. Il ne génère pas de données propres mais achemine celles de TICKETING et CATALOG vers les participants.
7. Flux métier transversaux
7.1. Parcours participant complet
Text
BC-03 (découverte)
    ↓
BC-04 (réservation)
    ↓
BC-05 (paiement)
    ↓
BC-06 (billet émis)
    ↓
BC-12 (distribution)
    ↓
BC-07 (contrôle à l'entrée)
Ce parcours traverse six bounded contexts. Les relations sont principalement de type Client-Fournisseur avec des événements publiés aux transitions critiques (paiement confirmé, billet émis).
7.2. Parcours organisateur complet
Text
BC-01 (compte organisateur)
    ↓
BC-02 (création et publication)
    ↓
BC-11 (statistiques pendant la vente)
    ↓
BC-07 (contrôle pendant l'événement)
    ↓
BC-08 (clôture et solde)
    ↓
BC-08 (retrait)
Ce parcours traverse cinq bounded contexts. Les relations sont principalement de type Client-Fournisseur avec BC-02 comme référentiel central.
7.3. Flux de sécurité
Text
PARTICIPANT → BC-10 (signalement)
    ↓
BC-10 → BC-01 (mesure sur compte/organisation)
BC-10 → BC-02 (mesure sur événement)
Ce flux transversal intervient à tout moment et peut interrompre les parcours principaux.
7.4. Flux de réconciliation
Text
BC-04 (réservation expirée)
    ↓
BC-05 (paiement tardif confirmé)
    ↓
BC-05 → BC-09 (déclenchement remboursement)
    ↓
BC-09 → BC-08 (remboursement traité)
Ce flux traverse quatre bounded contexts et illustre la gestion des cas particuliers métier.
8. Points de friction identifiés
8.1. Friction 1 : Cohérence entre BC-02 et BC-07
Nature : BC-07 modifie l'état des billets (USED) qui appartiennent à BC-06, tout en dépendant de BC-02 pour les points d'entrée.
Risque : Divergence entre l'état du billet dans BC-06 et son statut d'utilisation dans BC-07.
Mitigation : BC-07 publie des événements de présence que BC-06 consomme pour mettre à jour l'état de ses billets. La source de vérité reste BC-07 pour la validation, BC-06 pour l'émission.
8.2. Friction 2 : Cohérence entre BC-04 et BC-05
Nature : BC-04 libère les disponibilités après expiration, mais BC-05 peut confirmer un paiement tardif.
Risque : Attribution simultanée de la même disponibilité à deux achats différents.
Mitigation : La réconciliation dans BC-05 détermine l'issue (billet ou remboursement) sans jamais ignorer le paiement tardif. L'arbitre unique dans BC-04 garantit l'unicité d'attribution.
8.3. Friction 3 : Dépendance circulaire BC-02 ↔ BC-10
Nature : BC-02 fournit des événements à BC-10 pour l'analyse de risque, et BC-10 fournit des décisions de sécurité à BC-02.
Risque : Cycle de dépendances difficile à gérer.
Mitigation : La relation est décomposée en deux flux unidirectionnels : BC-02 publie des événements métier (asynchrone), BC-10 émet des décisions (asynchrone). Il n'y a pas d'appel synchrone circulaire.
8.4. Friction 4 : Volume d'événements vers BC-11
Nature : BC-11 consomme les événements de tous les contextes.
Risque : Surcharge du contexte d'analyse en cas de pic d'activité.
Mitigation : La consommation est asynchrone par nature. Le traitement progressif des remboursements dans BC-09 illustre le principe de résilience aux pics.
9. Alignement avec le core domain
9.1. Densité des relations par catégorie
Table
Catégorie	Nombre de BC	Nombre de relations sortantes	Nombre de relations entrantes
Cœur	5	12	8
Soutien	3	6	6
Générique	4	10	14
9.2. Contextes cœur et leurs relations
Table
Contexte cœur	Relations principales	Partenaires clés
BC-02	7 relations sortantes	BC-03, BC-04, BC-06, BC-07, BC-12
BC-03	2 relations	BC-02, PARTICIPANT
BC-06	5 relations	BC-05, BC-02, BC-07, BC-12, PARTICIPANT
BC-07	4 relations	BC-06, BC-02, BC-11
BC-11	2 relations	Tous les BC, ORGANISATEUR
9.3. Observations
BC-02 est le hub central : il fournit à quatre contextes cœur et deux contextes de soutien.
BC-11 est le consommateur universel : il reçoit de tous les contextes sans fournir de données métier en retour.
BC-05 et BC-09 forment un sous-système : la réconciliation des paiements tardifs crée une relation directe entre eux.
BC-07 et BC-11 forment un sous-système : la présence enregistrée alimente directement l'analyse.
10. Ce que la context map ne préjuge pas
La cartographie des contextes ne préjuge pas :
de l'architecture technique (monolithe, microservices, etc.) ;
des technologies de développement ;
des protocoles de communication (REST, messaging, etc.) ;
des bases de données ;
des frameworks ;
des mécanismes de déploiement ;
de l'ordre d'implémentation des contextes.
Une relation Client-Fournisseur peut être implémentée par un appel de fonction, une API REST, un message asynchrone ou une base de données partagée. Cette décision relève des phases ultérieures.
11. Résumé
Ce document établit la carte des relations entre les douze bounded contexts d'Eventix. Il identifie quatre types de relations (Client-Fournisseur, Événement Publié, Service Hébergé, Partenariat) et décrit les échanges entre chaque paire de contextes connectés. Quatre points de friction sont identifiés avec leurs mitigations. La carte confirme le rôle central de BC-02 (Event Catalog) et la densité des relations entre les contextes cœur du parcours participant.
12. Critères de qualité du document
Ce document doit respecter les propriétés suivantes :
chaque relation est dirigée et typée ;
chaque relation est justifiée par un besoin métier ;
la carte globale est lisible et vérifiable ;
les points de friction sont identifiés avec leurs mitigations ;
l'alignement avec la classification cœur / soutien / générique est vérifiable ;
aucune décision technique n'est prise ou implicite.
13. Statut
Table
Champ	Valeur
Document	context-map.md
Version	1.0
Statut	À valider par l'équipe
Périmètre	MVP Eventix
Marché	Cameroun
Table
Principe	État
Relations dirigées et typées	✅ APPLIQUÉ
Justification métier par relation	✅ APPLIQUÉ
Carte globale lisible	✅ ÉTABLIE
Points de friction identifiés	✅ SIGNALÉS
Alignement avec le core domain	✅ VÉRIFIÉ
Décisions techniques	⏳ NON PRÉJUGÉES
