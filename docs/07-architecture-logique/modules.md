# Modules — Eventix

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
3. [Ce qu'est un module Eventix](#3-ce-quest-un-module-eventix)
4. [Décisions structurantes](#4-décisions-structurantes)
5. [Structure interne canonique d'un module](#5-structure-interne-canonique-dun-module)
6. [Cartographie des douze modules](#6-cartographie-des-douze-modules)
7. [Règles structurelles entre modules](#7-règles-structurelles-entre-modules)
8. [Clients et systèmes externes](#8-clients-et-systèmes-externes)
9. [Incohérence connue et arbitrages en attente](#9-incohérence-connue-et-arbitrages-en-attente)
10. [Cas d'évolution](#10-cas-dévolution)
11. [Ce que les modules ne préjugent pas](#11-ce-que-les-modules-ne-préjugent-pas)
12. [Résumé](#12-résumé)
13. [Critères de qualité du document](#13-critères-de-qualité-du-document)
14. [Statut](#14-statut)

---

# 1. Objectif

Ce document concrétise la décomposition fonctionnelle en **modules logiciels** : des unités de code aux frontières vérifiables, qui détiennent les agrégats du domaine et s'échangent des capacités par des interfaces nommées.

Il répond à la question :

> **Comment la base de code d'Eventix est-elle organisée en unités cohérentes, et quelles règles gouvernent leurs relations ?**

Il précise la granularité des modules (un module par bloc fonctionnel), la posture de déploiement recommandée au MVP, la structure interne canonique de chaque module et les règles structurelles opposables entre modules.

Il ne définit **pas encore** :

- les responsabilités détaillées de chaque module (→ `responsabilites.md`) ;
- les contrats d'interface précis (→ `interfaces.md`) ;
- les modes de communication sync/async (→ `communication.md`) ;
- les langages, frameworks, bases de données et topologie de déploiement (→ phases 08 à 10).

---

# 2. Sources de référence

Ce document est dérivé de :

- `07-architecture-logique/decomposition-fonctionnelle.md` — les douze blocs fonctionnels BF-01 à BF-12 et leurs capacités ;
- `07-architecture-logique/principes-architecturaux.md` — les principes P1 à P10 et les anti-principes ;
- `06-modelisation-uml/diagrammes-de-composants.md` — le grain un composant = un bounded context, les agrégats regroupés par composant et la table de traçabilité des interfaces nommées ;
- `05-domain-driven-design/agregats.md` et `05-domain-driven-design/services-de-domaine.md` — le contenu de domaine que les modules détiennent ;
- `06-modelisation-uml/diagrammes-de-deploiement.md` — la posture client du MVP (web only, scan QR dédié) et les systèmes externes.

---

# 3. Ce qu'est un module Eventix

Un **module** est une unité logicielle qui :

- **détient** un ensemble d'agrégats, d'objets de valeur et de règles de domaine qui lui appartiennent en propre ;
- **expose** des capacités par des interfaces nommées, une capacité par interface ;
- **consomme** les interfaces nommées d'autres modules sans jamais accéder à leurs détails internes ;
- **publie** des événements métier consommés par d'autres modules (MOD-11 notamment).

Le module est la **frontière de propriété** : rien de ce qu'il détient n'est modifié par un autre module. C'est aussi la **frontière de remplacement** : tant que ses interfaces sont respectées, son implémentation interne peut changer sans impact sur le reste du système (P3, P8).

## 3.1. Convention d'identification

| Identifiant | Alignement |
|---|---|
| `MOD-01` … `MOD-12` | un module par bloc fonctionnel `BF-nn`, lui-même aligné sur le bounded context `BC-nn` |

La chaîne de traçabilité complète est :

```text
Sous-domaine (SD-nn-m, phase 05)
    → Bounded Context (BC-nn, phase 05)
        → Composant UML (phase 06)
            → Bloc fonctionnel (BF-nn, phase 07)
                → Module (MOD-nn, ce document)
```

Chaque niveau reprend le même découpage sans le réinterpréter — une exigence métier (EF-nnn) remonte ainsi jusqu'à l'unité de code qui la satisfait.

---

# 4. Décisions structurantes

## 4.1. Granularité : un module par bloc fonctionnel

Au MVP, **chaque bloc fonctionnel est implémenté par exactement un module** (12 modules). Cette décision prolonge le grain déjà arbitré en phase 06 (un composant par bounded context) et le confirme au niveau du code.

**Justification :**

- P3 (faible couplage) : les frontières modulaires suivent les frontières de langage ubiquitaire déjà validées ;
- P9 (simplicité opérationnelle) : douze modules est le maximum que l'équipe peut raisonnablement détenir au MVP ; davantage fragmenterait artificiellement ;
- Anti-principe « architecture distribuée prématurée » : aucun élément nouveau ne justifie de scinder ou de regrouper les blocs à ce stade.

**Porte de sortie :** un bloc dont la charge ou le cycle de vie divergerait pourra être scindé ultérieurement (voir §10) — les règles du §7 garantissent qu'une scission n'entraîne pas de refonte globale.

## 4.2. Posture de déploiement au MVP : monolithe modulaire

**Recommandation : les douze modules forment au MVP une base de code unique déployée ensemble (monolithe modulaire), les frontières modulaires étant conçues comme des candidats à l'extraction.**

Cette posture découle directement des principes validés :

| Principe | Implication |
|---|---|
| P9 — Simplicité opérationnelle | Une équipe réduite, un déploiement, moins de composants mobiles |
| Anti-principe — Architecture distribuée prématurée | Douze services d'emblée serait de la sur-ingénierie pour le MVP |
| P4 — Fiabilité avant débit | Un déploiement unique simplifie la cohérence des opérations critiques |
| P8 — Évolutivité contrôlée | Les frontières modulaires strictes rendent l'extraction ultérieure possible sans réécriture |

**Ce que cette recommandation ne fige pas :** la topologie physique (conteneurs, instances, réplication) relève des phases 09 et 10 ; la décision finale d'extraction d'un module sera arbitrée sur critères (voir §10.3).

## 4.3. Clients et systèmes externes : hors modules

Les applications clientes et les systèmes externes **ne sont pas des modules** du système Eventix :

| Élément | Nature | Relation aux modules |
|---|---|---|
| Navigateur web participant | Client (MVP web only) | Consomme les interfaces d'exposition des modules |
| Navigateur web organisateur | Client (MVP web only) | Consomme les interfaces d'exposition des modules |
| Application de scan QR (agent de contrôle) | Client dédié | Consomme principalement MOD-07 |
| Passerelle Mobile Money | Système externe | Dialogue avec MOD-05 via sa couche d'adaptation |
| Passerelle de notification (email) | Système externe | Dialogue avec MOD-12 via sa couche d'adaptation |

Leur architecture propre relève des phases ultérieures ; ce document ne retient d'eux que ce qui contraint les modules : le canal scan QR est intermittent (P10), les passerelles externes sont interchangeables derrière un adaptateur (P8).

---

# 5. Structure interne canonique d'un module

Chaque module suit la même organisation interne en quatre parties, quelle que soit sa catégorie :

```text
┌──────────────────────────── MOD-nn ────────────────────────────┐
│                                                                │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  NOYAU DE DOMAINE                                         │  │
│  │  Agrégats · Objets valeur · Services de domaine          │  │
│  │  Événements de domaine · Règles                          │  │
│  │  → ne référence aucun autre module                        │  │
│  └──────────────────────────────────────────────────────────┘  │
│                          ▲                                     │
│  ┌───────────────────────┴──────────────────────────────────┐  │
│  │  APPLICATION                                             │  │
│  │  Cas d'usage · Orchestration · Idempotence · Transactions│  │
│  └──────────────────────────────────────────────────────────┘  │
│        ▲                                    │                  │
│  ┌─────┴──────────────┐        ┌────────────▼────────────────┐  │
│  │  EXPOSITION        │        │  ADAPTATION                 │  │
│  │  Interfaces fournies│        │  Interfaces consommées      │  │
│  │  aux autres modules │        │  Systèmes externes          │  │
│  │  et aux clients     │        │  (Mobile Money, email…)     │  │
│  └─────────────────────┘        └────────────────────────────┘  │
└────────────────────────────────────────────────────────────────┘
```

| Partie | Rôle | Contrainte |
|---|---|---|
| **Noyau de domaine** | Le cœur métier : agrégats, règles, événements | Pur — aucune référence à un autre module ni à une technologie |
| **Application** | Cas d'usage orchestrant les agrégats, effets idempotents | Ne contient aucune règle métier |
| **Exposition** | Interfaces nommées fournies aux autres modules et aux clients | Une capacité par interface, jamais de détail interne |
| **Adaptation** | Consommation des interfaces des autres modules, dialogue avec les systèmes externes | Traduit les modèles externes (anticorruption) sans les laisser entrer dans le noyau |

Cette structure est **logique, pas technologique** : elle n'impose ni framework, ni format d'échange, ni base de données — uniquement la discipline de séparation. C'est elle qui rend chaque module extractible (P8) : ses effets externes passent tous par des frontières identifiées.

---

# 6. Cartographie des douze modules

| Module | Nom | Famille | Agrégats et entités détenus | Interfaces fournies | Interfaces consommées |
|---|---|---|---|---|---|
| `MOD-01` | Identity & Access | F4 | Utilisateur, Compte participant, Compte organisateur, Organisation | OrganisateurAutorisé, IdentitéDesActeurs (transversal) | MesuresDeSécurité |
| `MOD-02` | Event Catalog | F2 | Événement, Espace, Zone, Catégorie de billet, Historique de configuration | ÉvénementsPubliés, CatégoriesEtDisponibilités, ÉvénementPourÉmission, ÉvénementEtPointsDEntrée, ÉvénementsAnnulésOuReportés | OrganisateurAutorisé, DécisionsDeSécurité, StatutBilletUtilisé (⚠ §9) |
| `MOD-03` | Event Discovery | F1 | Catalogue (vue), Recherche | DemandeDeRéservation, parcours de découverte (client) | ÉvénementsPubliés |
| `MOD-04` | Booking & Availability | F1 | Réservation, Disponibilité | RéservationValide | DemandeDeRéservation, CatégoriesEtDisponibilités |
| `MOD-05` | Payment Processing | F1 | Paiement, Réconciliation | PaiementConfirmé, PaiementÀRembourser | RéservationValide + passerelle Mobile Money (adaptation) |
| `MOD-06` | Ticketing & Fulfillment | F1 | Achat, Billet, Transfert | BilletsÀContrôler, BilletsÀDistribuer, BilletsConcernés, AchatsFinalisés, billets consultables (client) | PaiementConfirmé, ÉvénementPourÉmission |
| `MOD-07` | Access Control | F2 | Point d'entrée, Contrôle, Présence | StatutBilletUtilisé (⚠ §9), ÉvénementsDePrésence | BilletsÀContrôler, ÉvénementEtPointsDEntrée + application de scan (client) |
| `MOD-08` | Financial Settlement | F3 | Clôture, Solde organisateur, Retrait | SoldeDisponible (client organisateur) | AchatsFinalisés, RemboursementsTraités |
| `MOD-09` | Refund Management | F3 | Obligation de remboursement, Remboursement | RemboursementsTraités | PaiementÀRembourser, BilletsConcernés |
| `MOD-10` | Trust & Safety | F4 | Signalement, Mesure de sécurité | MesuresDeSécurité, DécisionsDeSécurité | Signalements (client participant), ÉlémentsDAnalyse (tous) |
| `MOD-11` | Analytics & Observability | F4 | Événement métier, Statistique, Historique | StatistiquesEtHistorique (client organisateur) | ÉvénementsMétier (tous), ÉvénementsDePrésence |
| `MOD-12` | Communication | F1 | Distribution, Notification | Communications (client participant) | BilletsÀDistribuer, ÉvénementsAnnulésOuReportés + passerelle email (adaptation) |

**Lecture :**

- les agrégats détenus reprennent exactement la table §3 de `diagrammes-de-composants.md` — la couverture du modèle de classes est exhaustive et sans chevauchement ;
- les interfaces reprennent exactement la table de traçabilité §7 du même document — chaque interface nommée a une ligne source vérifiable ;
- les interfaces transversales (`IdentitéDesActeurs`, `ÉvénementsMétier`, `ÉlémentsDAnalyse`) sont notées une fois avec leur portée, conformément à la simplification de présentation assumée en phase 06 ;
- les capacités fonctionnelles de chaque module (EF couvertes) sont détaillées dans `decomposition-fonctionnelle.md` §5 à §8 et ne sont pas répétées ici.

---

# 7. Règles structurelles entre modules

Ces règles sont **normatives** : toute exception doit être explicitement justifiée et documentée dans `decisions-architecturales.md`.

| # | Règle | Fondement |
|---|---|---|
| **R1** | Chaque agrégat, entité et objet de valeur appartient à exactement un module | Principe D2 (phase 05) ; P3 |
| **R2** | Tout échange inter-modules passe par une interface nommée — jamais d'accès direct aux données ou aux types internes d'un autre module | P3 ; EF-129 |
| **R3** | Aucune dépendance circulaire entre modules ; les relations bidirectionnelles attendues sont décomposées en flux unidirectionnels | P3 ; mitigation friction 3 (BC-02 ↔ BC-10) |
| **R4** | Le noyau de domaine d'un module ne référence aucun autre module ; les dépendances inter-modules vivent dans les couches application et adaptation | P3 ; structure canonique §5 |
| **R5** | Un module ne modifie jamais une donnée qu'il ne détient pas ; il en consomme une vue via interface ou événement | P2 ; EF-128 |
| **R6** | Les opérations critiques exposées par un module sont idempotentes : une répétition ne produit pas d'effet supplémentaire | P6 ; EF-127 |

De ces règles découlent deux vérifications applicables à la base de code (leur outillage relève de la phase 09) :

- **V1** — le graphe des dépendances inter-modules est acyclique (R3) ;
- **V2** — aucun type du noyau de domaine d'un module n'apparaît dans un autre module (R1, R4).

---

# 8. Clients et systèmes externes

Le MVP est **web only** pour le participant et l'organisateur (navigateurs), avec une seule application cliente dédiée : le scan QR de l'agent de contrôle (`diagrammes-de-deploiement.md`). Deux conséquences structurelles :

- **MOD-07 est le seul module directement contraint par un client à connectivité intermittente** : son exposition doit fonctionner en mode dégradé local puis se réintégrer (P4, P10) — une contrainte qui pesera sur `interfaces.md` et `communication.md` ;
- **MOD-05 et MOD-12 sont les deux points de contact avec des systèmes externes** (Mobile Money, email) : leur couche d'adaptation est le seul endroit du système où le modèle d'un tiers peut entrer — sous forme traduite, jamais dans les noyaux de domaine (P8).

---

# 9. Incohérence connue et arbitrages en attente

## 9.1. Interface `StatutBilletUtilisé` (MOD-07 → MOD-02)

Héritée de `bounded-contexts.md` §5.4 et déjà signalée en phase 06 (§8) puis dans `decomposition-fonctionnelle.md` (§12.1) : l'état d'un billet appartient à l'agrégat Billet détenu par MOD-06, de sorte que la cible logique de cette interface serait MOD-06 plutôt que MOD-02.

**Position de ce document :** l'interface est conservée telle quelle par fidélité aux sources, marquée ⚠ dans la cartographie §6. **L'arbitrage du product owner est attendu avant `interfaces.md`**, où cette interface deviendra un contrat formellement défini.

## 9.2. Modules transversaux (MOD-01, MOD-10, MOD-11)

Question héritée de la phase 06 : ces modules seront-ils des services partagés uniques ou répliqués ? Dans un monolithe modulaire, la question se pose différemment (une seule instance au MVP), mais la réponse conditionnera les modes de communication (`communication.md`) et la trajectoire d'extraction (§10.3).

---

# 10. Cas d'évolution

| Scénario | Impact sur les modules | Pourquoi la structure l'absorbe |
|---|---|---|
| **10.1 Scission de MOD-02** (configuration / lecture publique) sous tension de charge | Deux modules, une interface interne ajoutée | Scénario déjà envisagé en phase 06 §10 ; les règles R1–R3 rendent la scission locale, jamais globale |
| **10.2 Vente physique** (EF-120, reporté hors MVP) | Un module supplémentaire consommant MOD-04 et MOD-01 | Strictement additif — aucun module existant ne change de frontière ; l'inventaire central (MOD-04) a été conçu pour |
| **10.3 Extraction d'un module** post-MVP | Le module devient une unité de déploiement autonome | Critères d'arbitrage : charge isolée, cycle de déploiement distinct, équipe dédiée. La structure canonique (§5) garantit que tous ses effets externes passent déjà par des frontières identifiées |
| **10.4 Nouveau moyen de paiement** | Couche d'adaptation de MOD-05 uniquement | P8 : les externes sont interchangeables derrière un adaptateur ; aucun autre module n'est concerné |
| **10.5 Application mobile native participant** | Aucun changement de module ; un client supplémentaire | Déjà anticipé en phase 06 : la frontière clients / infrastructure reste identique |

---

# 11. Ce que les modules ne préjugent pas

La définition des modules ne préjuge pas :

- des **langages et frameworks** (phase 09) ;
- des **bases de données** — une par module ou partagée est une décision de la phase 08 (le monolithe modulaire ne l'impose pas dans un sens ni dans l'autre) ;
- des **protocoles et formats** portant les interfaces — les modes logiques sont définis dans `communication.md`, leur implémentation en phase 09 ;
- de la **topologie de déploiement** finale (phases 09 et 10) ;
- de l'**ordre d'implémentation** des modules (phase 17) ;
- de l'**architecture des applications clientes** (web, scan QR).

---

# 12. Résumé

Ce document définit **douze modules logiciels** (`MOD-01` à `MOD-12`), un par bloc fonctionnel, détenteurs exclusifs de leurs agrégats et regroupés au MVP en **monolithe modulaire** recommandé — posture dérivée du principe P9 et de l'anti-principe « architecture distribuée prématurée ». Chaque module suit une **structure interne canonique** en quatre parties (domaine, application, exposition, adaptation) qui isole son noyau de domaine. Six **règles structurelles normatives** (R1 à R6) gouvernent les relations inter-modules, rendant vérifiables l'acyclicité du graphe de dépendances et l'étanchéité des frontières. Les clients et systèmes externes restent hors modules, derrière des interfaces. L'incohérence `StatutBilletUtilisé` reste signalée en attente d'arbitrage avant `interfaces.md`.

---

# 13. Critères de qualité du document

- chaque module a une frontière, un contenu détenu et des interfaces identifiés ;
- la chaîne de traçabilité SD → BC → composant → BF → MOD est vérifiable ;
- les agrégats et interfaces reprennent exactement les tables sources sans invention ;
- les règles R1 à R6 sont opposables et rattachées à leurs principes ;
- la posture « monolithe modulaire » est présentée comme recommandation fondée, pas comme choix technique définitif ;
- aucune technologie, base de données ou protocole n'est imposé.

---

# 14. Statut

| Champ | Valeur |
|---|---|
| **Document** | `modules.md` |
| **Version** | 1.0 |
| **Statut** | Proposition — à valider par l'équipe |
| **Périmètre** | MVP Eventix |
| **Marché** | Cameroun |
| **Document suivant** | `responsabilites.md` |

| Élément | État |
|---|---|
| Modules définis | ✅ 12/12 (un par bloc fonctionnel) |
| Agrégats attribués | ✅ Couverture exhaustive, sans chevauchement |
| Interfaces fournies/consommées | ✅ Héritées de la table de traçabilité (phase 06) |
| Règles structurelles | ✅ R1 à R6 définies et fondées |
| Posture MVP | ✅ Monolithe modulaire recommandé (P9, anti-principes) |
| Incohérence StatutBilletUtilisé | ⚠️ Signalée — arbitrage en attente |
| Décisions techniques | ⏳ NON PRÉJUGÉES |

---

## Notes de rédaction

### Séparation avec `responsabilites.md`

Ce document répond à « quelles unités de code et quelles règles » ; le suivant répond à « qui fait quoi ». Les capacités fonctionnelles (EF) ne sont donc pas répétées ici : elles vivent dans `decomposition-fonctionnelle.md` et seront attribuées précisément dans `responsabilites.md`, qui s'appuiera aussi sur `services-de-domaine.md`.

### Sur la recommandation « monolithe modulaire »

Elle est formulée au niveau logique parce que c'est le dernier endroit où elle peut être débattue sans entraîner de choix d'outillage : les principes validés (P9, anti-principes) la rendent presque obligatoire pour le MVP, mais sa confirmation et sa traduction physique appartiennent aux phases 09 et 10. Le §10.3 fixe dès maintenant les critères objectifs qui permettront d'en sortir proprement si la croissance l'exige.
