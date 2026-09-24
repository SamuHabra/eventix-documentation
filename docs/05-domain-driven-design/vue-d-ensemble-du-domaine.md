# Vue d’ensemble du domaine Eventix

## 1. Objet du document

Ce document établit la **vue stratégique et opérationnelle du domaine métier Eventix** dans le cadre de la démarche Domain-Driven Design (DDD).

Il constitue une première carte de référence permettant de passer de l’analyse des besoins à l’identification progressive :

- des capacités métier ;
- des domaines et sous-domaines ;
- du Core Domain ;
- des domaines Supporting et Generic ;
- des interactions entre domaines ;
- des frontières conceptuelles candidates ;
- des zones de complexité et de différenciation ;
- des hypothèses structurantes à confirmer dans les travaux DDD suivants.

Ce document **ne définit pas encore les Bounded Contexts définitifs**, les agrégats, les entités ou l’architecture technique. Ces éléments feront l’objet de documents dédiés.

---

## 2. Positionnement dans la démarche

La vue du domaine est construite à partir des travaux réalisés dans les phases précédentes :

```text
Organisation du projet
        ↓
Vision produit
        ↓
Découverte du métier
        ↓
Analyse des besoins
        ↓
┌──────────────────────────────┐
│ Vue d’ensemble du domaine    │
└──────────────────────────────┘
        ↓
Sous-domaines
        ↓
Core Domain
        ↓
Bounded Contexts
        ↓
Context Map
        ↓
Modèle tactique DDD
```

La démarche retenue est volontairement **métier avant technique**.

Aucune correspondance automatique n'est établie entre domaine métier, module logiciel, service ou microservice.

---

# 3. Domaine métier global

## 3.1 Définition

Le domaine métier d’Eventix est celui de la **billetterie événementielle numérique**.

Eventix permet notamment de mettre en relation :

- des organisateurs ;
- des participants ;
- des agents de contrôle ;
- la plateforme Eventix ;
- des services externes nécessaires au fonctionnement de la billetterie.

La mission métier centrale est de permettre à un organisateur de **proposer et gérer des accès à un événement**, et à un participant de **découvrir, acheter ou obtenir un billet, puis utiliser cet accès de manière fiable et vérifiable**.

---

## 3.2 Vue globale

```text
                         ┌──────────────────┐
                         │   ORGANISATEUR   │
                         └────────┬─────────┘
                                  │
                         crée / configure
                                  │
                                  ▼
                         ┌──────────────────┐
                         │     ÉVÉNEMENT    │
                         └────────┬─────────┘
                                  │
                         définit l'offre
                                  │
                                  ▼
                         ┌──────────────────┐
                         │    BILLETTERIE   │
                         └────────┬─────────┘
                                  │
                         achat / réservation
                                  │
                                  ▼
                         ┌──────────────────┐
                         │    TRANSACTION   │
                         └────────┬─────────┘
                                  │
                           paiement confirmé
                                  │
                                  ▼
                         ┌──────────────────┐
                         │      BILLET      │
                         └────────┬─────────┘
                                  │
                         émission / distribution
                                  │
                                  ▼
                         ┌──────────────────┐
                         │   PARTICIPANT    │
                         └────────┬─────────┘
                                  │
                               accès
                                  │
                                  ▼
                         ┌──────────────────┐
                         │     CONTRÔLE     │
                         └──────────────────┘
```

Cette représentation constitue une **vue métier simplifiée** et non une architecture technique.

---

# 4. Grandes capacités métier

L'analyse combinée des fonctionnalités, capacités, processus et règles métier fait apparaître plusieurs capacités majeures.

## 4.1 Gestion des événements

Eventix doit permettre de :

- créer un événement ;
- configurer ses caractéristiques ;
- définir sa capacité ;
- définir ses modalités d'accès ;
- configurer les billets ;
- publier l'événement ;
- suivre son évolution ;
- gérer son cycle de vie.

Cette capacité concerne principalement la relation entre l'organisateur et l'événement.

---

## 4.2 Gestion de l'offre de billetterie

Cette capacité consiste à définir ce qui peut être vendu ou attribué dans le cadre d'un événement.

Elle couvre notamment :

- les types de billets ;
- les prix ;
- les quantités disponibles ;
- les places ou sièges lorsqu'ils sont configurés ;
- les conditions d'accès ;
- la disponibilité.

Elle constitue le lien entre la configuration d'un événement et la transaction réalisée par un participant.

---

## 4.3 Gestion de la réservation

La réservation doit être distinguée du paiement et du billet.

Une réservation peut temporairement retenir une disponibilité.

Dans le modèle métier établi :

```text
PENDING
   │
   ├── paiement confirmé
   │        ↓
   │    poursuite de l'achat
   │
   └── expiration
            ↓
         EXPIRED
            ↓
     libération de la disponibilité
```

Une expiration de réservation ne signifie donc pas nécessairement que le paiement a échoué.

---

## 4.4 Gestion du paiement et de la transaction

La transaction financière constitue une capacité distincte de la réservation et du billet.

Elle doit notamment prendre en compte :

- la tentative de paiement ;
- la confirmation ;
- l'échec ;
- les confirmations tardives ;
- l'idempotence ;
- la réconciliation ;
- le remboursement lorsque les conditions métier sont réunies.

Une confirmation de paiement ne doit pas provoquer plusieurs créations de billets ou plusieurs remboursements.

---

## 4.5 Gestion du billet

Le billet représente l'accès attribué au participant.

Son cycle de vie est lié à :

```text
Disponibilité
    ↓
Réservation
    ↓
Paiement
    ↓
Émission
    ↓
Distribution
    ↓
Contrôle
    ↓
Utilisation
```

Le billet doit pouvoir être :

- identifié ;
- distribué ;
- vérifié ;
- contrôlé ;
- marqué comme utilisé ;
- invalidé ou annulé selon les règles métier applicables.

---

## 4.6 Distribution du billet

Eventix doit permettre au participant de recevoir son billet.

Dans le périmètre actuellement défini, cela inclut notamment :

- la mise à disposition du billet ;
- le téléchargement ;
- l'envoi par email ;
- les canaux de distribution prévus par le MVP.

WhatsApp a également été identifié comme canal potentiel de distribution du billet dans les besoins établis.

Les mécanismes techniques permettant cette distribution ne sont pas définis dans ce document.

---

## 4.7 Contrôle et validation du billet

Le contrôle constitue une capacité particulièrement importante pour garantir l'intégrité de l'accès à l'événement.

Le système doit permettre de déterminer notamment si un billet est :

- valide ;
- déjà utilisé ;
- faux ;
- annulé ;
- utilisable ou non.

Une contrainte métier importante concerne également la rapidité du contrôle.

Le système doit pouvoir fournir une réponse suffisamment rapidement lors du scan.

Lorsque la connectivité est limitée, le fonctionnement local et la synchronisation ont été identifiés comme besoins métier importants.

---

## 4.8 Gestion des participants

Cette capacité concerne notamment :

- la consultation des événements ;
- la sélection d'une offre ;
- l'achat ;
- la réception du billet ;
- l'accès à l'événement.

Le participant constitue l'un des principaux utilisateurs du système mais son modèle ne doit pas être confondu automatiquement avec un domaine métier.

---

## 4.9 Suivi et gestion de l'activité événementielle

Eventix doit également permettre aux organisateurs de suivre leur activité.

Cela comprend notamment :

- les ventes ;
- les statistiques ;
- l'état des événements ;
- les informations nécessaires au suivi de l'activité.

La granularité exacte de cette capacité devra être précisée dans les travaux DDD ultérieurs.

---

# 5. Processus métier transversal

Le domaine Eventix peut être représenté par plusieurs processus qui se croisent.

## 5.1 Cycle de l'événement

```text
Création
   ↓
Configuration
   ↓
Publication
   ↓
Vente
   ↓
Tenue de l'événement
   ↓
Clôture
```

---

## 5.2 Cycle transactionnel

```text
Sélection
   ↓
Réservation
   ↓
Paiement
   ↓
Confirmation
   ↓
Émission du billet
   ↓
Distribution
```

---

## 5.3 Cycle de contrôle

```text
Billet présenté
      ↓
Identification
      ↓
Vérification
      ↓
┌───────────────┐
│ Valide ?      │
└───────┬───────┘
        │
   ┌────┴────┐
   ↓         ↓
  OUI       NON
   │         │
   ↓         ↓
 Accès      Refus
   │
   ↓
Marquage comme utilisé
```

---

# 6. Domaines candidats

À ce stade, les domaines suivants sont identifiés comme **domaines candidats**.

Ils ne constituent pas encore une liste définitive de Bounded Contexts.

| Domaine candidat | Rôle métier | Valeur | Complexité | Statut |
|---|---|---|---|---|
| Gestion des événements | Gérer le cycle de vie des événements | Élevée | Moyenne | Candidat |
| Offre de billetterie | Définir ce qui est vendu | Élevée | Élevée | Candidat |
| Réservation | Gérer temporairement la disponibilité | Très élevée | Élevée | Candidat |
| Transaction / paiement | Gérer la transaction financière | Très élevée | Élevée | Candidat |
| Billetterie | Gérer le cycle de vie du billet | Très élevée | Très élevée | Candidat |
| Contrôle | Vérifier l'accès et l'utilisation du billet | Très élevée | Très élevée | Candidat |
| Distribution | Acheminer les billets vers le participant | Moyenne | Moyenne | Candidat |
| Statistiques / suivi | Donner de la visibilité sur l'activité | Moyenne | Moyenne | Candidat |
| Notifications | Informer les utilisateurs | Moyenne | Faible à moyenne | Candidat |
| Identité / accès | Gérer l'identité et les accès | Nécessaire | Faible à moyenne | Candidat |

Cette classification est **préliminaire**.

---

# 7. Classification stratégique préliminaire

La classification DDD retenue à ce stade est une hypothèse de travail.

## 7.1 Core Domain

Le cœur d'Eventix doit être recherché autour de la capacité de la plateforme à fournir un **accès événementiel fiable, sécurisé et traçable au travers de la billetterie**.

Le Core Domain est donc envisagé comme un ensemble cohérent autour de :

```text
              CORE EVENTIX
                   │
        ┌──────────┼──────────┐
        ↓          ↓          ↓
   Billetterie  Transaction  Contrôle
        │          │          │
        └──────────┼──────────┘
                   ↓
          Accès événementiel
```

Cette classification est volontairement plus large que la seule fonctionnalité d'achat.

---

## 7.2 Supporting Domains

Des capacités nécessaires au fonctionnement d'Eventix mais moins différenciantes sont susceptibles d'être classées comme Supporting Domains.

Exemples candidats :

- gestion des événements ;
- gestion de l'offre ;
- statistiques ;
- distribution ;
- certaines capacités administratives.

Cette classification devra être confirmée après analyse plus détaillée.

---

## 7.3 Generic Domains

Certaines capacités peuvent relever de domaines génériques ou de services spécialisés existants.

Exemples candidats :

- authentification ;
- gestion d'identité ;
- notifications ;
- certaines capacités techniques transverses.

L'objectif n'est pas de réinventer une capacité générique lorsque celle-ci ne constitue pas une source de différenciation pour Eventix.

---

# 8. Pourquoi le Core Domain est plus large que « l'achat d'un billet »

Limiter le Core Domain à l'achat serait insuffisant.

La valeur d'Eventix ne réside pas uniquement dans le fait de permettre à un utilisateur de payer.

La chaîne métier doit garantir la cohérence entre :

```text
Disponibilité
      ↓
Réservation
      ↓
Paiement
      ↓
Billet
      ↓
Distribution
      ↓
Contrôle
      ↓
Accès
```

Une rupture dans cette chaîne peut compromettre directement la valeur délivrée :

- une disponibilité incorrecte peut provoquer une survente ;
- une réservation mal gérée peut bloquer inutilement un billet ;
- un paiement mal réconcilié peut provoquer une incohérence financière ;
- un billet incorrectement émis peut empêcher l'accès ;
- un contrôle incorrect peut permettre plusieurs utilisations ;
- une validation trop lente peut dégrader fortement l'expérience d'entrée.

La complexité métier se situe donc dans **la cohérence du cycle d'accès événementiel**, et pas uniquement dans la transaction de paiement.

---

# 9. Zones de complexité métier

Plusieurs zones présentent une complexité supérieure et devront recevoir une attention particulière.

## 9.1 Disponibilité et concurrence

Plusieurs participants peuvent tenter d'acquérir simultanément les mêmes billets.

Le domaine devra donc préserver la cohérence de la disponibilité.

---

## 9.2 Expiration des réservations

Une réservation temporaire doit pouvoir expirer sans être confondue avec un échec de paiement.

Cela implique une distinction claire entre :

- réservation ;
- paiement ;
- billet.

---

## 9.3 Paiement tardif et réconciliation

Un paiement peut être confirmé après l'expiration d'une réservation.

Le système métier doit alors déterminer :

```text
Billet toujours disponible ?
        │
   ┌────┴────┐
   ↓         ↓
  OUI       NON
   │         │
   ↓         ↓
Attribuer   Réconcilier
le billet   / rembourser
```

Cette situation constitue une zone importante de complexité métier.

---

## 9.4 Idempotence

Les événements ou confirmations provenant de systèmes externes peuvent être reçus plusieurs fois.

Le domaine doit empêcher qu'une même opération financière ou métier soit exécutée plusieurs fois.

---

## 9.5 Contrôle du billet

Le contrôle doit garantir qu'un billet ne puisse pas être utilisé plusieurs fois dans des conditions incompatibles avec les règles métier.

La contrainte de faible latence et les problèmes de connectivité rendent cette capacité particulièrement sensible.

---

# 10. Différenciation stratégique

La différenciation d'Eventix ne doit pas être présumée à partir de la technologie utilisée.

Elle doit être recherchée dans la capacité de la plateforme à résoudre efficacement les problèmes métier de la billetterie événementielle.

À ce stade, les axes potentiellement différenciants sont :

- fiabilité du cycle de vie du billet ;
- cohérence entre réservation, paiement et billet ;
- sécurité et authenticité des billets ;
- rapidité du contrôle ;
- capacité à fonctionner dans des conditions de connectivité imparfaite ;
- visibilité sur les ventes et l'activité événementielle.

Ces axes constituent des **hypothèses stratégiques** et non des affirmations définitives de positionnement concurrentiel.

---

# 11. Interactions majeures entre domaines

Une première vue des interactions métier peut être représentée ainsi :

```text
             Gestion événement
                    │
                    ▼
             Offre de billets
                    │
                    ▼
               Réservation
                    │
                    ▼
              Transaction
                    │
             confirmation
                    ▼
                Billet
                    │
              distribution
                    ▼
               Participant
                    │
                 accès
                    ▼
                Contrôle
```

Avec des interactions transversales :

```text
Transaction ───────→ Billet
     │
     └──────────────→ Réconciliation

Billet ─────────────→ Distribution

Billet ─────────────→ Contrôle

Événement ──────────→ Statistiques

Tous les domaines ──→ Notifications
```

Cette représentation est volontairement conceptuelle.

Elle ne préjuge pas des mécanismes de communication qui seront retenus ultérieurement.

---

# 12. Frontières conceptuelles candidates

Certaines frontières apparaissent naturellement à partir des responsabilités métier.

## 12.1 Événement

Responsabilité principale :

> Définir et gérer l'événement.

---

## 12.2 Offre de billetterie

Responsabilité principale :

> Définir ce qui peut être vendu et dans quelles conditions.

---

## 12.3 Réservation

Responsabilité principale :

> Gérer temporairement la disponibilité pour une intention d'achat.

---

## 12.4 Transaction

Responsabilité principale :

> Gérer l'état de la transaction financière et sa réconciliation.

---

## 12.5 Billet

Responsabilité principale :

> Représenter l'accès attribué à un participant.

---

## 12.6 Contrôle

Responsabilité principale :

> Déterminer si un billet présenté peut être utilisé et enregistrer son utilisation.

Ces frontières sont **candidates**. Leur transformation en Bounded Contexts devra être justifiée dans `bounded-contexts.md`.

---

# 13. Principes de séparation du domaine

La modélisation Eventix doit respecter plusieurs principes.

### 13.1 Ne pas confondre réservation, paiement et billet

```text
Réservation ≠ Paiement ≠ Billet
```

Ils possèdent des responsabilités et des cycles de vie différents.

---

### 13.2 Ne pas confondre fonctionnalité et domaine

Une fonctionnalité visible par l'utilisateur n'est pas nécessairement un domaine métier autonome.

---

### 13.3 Ne pas confondre domaine et service technique

Un domaine métier peut être implémenté par plusieurs composants techniques.

Inversement, un composant technique peut servir plusieurs domaines.

---

### 13.4 Ne pas créer des frontières uniquement pour des raisons techniques

La séparation doit d'abord être justifiée par :

- le métier ;
- les responsabilités ;
- les règles ;
- la cohérence du modèle ;
- la complexité ;
- les changements attendus.

---

### 13.5 Préserver le langage métier

Les termes utilisés dans les différents domaines devront rester cohérents avec le **langage ubiquitaire Eventix**.

---

# 14. Décisions prises

Les décisions suivantes sont retenues pour la suite du travail DDD :

| Décision | Statut |
|---|---|
| Eventix est modélisé comme un domaine de billetterie événementielle | Décidé |
| La vue du domaine combine stratégie et opérationnel | Décidé |
| L'identification des domaines croise fonctionnalités, capacités, processus, règles, complexité et valeur | Décidé |
| Le Core Domain ne se limite pas à l'achat | Décidé |
| Réservation, paiement et billet sont séparés conceptuellement | Décidé |
| Les Bounded Contexts ne sont pas encore figés | Décidé |
| Les frontières actuelles sont des candidates | Décidé |
| La technologie ne doit pas déterminer les frontières métier | Décidé |

---

# 15. Hypothèses à confirmer

Les éléments suivants doivent encore être validés :

- la liste définitive des sous-domaines ;
- la classification exacte Core / Supporting / Generic ;
- les frontières des Bounded Contexts ;
- les relations entre les contextes ;
- les responsabilités exactes de chaque contexte ;
- les modèles propres à chaque contexte ;
- les événements de domaine pertinents ;
- les agrégats nécessaires ;
- les entités et objets valeur.

Ces décisions ne doivent pas être anticipées artificiellement dans ce document.

---

# 16. Questions ouvertes

Les questions suivantes devront être traitées dans les documents DDD suivants :

1. La gestion de l'événement et la gestion de l'offre doivent-elles appartenir au même sous-domaine ?
2. La réservation constitue-t-elle un sous-domaine autonome ?
3. Le paiement doit-il être traité comme un domaine propre ou comme une capacité du domaine transactionnel ?
4. Le billet et son contrôle appartiennent-ils au même contexte ou à deux contextes distincts ?
5. Quelle frontière exacte sépare la billetterie de la gestion des événements ?
6. Quelles capacités sont réellement différenciantes pour Eventix ?
7. Quels domaines peuvent être externalisés ou s'appuyer sur des solutions génériques ?
8. Quels domaines présentent les règles métier les plus complexes ?
9. Quels domaines évoluent indépendamment les uns des autres ?
10. Quels domaines doivent rester fortement cohérents entre eux ?

---

# 17. Conséquences pour la suite du DDD

Cette vue d'ensemble constitue la base des prochains travaux.

```text
Vue d'ensemble du domaine
          │
          ├──────────────→ domaines.md
          │
          ├──────────────→ sous-domaines.md
          │
          ├──────────────→ core-domain.md
          │
          ├──────────────→ bounded-contexts.md
          │
          ├──────────────→ context-map.md
          │
          ├──────────────→ langage-ubiquitaire.md
          │
          ├──────────────→ entites.md
          │
          ├──────────────→ objets-valeur.md
          │
          ├──────────────→ agregats.md
          │
          └──────────────→ evenements-de-domaine.md
```

Chaque document suivant devra **affiner**, et non contredire sans justification, les décisions établies ici.

---

# 18. Synthèse

Eventix est modélisé comme un domaine de **billetterie événementielle numérique** dont la valeur centrale repose sur la capacité à transformer une intention d'achat en un **accès événementiel fiable, sécurisé, traçable et contrôlable**.

Le domaine ne doit pas être réduit à une simple fonctionnalité de paiement.

La chaîne métier centrale est :

```text
Événement
   ↓
Offre
   ↓
Réservation
   ↓
Transaction
   ↓
Billet
   ↓
Distribution
   ↓
Contrôle
   ↓
Accès
```

Le Core Domain est donc considéré, à ce stade, comme un ensemble cohérent autour de la **billetterie, de la transaction et du contrôle**, tandis que d'autres capacités constituent des domaines candidats Supporting ou Generic.

Cette classification reste volontairement évolutive.

La prochaine étape consiste à transformer cette première carte en une **décomposition explicite des domaines et sous-domaines**, puis à établir les frontières et relations nécessaires à la modélisation DDD.

---

## 19. Statut du document

**Statut :** Validé comme base de travail DDD  
**Phase :** 05 — Domain-Driven Design  
**Nature :** Document stratégique de référence  
**Niveau :** Domaine / stratégie DDD  
**Prochaine étape :** `domaines.md` puis `sous-domaines.md`  



# Vue d'ensemble du domaine — Eventix

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
3. [Principes de construction](#3-principes-de-construction)
4. [Définition du domaine](#4-définition-du-domaine)
5. [Vue globale du domaine](#5-vue-globale-du-domaine)
6. [Domaines identifiés](#6-domaines-identifiés)
   - 6.1 [Domaine — Confiance et vérification](#61-domaine--confiance-et-vérification)
   - 6.2 [Domaine — Gestion des événements](#62-domaine--gestion-des-événements)
   - 6.3 [Domaine — Disponibilité et espaces](#63-domaine--disponibilité-et-espaces)
   - 6.4 [Domaine — Réservation](#64-domaine--réservation)
   - 6.5 [Domaine — Paiement](#65-domaine--paiement)
   - 6.6 [Domaine — Billetterie](#66-domaine--billetterie)
   - 6.7 [Domaine — Distribution](#67-domaine--distribution)
   - 6.8 [Domaine — Contrôle d'accès](#68-domaine--contrôle-daccès)
   - 6.9 [Domaine — Annulation et report](#69-domaine--annulation-et-report)
   - 6.10 [Domaine — Remboursement](#610-domaine--remboursement)
   - 6.11 [Domaine — Sécurité et signalement](#611-domaine--sécurité-et-signalement)
   - 6.12 [Domaine — Finance](#612-domaine--finance)
   - 6.13 [Domaine — Statistiques](#613-domaine--statistiques)
   - 6.14 [Domaine — Historique et traçabilité](#614-domaine--historique-et-traçabilité)
7. [Core Domain](#7-core-domain)
8. [Sous-domaines de support](#8-sous-domaines-de-support)
9. [Sous-domaines génériques](#9-sous-domaines-génériques)
10. [Langage ubiquitaire](#10-langage-ubiquitaire)
11. [Relations entre domaines](#11-relations-entre-domaines)
12. [Flux métier principal](#12-flux-métier-principal)
13. [Invariants du domaine](#13-invariants-du-domaine)
14. [Événements de domaine](#14-événements-de-domaine)
15. [Frontières du domaine](#15-frontières-du-domaine)
16. [Questions ouvertes](#16-questions-ouvertes)
17. [Résumé](#17-résume)
18. [Critères de qualité du document](#18-critères-de-qualité-du-document)
19. [Statut](#19-statut)

---

# 1. Objectif

Ce document définit la vue d'ensemble du domaine Eventix dans le cadre du Domain-Driven Design (DDD).

Il répond aux questions :

> **Quel est le périmètre métier d'Eventix ?**
>
> **Quels sont les domaines métier qui le composent ?**
>
> **Quels sont les domaines critiques, de support et génériques ?**
>
> **Comment les domaines interagissent-ils ?**

Il ne définit **pas encore** :

- les bounded contexts détaillés (voir `bounded-contexts.md`) ;
- les agrégats (voir `agregats.md`) ;
- les entités et objets-valeur (voir `entites.md` et `objets-valeur.md`) ;
- les services de domaine (voir `services-de-domaine.md`) ;
- les événements de domaine détaillés (voir `evenements-de-domaine.md`) ;
- les règles métier détaillées (voir `regles-du-domaine.md`) ;
- l'architecture technique.

Ces éléments seront traités dans les documents spécifiques de la phase 05.

---

# 2. Sources de référence

La vue d'ensemble du domaine est dérivée principalement de :

- `03-decouverte-du-metier/ecosysteme-eventix.md`
- `03-decouverte-du-metier/besoins-metier.md`
- `03-decouverte-du-metier/processus-metier.md`
- `03-decouverte-du-metier/regles-metier.md`
- `03-decouverte-du-metier/user-journeys.md`
- `04-analyse-des-besoins/exigences-fonctionnelles.md`

---

# 3. Principes de construction

## 3.1 Alignement sur le langage métier

Chaque domaine est nommé et défini avec le **langage ubiquitaire** d'Eventix. Le vocabulaire technique ne doit pas remplacer le vocabulaire métier.

## 3.2 Distillation du domaine

Les domaines sont catégorisés selon le principe de distillation du DDD cite🛠web_search:2#3:~:text=Context Distillation ou Distillation du domaine...3 domaines :

| Catégorie | Définition |
|---|---|
| **Core Domain** | Ce qui apporte la valeur distinctive d'Eventix, ce qui le différencie de la concurrence |
| **Supporting Domain** | Nécessaire au fonctionnement mais non différentiant |
| **Generic Domain** | Résoluble par des solutions existantes, standardisables |

## 3.3 Séparation des responsabilités

Chaque domaine possède une responsabilité métier clairement délimitée. Un domaine ne doit pas absorber les responsabilités d'un autre.

## 3.4 Une question ouverte reste une question ouverte

Lorsqu'une frontière de domaine dépend d'une décision non encore prise, elle est explicitement marquée :

> **À PRÉCISER** — voir `03-decouverte-du-metier/questions-metier-ouvertes.md`.

---

# 4. Définition du domaine

Le **domaine Eventix** représente l'ensemble des connaissances, activités et règles métier relatives à la plateforme numérique permettant :

- la **découverte** d'événements par les participants ;
- la **création et gestion** d'événements par les organisateurs ;
- la **billetterie** (réservation, paiement, émission de billets) ;
- le **contrôle d'accès** aux événements ;
- la **gestion financière** entre Eventix et les organisateurs ;
- la **sécurité et la confiance** au sein de l'écosystème.

Eventix ne modélise pas l'ensemble de l'écosystème événementiel. Il modélise la **couche numérique** qui facilite la rencontre entre organisateurs et participants.

---

# 5. Vue globale du domaine

```text
                         DOMAINE EVENTIX

    ┌─────────────────────────────────────────────────────────┐
    │                    CONFIDENCE                           │
    │         (Vérification, Sécurité, Signalement)           │
    └─────────────────────────────────────────────────────────┘
                              │
                              ▼
    ┌─────────────────────────────────────────────────────────┐
    │              GESTION DES ÉVÉNEMENTS                     │
    │    (Création, Configuration, Publication, Archivage)    │
    └─────────────────────────────────────────────────────────┘
                              │
                              ▼
    ┌─────────────────────────────────────────────────────────┐
    │              DISPONIBILITÉ ET ESPACES                   │
    │         (Zones, Places, Capacités, Catégories)          │
    └─────────────────────────────────────────────────────────┘
                              │
                              ▼
    ┌─────────────┐    ┌─────────────┐    ┌─────────────────┐
    │ RÉSERVATION │───►│   PAIEMENT  │───►│   BILLETTERIE   │
    │  (Blocage   │    │  (Mobile    │    │ (Émission, QR   │
    │ temporaire) │    │   Money)    │    │  Code, Propriété)│
    └─────────────┘    └─────────────┘    └─────────────────┘
                              │
                              ▼
    ┌─────────────────────────────────────────────────────────┐
    │              CONTRÔLE D'ACCÈS                          │
    │    (Scan, Validation, Consommation, Mode dégradé)      │
    └─────────────────────────────────────────────────────────┘
                              │
                              ▼
    ┌─────────────────────────────────────────────────────────┐
    │                    PARTICIPATION                        │
    └─────────────────────────────────────────────────────────┘

    ┌─────────────┐    ┌─────────────┐    ┌─────────────────┐
    │ ANNULATION  │◄──►│ REMBOURSE-  │◄──►│    FINANCE      │
    │   / REPORT  │    │   MENT      │    │(Clôture, Solde, │
    │             │    │             │    │    Retrait)     │
    └─────────────┘    └─────────────┘    └─────────────────┘

    ┌─────────────┐    ┌─────────────┐    ┌─────────────────┐
    │  STATIS-    │    │  HISTORIQUE │    │   DISTRIBU-     │
    │   TIQUE     │    │   ET TRAÇA- │    │    TION         │
    │             │    │   BILITÉ    │    │ (Email, Compte) │
    └─────────────┘    └─────────────┘    └─────────────────┘
```

---

# 6. Domaines identifiés

## 6.1 Domaine — Confiance et vérification

**Responsabilité :** Garantir la crédibilité des organisateurs, des événements et la sécurité de la plateforme.

**Acteurs concernés :** Eventix (équipe de vérification), Organisateur, Participant.

**Activités métier :**

- Vérification d'un événement avant publication.
- Vérification de la crédibilité d'un organisateur.
- Analyse des signalements.
- Évaluation du niveau de risque.
- Bannissement d'organisations frauduleuses.
- Traçabilité des décisions de sécurité.

**Pourquoi ce domaine :** La confiance est la fondation de la plateforme. Sans vérification, Eventix ne peut pas garantir la légitimité des événements proposés.

**Catégorie DDD :** Core Domain — différenciant pour un marché comme le Cameroun où la confiance numérique est un enjeu critique.

---

## 6.2 Domaine — Gestion des événements

**Responsabilité :** Permettre la création, la configuration, la publication et le cycle de vie d'un événement.

**Acteurs concernés :** Organisateur, Eventix.

**Activités métier :**

- Création d'un événement en brouillon.
- Configuration (nom, description, date, lieu, capacité, catégories de billets, prix, périodes de vente).
- Modification d'un événement conformément à son état.
- Soumission pour vérification.
- Publication après validation.
- Arrêt des ventes.
- Archivage.

**Pourquoi ce domaine :** C'est le point de départ de toute activité sur la plateforme. Un organisateur ne peut pas vendre de billets sans un événement configuré.

**Catégorie DDD :** Core Domain — la qualité de l'expérience de création d'événement différencie Eventix.

---

## 6.3 Domaine — Disponibilité et espaces

**Responsabilité :** Gérer la capacité physique et logique d'un événement, et garantir qu'une place ne peut être vendue deux fois.

**Acteurs concernés :** Organisateur, Eventix.

**Activités métier :**

- Configuration des espaces, zones et places.
- Association des catégories de billets aux disponibilités.
- Gestion des capacités par zone.
- Protection contre la réduction de capacité incompatible avec les billets déjà vendus.
- Suivi des disponibilités en temps réel.

**Pourquoi ce domaine :** La disponibilité est la ressource fondamentale que la plateforme gère. C'est ici que réside le risque de survente.

**Catégorie DDD :** Core Domain — la gestion fine des disponibilités est un différenciateur clé.

---

## 6.4 Domaine — Réservation

**Responsabilité :** Bloquer temporairement une disponibilité pour permettre à un participant de finaliser son achat.

**Acteurs concernés :** Participant, Eventix.

**Activités métier :**

- Création d'une réservation temporaire.
- Blocage de la disponibilité correspondante.
- Expiration après cinq minutes si non finalisée.
- Libération de la disponibilité après expiration.
- Prévention de la double attribution.

**Pourquoi ce domaine :** La réservation est le mécanisme qui protège la disponibilité pendant le parcours d'achat. Elle sépare le cycle de réservation du cycle de paiement.

**Catégorie DDD :** Core Domain — la fiabilité de la réservation est essentielle à l'intégrité de la billetterie.

---

## 6.5 Domaine — Paiement

**Responsabilité :** Traiter les transactions financières liées à l'achat de billets.

**Acteurs concernés :** Participant, Eventix, Services de paiement externe (Mobile Money).

**Activités métier :**

- Initiation d'un paiement.
- Suivi des états de paiement (`PENDING`, `CONFIRMED`, `FAILED`).
- Traitement des confirmations.
- Gestion des échecs.
- Garantie de l'idempotence.
- Réconciliation des paiements tardifs.

**Pourquoi ce domaine :** Le paiement est le moment de conversion. Sa fiabilité conditionne directement le revenu de la plateforme.

**Catégorie DDD :** Core Domain — l'intégration Mobile Money et la fiabilité des transactions sont critiques pour le marché camerounais.

---

## 6.6 Domaine — Billetterie

**Responsabilité :** Émettre, gérer et transférer les billets une fois l'achat finalisé.

**Acteurs concernés :** Participant, Eventix.

**Activités métier :**

- Finalisation d'un achat.
- Émission d'un billet.
- Génération du QR Code de contrôle.
- Association du billet à un événement et à un propriétaire.
- Garantie de l'unicité du propriétaire actif.
- Consultation et téléchargement du billet.
- Transfert de billet entre participants.
- Conservation de l'historique des transferts.

**Pourquoi ce domaine :** Le billet est le produit livré au participant. Sa validité et son intégrité sont essentielles.

**Catégorie DDD :** Core Domain — le billet est le cœur de la promesse faite au participant.

---

## 6.7 Domaine — Distribution

**Responsabilité :** Mettre le billet à disposition du participant et le distribuer via les canaux appropriés.

**Acteurs concernés :** Participant, Eventix.

**Activités métier :**

- Mise à disposition du billet dans le compte participant.
- Distribution du billet par email.
- Accès au billet depuis l'application.

**Pourquoi ce domaine :** La distribution est la promesse de livraison tenue. Elle sépare la logique d'émission du billet de sa mise à disposition.

**Catégorie DDD :** Supporting Domain — nécessaire mais standardisable.

---

## 6.8 Domaine — Contrôle d'accès

**Responsabilité :** Vérifier la validité des billets à l'entrée des événements et enregistrer la présence.

**Acteurs concernés :** Agent de contrôle, Organisateur, Eventix.

**Activités métier :**

- Affectation d'un contrôleur à un point d'entrée.
- Scan du QR Code.
- Vérification de l'authenticité, de l'événement et de l'état du billet.
- Refus des billets déjà utilisés, annulés ou d'un autre événement.
- Autorisation d'accès et marquage comme `USED`.
- Garantie d'une seule validation réussie.
- Enregistrement du contexte de contrôle.
- Gestion du mode multi-scanner et du mode dégradé mono-scanner.
- Réintégration après retour du mode dégradé.

**Pourquoi ce domaine :** Le contrôle d'accès est le dernier maillon de la chaîne de valeur. Sa fiabilité conditionne l'expérience sur place.

**Catégorie DDD :** Core Domain — la fiabilité du contrôle, notamment en mode dégradé, est un différenciateur.

---

## 6.9 Domaine — Annulation et report

**Responsabilité :** Gérer les situations où un événement ne peut pas se dérouler comme prévu.

**Acteurs concernés :** Organisateur, Eventix, Participant.

**Activités métier :**

- Annulation d'un événement.
- Blocage des nouvelles ventes après annulation.
- Invalidation des billets concernés.
- Information des participants.
- Détermination de l'éligibilité au remboursement.
- Report d'un événement.
- Conservation des billets lors d'un report.
- Gestion des incompatibilités de configuration lors d'un report.

**Pourquoi ce domaine :** L'annulation et le report sont des événements exceptionnels mais critiques pour la réputation et la confiance.

**Catégorie DDD :** Supporting Domain — nécessaire mais géré par des règles standardisables.

---

## 6.10 Domaine — Remboursement

**Responsabilité :** Traiter les remboursements lorsque les règles métier l'exigent.

**Acteurs concernés :** Eventix, Participant.

**Activités métier :**

- Détermination qu'un remboursement est requis.
- Création d'une demande de remboursement.
- Calcul du montant de référence.
- Suivi des états (`PENDING`, `PROCESSING`, `FAILED`, `COMPLETED`).
- Retentative en cas d'échec.
- Garantie de l'idempotence.
- Traitement progressif des volumes importants.

**Pourquoi ce domaine :** Le remboursement est une obligation légale et commerciale. Sa fiabilité conditionne la confiance.

**Catégorie DDD :** Supporting Domain — nécessaire mais géré par des processus standardisables.

---

## 6.11 Domaine — Sécurité et signalement

**Responsabilité :** Permettre aux participants de signaler des problèmes et à Eventix d'y répondre.

**Acteurs concernés :** Participant, Eventix.

**Activités métier :**

- Signalement d'un événement ou d'un organisateur.
- Enregistrement d'un motif de signalement.
- Ajout d'une description facultative.
- Analyse des signalements.
- Évaluation du niveau de risque.
- Application de mesures proportionnées.

**Pourquoi ce domaine :** La sécurité participative renforce la confiance collective dans la plateforme.

**Catégorie DDD :** Supporting Domain — complémentaire au domaine Confiance.

---

## 6.12 Domaine — Finance

**Responsabilité :** Gérer les flux financiers entre Eventix et les organisateurs.

**Acteurs concernés :** Organisateur, Eventix.

**Activités métier :**

- Préparation de la clôture financière après un événement.
- Détermination du montant net organisateur (ventes − remboursements − frais).
- Clôture financière.
- Rendu du solde disponible.
- Consultation du solde.
- Demande de retrait.
- Vérification que le retrait ne dépasse pas le solde.
- Suivi des états de retrait.
- Restitution au solde en cas d'échec.

**Pourquoi ce domaine :** La finance est le mécanisme de rétribution des organisateurs. Sa fiabilité conditionne l'adoption de la plateforme.

**Catégorie DDD :** Core Domain — la gestion financière transparente est un différenciateur pour les organisateurs.

---

## 6.13 Domaine — Statistiques

**Responsabilité :** Produire des indicateurs de pilotage pour les organisateurs sans modifier les données métier.

**Acteurs concernés :** Organisateur, Eventix.

**Activités métier :**

- Suivi des ventes.
- Analyse des ventes par événement, catégorie, période.
- Suivi des entrées.
- Suivi des participants (vendus, attendus, présents).
- Consultation de l'activité de l'événement.
- Production de statistiques en lecture seule.

**Pourquoi ce domaine :** Les statistiques permettent aux organisateurs de piloter leurs événements. Elles doivent être fiables sans impacter les opérations.

**Catégorie DDD :** Supporting Domain — lecture seule, dérivée des données métier.

---

## 6.14 Domaine — Historique et traçabilité

**Responsabilité :** Conserver l'historique des opérations importantes pour garantir la traçabilité.

**Acteurs concernés :** Eventix.

**Activités métier :**

- Conservation des opérations métier importantes.
- Association d'une opération à son contexte (acteur, événement, objet, date, résultat).
- Préservation de l'historique après modification.

**Pourquoi ce domaine :** La traçabilité est une exigence de confiance, de sécurité et potentiellement réglementaire.

**Catégorie DDD :** Generic Domain — résoluble par des solutions d'audit standard.

---

# 7. Core Domain

Les domaines suivants constituent le **Core Domain** d'Eventix — ce qui apporte la valeur distinctive et différencie la plateforme cite🛠web_search:2#3:~:text=Qu'est-ce qui apporte de la valeur...sépare de la concurrence :

```text
┌─────────────────────────────────────────────────────────────┐
│                     CORE DOMAIN                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────┐   ┌─────────────┐   ┌─────────────────┐   │
│  │  CONFIANCE  │   │   GESTION   │   │  DISPONIBILITÉ  │   │
│  │ ET VÉRIF.   │   │  ÉVÉNEMENTS │   │   ET ESPACES    │   │
│  └─────────────┘   └─────────────┘   └─────────────────┘   │
│                                                             │
│  ┌─────────────┐   ┌─────────────┐   ┌─────────────────┐   │
│  │ RÉSERVATION │   │   PAIEMENT  │   │   BILLETTERIE   │   │
│  └─────────────┘   └─────────────┘   └─────────────────┘   │
│                                                             │
│  ┌─────────────┐   ┌─────────────┐                          │
│  │  CONTRÔLE   │   │   FINANCE   │                          │
│  │   D'ACCÈS   │   │             │                          │
│  └─────────────┘   └─────────────┘                          │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**Justification :**

| Domaine | Justification du caractère critique |
|---|---|
| Confiance et vérification | Dans un marché émergent, la crédibilité de la plateforme est le premier frein à l'adoption. La vérification manuelle ou semi-automatisée est un différenciateur. |
| Gestion des événements | La simplicité et la flexibilité de création d'événement conditionnent l'adoption par les organisateurs. |
| Disponibilité et espaces | La gestion fine des capacités (places numérotées, zones, catégories) est un avantage concurrentiel face aux solutions génériques. |
| Réservation | La fiabilité du blocage temporaire (5 minutes, pas de double attribution) est essentielle à l'intégrité de la billetterie. |
| Paiement | L'intégration Mobile Money adaptée au contexte camerounais est un différenciateur technique et commercial. |
| Billetterie | L'émission sécurisée des billets avec QR Code et la gestion des transferts sont au cœur de la promesse produit. |
| Contrôle d'accès | La fiabilité du contrôle, notamment le mode dégradé mono-scanner, est un gage de qualité sur le terrain. |
| Finance | La transparence et la rapidité des retraits conditionnent la fidélisation des organisateurs. |

---

# 8. Sous-domaines de support

Les domaines suivants sont des **Supporting Domains** — nécessaires au fonctionnement mais non différentiants :

```text
┌─────────────────────────────────────────────────────────────┐
│                SUPPORTING DOMAINS                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────┐   ┌─────────────┐   ┌─────────────────┐   │
│  │ ANNULATION  │   │ REMBOURSE-  │   │   SÉCURITÉ ET   │   │
│  │  / REPORT   │   │   MENT      │   │   SIGNALEMENT   │   │
│  └─────────────┘   └─────────────┘   └─────────────────┘   │
│                                                             │
│  ┌─────────────┐   ┌─────────────┐                          │
│  │ STATISTIQUES│   │ DISTRIBU-   │                          │
│  │             │   │   TION      │                          │
│  └─────────────┘   └─────────────┘                          │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**Justification :**

| Domaine | Justification du caractère de support |
|---|---|
| Annulation et report | Nécessaire mais régi par des règles métier standardisables. La logique est principalement déclenchée par des événements du Core Domain. |
| Remboursement | Processus financier standard. L'idempotence est critique mais le processus lui-même est reproductible. |
| Sécurité et signalement | Complémentaire au domaine Confiance. Le mécanisme de signalement est standard, l'analyse peut être enrichie. |
| Statistiques | Lecture seule, dérivée des données du Core Domain. Les calculs sont standardisables. |
| Distribution | La mise à disposition par email et compte est un mécanisme de livraison standard. |

---

# 9. Sous-domaines génériques

Le domaine suivant est un **Generic Domain** — résoluble par des solutions existantes :

```text
┌─────────────────────────────────────────────────────────────┐
│                  GENERIC DOMAIN                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │           HISTORIQUE ET TRAÇABILITÉ                 │   │
│  │                                                     │   │
│  │  (Journalisation, audit, conservation               │   │
│  │   des opérations, traçabilité réglementaire)        │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**Justification :**

| Domaine | Justification du caractère générique |
|---|---|
| Historique et traçabilité | La journalisation d'événements métier avec contexte est un problème résolu par de nombreuses solutions (event sourcing, audit logs, change data capture). Eventix n'a pas besoin d'innover ici. |

---

# 10. Langage ubiquitaire

Le langage ubiquitaire d'Eventix est le vocabulaire partagé par l'ensemble de l'équipe. Les termes suivants sont définis au niveau du domaine :

| Terme | Définition dans le domaine Eventix |
|---|---|
| **Événement** | Occurrence planifiée (concert, conférence, spectacle, etc.) créée par un organisateur et publiée sur la plateforme. |
| **Organisateur** | Entité (personne physique ou morale) responsable de la création et de la gestion d'un événement. |
| **Participant** | Personne qui découvre, réserve, achète ou participe à un événement via Eventix. |
| **Billet** | Titre d'accès numérique émis par Eventix après finalisation d'un achat, contenant un QR Code unique. |
| **Réservation** | Blocage temporaire d'une disponibilité pendant cinq minutes maximum, avant finalisation de l'achat. |
| **Disponibilité** | Unité de capacité (place, zone, catégorie) pouvant être attribuée à un billet. |
| **Catégorie de billet** | Classification des billets d'un événement (ex: VIP, Standard, Early Bird) avec prix et capacité associés. |
| **QR Code** | Code-barres bidimensionnel généré pour chaque billet, servant à l'identification et au contrôle d'accès. |
| **Contrôle d'accès** | Opération de vérification du billet à l'entrée d'un événement, aboutissant à une autorisation ou un refus. |
| **Validation** | Action de marquer un billet comme `USED` après un contrôle d'accès réussi. |
| **Remboursement** | Restitution des fonds au participant lorsque les règles métier l'exigent (annulation, incompatibilité, etc.). |
| **Clôture financière** | Opération de calcul du montant net dû à l'organisateur après la fin d'un événement. |
| **Solde** | Montant disponible pour retrait par l'organisateur, résultant des clôtures financières. |
| **Retrait** | Demande de transfert des fonds du solde de l'organisateur vers son compte Mobile Money. |
| **Signalement** | Action d'un participant pour alerter Eventix sur un événement ou un organisateur suspect. |
| **Vérification** | Processus de contrôle de la crédibilité d'un événement ou d'un organisateur avant ou pendant la publication. |
| **Bannissement** | Mesure définitive interdisant à une organisation de poursuivre son activité sur la plateforme. |
| **Idempotence** | Propriété garantissant qu'une même opération répétée ne produit qu'un seul effet métier. |
| **Mode dégradé** | Mode de fonctionnement du contrôle d'accès où un seul scanner est actif faute de synchronisation fiable. |
| **Réconciliation** | Processus de traitement d'un paiement confirmé après expiration de la réservation associée. |

---

# 11. Relations entre domaines

Les domaines collaborent selon les principes de faible couplage définis dans les exigences fonctionnelles. Les relations suivantes représentent des **dépendances de résultat**, pas des fusions de responsabilité.

```text
CONFIDENCE
    │
    ├──► GESTION ÉVÉNEMENTS (vérification avant publication)
    │
    └──► SÉCURITÉ ET SIGNALEMENT (analyse des signalements)

GESTION ÉVÉNEMENTS
    │
    ├──► DISPONIBILITÉ (configuration des espaces et catégories)
    │
    └──► STATISTIQUES (données d'activité)

DISPONIBILITÉ
    │
    └──► RÉSERVATION (consommation d'une disponibilité)

RÉSERVATION
    │
    ├──► PAIEMENT (déclenchement du paiement)
    │
    └──► EXPIRATION ──► DISPONIBILITÉ (libération)

PAIEMENT
    │
    ├──► CONFIRMATION ──► BILLETTERIE (émission du billet)
    │
    ├──► ÉCHEC ──► RÉSERVATION (libération)
    │
    └──► TARDIF ──► RÉCONCILIATION ──► BILLETTERIE ou REMBOURSEMENT

BILLETTERIE
    │
    ├──► DISTRIBUTION (mise à disposition)
    │
    ├──► CONTRÔLE D'ACCÈS (validation)
    │
    └──► TRANSFERT ──► BILLETTERIE (changement de propriétaire)

CONTRÔLE D'ACCÈS
    │
    └──► HISTORIQUE (enregistrement du contrôle)

ANNULATION / REPORT
    │
    ├──► BILLETTERIE (invalidation ou conservation)
    │
    ├──► REMBOURSEMENT (déclenchement si éligible)
    │
    └──► DISTRIBUTION (information des participants)

REMBOURSEMENT
    │
    └──► FINANCE (impact sur le solde)

FINANCE
    │
    ├──► CLÔTURE ──► SOLDE
    │
    └──► RETRAIT ──► SERVICE DE PAIEMENT EXTERNE

HISTORIQUE
    │
    └──◄── Tous les domaines (opérations importantes)
```

---

# 12. Flux métier principal

Le flux métier principal représente le parcours de valeur le plus courant sur la plateforme :

```text
ORGANISATEUR
     │
     ├──► Crée un événement (brouillon)
     │
     ├──► Configure les espaces, zones, catégories, prix
     │
     ├──► Soumet pour vérification
     │
     └──► Publie l'événement validé
              │
              ▼
     PARTICIPANT
              │
              ├──► Découvre l'événement
              │
              ├──► Consulte les disponibilités
              │
              ├──► Crée une réservation (5 min)
              │
              ├──► Effectue le paiement (Mobile Money)
              │
              └──► Reçoit le billet (QR Code)
                        │
                        ▼
     JOUR DE L'ÉVÉNEMENT
                        │
                        ├──► Présente le billet
                        │
                        ├──► Scan du QR Code
                        │
                        └──► Accès autorisé, billet marqué USED
                                  │
                                  ▼
     APRÈS L'ÉVÉNEMENT
                                  │
                                  ├──► Clôture financière
                                  │
                                  ├──► Calcul du montant net
                                  │
                                  └──► Solde disponible pour retrait
```

---

# 13. Invariants du domaine

Les invariants suivants doivent être préservés par l'ensemble des domaines :

```text
INVARIANT 1 — Unicité de la disponibilité
─────────────────────────────────────────
Une disponibilité ne peut être attribuée qu'à un seul billet valide
à un instant donné.

INVARIANT 2 — Unicité du propriétaire actif
───────────────────────────────────────────
Un billet ne peut avoir qu'un seul propriétaire actif à un instant donné.

INVARIANT 3 — Idempotence du paiement
─────────────────────────────────────
Une même confirmation de paiement ne produit qu'un seul effet métier,
quelle que soit la fréquence de sa réception.

INVARIANT 4 — Idempotence du remboursement
──────────────────────────────────────────
Une même obligation de remboursement ne produit qu'un seul remboursement
effectif.

INVARIANT 5 — Unicité de la validation
──────────────────────────────────────
Un billet ne peut être validé avec succès qu'une seule fois
pour un même événement.

INVARIANT 6 — Intégrité du solde
────────────────────────────────
Un organisateur ne peut retirer un montant supérieur à son solde disponible.

INVARIANT 7 — Traçabilité des opérations
────────────────────────────────────────
Toute opération métier irréversible doit rester traçable
indépendamment des modifications ultérieures.

INVARIANT 8 — Séparation des cycles métier
──────────────────────────────────────────
L'état d'un cycle métier (Réservation, Paiement, Billet, Présence,
Règlement financier) ne peut pas être déduit automatiquement
de l'état d'un autre cycle.
```

---

# 14. Événements de domaine

Les événements de domaine suivants émergent naturellement de la vue d'ensemble. Ils seront détaillés dans `evenements-de-domaine.md`.

| Événement | Domaine source | Domaines concernés |
|---|---|---|
| `ÉvénementCréé` | Gestion des événements | Disponibilité, Confiance |
| `ÉvénementSoumis` | Gestion des événements | Confiance |
| `ÉvénementValidé` | Confiance | Gestion des événements |
| `ÉvénementPublié` | Gestion des événements | Découverte, Disponibilité |
| `ÉvénementAnnulé` | Gestion des événements | Billetterie, Remboursement, Distribution |
| `ÉvénementReporté` | Gestion des événements | Billetterie, Distribution |
| `RéservationCréée` | Réservation | Disponibilité, Paiement |
| `RéservationExpirée` | Réservation | Disponibilité |
| `PaiementConfirmé` | Paiement | Billetterie, Réservation |
| `PaiementÉchoué` | Paiement | Réservation |
| `PaiementTardifReçu` | Paiement | Réconciliation |
| `BilletÉmis` | Billetterie | Distribution, Contrôle d'accès |
| `BilletTransféré` | Billetterie | Billetterie (historique) |
| `BilletValidé` | Contrôle d'accès | Billetterie, Statistiques |
| `BilletRefusé` | Contrôle d'accès | Historique |
| `RemboursementRequis` | Annulation / Réconciliation | Remboursement |
| `RemboursementComplété` | Remboursement | Finance |
| `ClôtureFinancièreEffectuée` | Finance | Solde, Statistiques |
| `RetraitDemandé` | Finance | Service de paiement externe |
| `RetraitComplété` | Finance | Solde |
| `SignalementReçu` | Sécurité | Confiance |
| `OrganisationBannie` | Confiance | Gestion des événements, Billetterie |

---

# 15. Frontières du domaine

## 15.1 Ce qui est dans le domaine Eventix

- Découverte, création, gestion et publication d'événements.
- Configuration des espaces, zones, places et catégories de billets.
- Réservation temporaire et gestion des disponibilités.
- Paiement par Mobile Money.
- Émission, distribution et transfert de billets numériques.
- Contrôle d'accès avec QR Code.
- Annulation, report et remboursement.
- Gestion financière (clôture, solde, retrait).
- Statistiques de pilotage pour les organisateurs.
- Vérification, sécurité et signalement.
- Traçabilité des opérations importantes.

## 15.2 Ce qui est hors du domaine Eventix

- Organisation physique de l'événement (sonorisation, éclairage, restauration, sécurité sur place).
- Création de contenu événementiel (artistes, conférenciers, animateurs).
- Promotion externe (médias, réseaux sociaux, partenariats de visibilité).
- Gestion des lieux et infrastructures physiques.
- Autorités et réglementations (Eventix s'y conforme mais ne les modélise pas).
- Services de paiement eux-mêmes (Eventix intègre mais n'est pas un opérateur de paiement).
- Vente physique de billets (hors MVP).
- Marketplace de revente (hors MVP).
- Fonctionnalités sociales avancées (hors MVP).

---

# 16. Questions ouvertes

Certaines frontières de domaine dépendent de décisions non encore prises :

### 16.1 Incompatibilité lors d'un report

> **À PRÉCISER** — voir `03-decouverte-du-metier/questions-metier-ouvertes.md`
>
> Quelles règles métier s'appliquent lorsqu'un report rend la nouvelle configuration incompatible avec les billets existants ?
> Cette question impacte les frontières entre **Gestion des événements**, **Disponibilité** et **Billetterie**.

### 16.2 Modification après publication avec ventes en cours

> **À PRÉCISER** — voir `03-decouverte-du-metier/questions-metier-ouvertes.md`
>
> Quelles modifications d'un événement sont autorisées après publication et après début des ventes ?
> Cette question impacte la frontière entre **Gestion des événements** et **Disponibilité**.

### 16.3 Frais, commissions et taxes

> **À PRÉCISER** — voir `03-decouverte-du-metier/questions-metier-ouvertes.md`
>
> Quels sont les éléments financiers exacts déduits du montant brut pour obtenir le montant net organisateur ?
> Cette question impacte la frontière entre **Finance** et les règles commerciales externes.

### 16.4 Vente physique (hors MVP)

> La vente physique est hors MVP. Lorsqu'elle entrera dans le périmètre, un nouveau domaine **Vente physique**
> devra être défini avec ses propres frontières par rapport à **Disponibilité** et **Billetterie**.

---

# 17. Résumé

```text
┌─────────────────────────────────────────────────────────────────────┐
│                    VUE D'ENSEMBLE DU DOMAINE                        │
│                         EVENTIX — MVP                               │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  CORE DOMAIN (8)                                                    │
│  ───────────────                                                    │
│  Confiance · Événements · Disponibilité · Réservation               │
│  Paiement · Billetterie · Contrôle d'accès · Finance                │
│                                                                     │
│  SUPPORTING DOMAIN (5)                                              │
│  ─────────────────────                                              │
│  Annulation/Report · Remboursement · Sécurité/Signalement           │
│  Statistiques · Distribution                                        │
│                                                                     │
│  GENERIC DOMAIN (1)                                                 │
│  ────────────────                                                   │
│  Historique et traçabilité                                          │
│                                                                     │
│  TOTAL : 14 domaines                                                │
│                                                                     │
│  INVARIANTS : 8 invariants fonctionnels majeurs                     │
│  ÉVÉNEMENTS : 21 événements de domaine identifiés                   │
│  QUESTIONS OUVERTES : 4 frontières à préciser                       │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

# 18. Critères de qualité du document

La vue d'ensemble du domaine doit respecter les propriétés suivantes :

- une responsabilité clairement définie par domaine ;
- une catégorisation DDD explicite (Core / Supporting / Generic) ;
- un alignement sur le langage ubiquitaire ;
- une séparation des responsabilités sans fusion des cycles métier ;
- des frontières explicites entre ce qui est dans le domaine et ce qui est hors domaine ;
- des invariants du domaine clairement énoncés ;
- des événements de domaine identifiés sans anticipation technique ;
- des questions ouvertes explicitement conservées ;
- une traçabilité vers les sources de référence.

---

# 19. Statut

| Champ | Valeur |
|---|---|
| **Document** | `vue-d-ensemble-du-domaine.md` |
| **Version** | 1.0 |
| **Statut** | À valider par l'équipe |
| **Périmètre** | MVP Eventix |
| **Marché** | Cameroun |

| Principe | État |
|---|---|
| Langage ubiquitaire | ✅ DÉFINI |
| Distillation du domaine (Core/Supporting/Generic) | ✅ APPLIQUÉE |
| Séparation des responsabilités | ✅ APPLIQUÉE |
| Invariants du domaine | ✅ IDENTIFIÉS |
| Événements de domaine | 🔧 PRÉPARÉS (détail dans `evenements-de-domaine.md`) |
| Frontières du domaine | ✅ DÉFINIES |
| Questions ouvertes | 📌 EXPLICITEMENT CONSERVÉES |
| Bounded contexts | ⏳ NON DÉFINIS (voir `bounded-contexts.md`) |
| Agrégats | ⏳ NON DÉFINIS (voir `agregats.md`) |

---

## Notes de rédaction

### Alignement avec les exigences fonctionnelles

Cette vue d'ensemble du domaine est directement dérivée des 129 exigences fonctionnelles (`EF-001` à `EF-129`). Chaque domaine identifié ici regroupe les exigences qui partagent une responsabilité métier commune, sans anticiper l'architecture technique future.

### Distillation

La distillation en Core / Supporting / Generic suit le principe de DDD : le Core Domain représente ce qui différencie Eventix de la concurrence. Pour un marché comme le Cameroun, la confiance, la fiabilité du paiement Mobile Money et la gestion fine des disponibilités sont des différenciateurs clés.

### Questions ouvertes

Les frontières entre domaines ne sont pas figées. Les 4 questions ouvertes identifiées pourront modifier les frontières une fois tranchées. Le document sera mis à jour en conséquence.

### Incohérence documentaire connue

> ⚠️ La vente physique est hors MVP. Si elle entre dans le périmètre futur, un nouveau domaine devra être créé avec ses propres frontières.



# Vue d'ensemble du domaine — Eventix

| Champ | Valeur |
|---|---|
| **Phase** | 05 — Domain-Driven Design |
| **Projet** | Eventix |
| **Périmètre** | MVP |
| **Marché initial** | Cameroun |
| **Statut** | Version de référence — à valider par l'équipe |

---

## Table des matières

1. [Pourquoi le Domain-Driven Design ?](#1-pourquoi-le-domain-driven-design)
2. [Sources de référence](#2-sources-de-référence)
3. [Principes de conception](#3-principes-de-conception)
4. [Cartographie des domaines](#4-cartographie-des-domaines)
5. [Domaines et responsabilités](#5-domaines-et-responsabilités)
6. [Relations entre domaines](#6-relations-entre-domaines)
7. [Langage ubiquitaire](#7-langage-ubiquitaire)
8. [États métier consolidés](#8-états-métier-consolidés)
9. [Invariants métier](#9-invariants-métier)
10. [Cas particuliers métier](#10-cas-particuliers-métier)
11. [Ce que le DDD ne préjuge pas](#11-ce-que-le-ddd-ne-préjuge-pas)
12. [Résumé](#12-résumé)
13. [Critères de qualité du document](#13-critères-de-qualité-du-document)
14. [Statut](#14-statut)

---

# 1. Pourquoi le Domain-Driven Design ?

## 1.1. Le problème à résoudre

Eventix est une billetterie en ligne destinée au marché camerounais. Les phases précédentes ont produit :

- une cartographie de l'écosystème événementiel ;
- un ensemble d'exigences fonctionnelles identifiées, priorisées et organisées par responsabilités métier.

Ces livrables décrivent **ce que** le produit doit permettre. Ils ne décrivent pas encore **comment** le domaine métier doit être structuré pour être :

- compréhensible par tous les membres de l'équipe ;
- cohérent avec les règles métier validées ;
- évolutif sans régression ;
- indépendant des choix technologiques.

Sans un modèle de domaine explicite, le risque est élevé de produire un système dans lequel :

- les responsabilités métier se chevauchent ;
- les mêmes concepts sont nommés différemment selon les contextes ;
- les règles métier sont dispersées et difficiles à maintenir ;
- les évolutions futures nécessitent des modifications transversales coûteuses.

## 1.2. Pourquoi le DDD est pertinent pour Eventix

Le Domain-Driven Design (DDD) est une approche de conception logicielle qui place le **domaine métier** au centre de toutes les décisions. Elle est particulièrement pertinente pour Eventix pour les raisons suivantes.

### Raison 1 — Un domaine riche en règles métier

La billetterie événementielle n'est pas une simple application de catalogue. Elle implique des règles complexes :

- gestion des disponibilités et des capacités ;
- cycle de vie des réservations avec expiration ;
- idempotence des paiements ;
- unicité des billets et de leurs propriétaires ;
- contrôle d'accès avec mode dégradé ;
- clôture financière et retraits ;
- remboursements avec traçabilité.

Ces règles ne sont pas des détails d'implémentation. Elles constituent le **cœur du produit**. Le DDD fournit un cadre pour les exprimer explicitement et les faire respecter.

### Raison 2 — Une équipe qui doit partager un langage commun

Eventix implique des profils variés : produit, design, développement, opérations, finance. Sans un langage partagé, chaque profil risque de développer sa propre terminologie pour les mêmes concepts.

Le DDD introduit le concept de **langage ubiquitaire** : un vocabulaire métier commun, utilisé dans le code, la documentation, les conversations et les interfaces utilisateur. Cela réduit les malentendus et accélère la prise de décision.

### Raison 3 — Un produit destiné à évoluer

Le MVP est volontairement restreint. Les fonctionnalités futures identifiées — vente physique, marketplace de revente, fonctionnalités sociales — ne sont pas des ajouts triviaux. Elles toucheront des domaines existants et en créeront de nouveaux.

Un modèle de domaine bien conçu permet d'intégrer ces évolutions sans remettre en cause l'ensemble du système. Les frontières entre domaines sont claires, ce qui limite l'impact des modifications.

### Raison 4 — Un marché avec des contraintes spécifiques

Le marché camerounais présente des particularités qui influencent directement le domaine :

- Mobile Money comme moyen de paiement principal ;
- connectivité variable nécessitant un mode dégradé pour le contrôle ;
- environnement réglementaire spécifique ;
- habitudes de consommation locales.

Ces contraintes ne sont pas des exceptions à traiter ponctuellement. Elles doivent être intégrées dans le modèle de domaine dès la conception. Le DDD permet de les traiter comme des règles métier de première classe.

### Raison 5 — Une traçabilité exigée

Les exigences fonctionnelles identifiées dans la phase précédente exigent une traçabilité forte : chaque opération métier importante doit être tracée, chaque décision de sécurité doit être justifiée, chaque remboursement doit être suivi.

Le DDD facilite cette traçabilité en rendant explicites les agrégats, les événements métier et les invariants. Le modèle de domaine devient la référence pour comprendre ce qui s'est passé et pourquoi.

## 1.3. Ce que le DDD apporte concrètement à Eventix

| Apport | Bénéfice pour Eventix |
|---|---|
| Langage ubiquitaire | Réduction des malentendus entre équipe produit, design et développement |
| Frontières de domaine explicites | Modifications localisées, impact limité des évolutions |
| Modèle de domaine riche | Règles métier centralisées et vérifiables |
| Agrégats et invariants | Cohérence garantie des données métier |
| Événements métier | Traçabilité et réactivité du système |
| Contextes bornés | Séparation claire des responsabilités, faible couplage |

## 1.4. Ce que le DDD n'est pas

Le DDD n'est pas :

- une architecture technique imposée ;
- une obligation d'utiliser des microservices ;
- une méthodologie de développement ;
- un framework ou une bibliothèque.

C'est une **approche de conception** qui guide la structuration du domaine métier. Les décisions d'architecture technique restent ouvertes et seront traitées dans les phases ultérieures.

---

# 2. Sources de référence

Ce document est dérivé principalement de :

- `03-decouverte-du-metier/ecosysteme-eventix.md`
- `04-analyse-des-besoins/exigences-fonctionnelles.md`

Les règles métier, les états métier et les invariants fonctionnels définis dans ces documents constituent la base sur laquelle le modèle de domaine est construit.

---

# 3. Principes de conception

## 3.1. Le domaine métier comme centre de gravité

Toute décision de conception doit être justifiée par une règle métier, un besoin métier ou une contrainte métier. Les choix techniques ne doivent pas influencer la structure du domaine.

## 3.2. Un langage ubiquitaire

Chaque concept métier possède un nom unique et partagé. Ce nom est utilisé de manière cohérente dans :

- le code ;
- la documentation ;
- les conversations ;
- les interfaces utilisateur.

Les synonymes et les ambiguïtés sont proscrits.

## 3.3. Des frontières de domaine explicites

Chaque domaine possède une responsabilité clairement délimitée. Les responsabilités ne se chevauchent pas. Les collaborations entre domaines sont explicites et minimales.

## 3.4. Des invariants préservés

Les invariants métier identifiés dans les phases précédentes doivent être préservés par le modèle de domaine. Toute violation d'un invariant constitue un bug métier, pas seulement un bug technique.

## 3.5. Des états métier explicites

Les objets métier possèdent des états explicites et vérifiables. Les transitions entre états sont contrôlées et traçables.

---

# 4. Cartographie des domaines

La cartographie suivante identifie les domaines métier d'Eventix et leurs relations. Elle constitue la base pour la définition détaillée de chaque domaine dans les documents ultérieurs de cette phase.

```text
                        ┌─────────────────┐
                        │    IDENTITY     │
                        │  (Comptes,      │
                        │   Organisations)│
                        └────────┬────────┘
                                 │
                                 ▼
                        ┌─────────────────┐
                        │    CATALOG      │
                        │  (Événements,   │
                        │   Espaces,      │
                        │   Catégories)   │
                        └────────┬────────┘
                                 │
            ┌────────────────────┼────────────────────┐
            │                    │                    │
            ▼                    ▼                    ▼
    ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
    │  DISCOVERY   │     │  BOOKING     │     │  TRUST &     │
    │  (Recherche, │     │  (Réservation│     │  SAFETY      │
    │   Filtrage,  │     │   Disponibilité)   │  (Signalement│
    │   Détail)    │     └──────┬───────┘     │   Bannissement)│
    └──────────────┘            │             └──────────────┘
                                │
                                ▼
                        ┌──────────────┐
                        │   PAYMENT    │
                        │  (Paiement,  │
                        │   Réconciliation)│
                        └──────┬───────┘
                               │
                               ▼
                        ┌──────────────┐
                        │   TICKETING  │
                        │  (Achat,     │
                        │   Billet,    │
                        │   Transfert) │
                        └──────┬───────┘
                               │
                               ▼
                        ┌──────────────┐
                        │   ACCESS     │
                        │  (Contrôle,  │
                        │   Présence)  │
                        └──────┬───────┘
                               │
                               ▼
                        ┌──────────────┐
                        │   FINANCE    │
                        │  (Clôture,   │
                        │   Solde,     │
                        │   Retrait)   │
                        └──────┬───────┘
                               │
                               ▼
                        ┌──────────────┐
                        │  OBSERVATION │
                        │  (Statistiques│
                        │   Historique)│
                        └──────────────┘

5. Domaines et responsabilités
5.1. IDENTITY — Identité et comptes

Responsabilité : Gérer les comptes utilisateurs, les organisations et leurs capacités.

Concepts clés :

Utilisateur

Compte participant

Compte organisateur

Organisation

Capacité

Règles métier principales :

Un compte peut exercer les capacités de participant et d'organisateur.

Une organisation peut être soumise à vérification, suspension ou bannissement.

Les informations de sécurité et de fraude ne sont pas exposées aux utilisateurs.

Exigences associées : EF-001 à EF-007, EF-014, EF-095 à EF-098.

5.2. CATALOG — Catalogue et configuration

Responsabilité : Gérer le cycle de vie des événements et leur configuration.

Concepts clés :

Événement

Brouillon

Soumission

Vérification

Publication

Espace

Zone

Capacité

Catégorie de billet

Règles métier principales :

Un événement commence dans un état ne permettant pas sa publication.

La publication est conditionnée à une vérification positive.

Les capacités ne peuvent pas être réduites en dessous des billets déjà attribués.

Exigences associées : EF-008 à EF-024.

5.3. DISCOVERY — Découverte

Responsabilité : Permettre aux participants de découvrir et consulter les événements.

Concepts clés :

Recherche

Filtre

Détail événement

Disponibilité visible

Règles métier principales :

La découverte ne nécessite pas de compte.

La disponibilité affichée tient compte des billets vendus et des réservations en cours.

Exigences associées : EF-025 à EF-029.

5.4. BOOKING — Réservation et disponibilité

Responsabilité : Gérer les réservations temporaires et la disponibilité des billets.

Concepts clés :

Réservation

Disponibilité

Blocage temporaire

Expiration

Règles métier principales :

Une réservation bloque temporairement une disponibilité.

Une réservation expire après cinq minutes.

L'expiration libère la disponibilité sans constituer un échec de paiement.

Exigences associées : EF-030 à EF-034.

5.5. PAYMENT — Paiement

Responsabilité : Gérer les opérations de paiement et leur réconciliation.

Concepts clés :

Paiement

Confirmation

Échec

Réconciliation

Mobile Money

Règles métier principales :

Le MVP supporte Mobile Money.

Une confirmation de paiement reçue plusieurs fois ne produit qu'un seul effet métier.

Un paiement tardif déclenche une réconciliation.

Exigences associées : EF-035 à EF-041.

5.6. TICKETING — Billetterie

Responsabilité : Gérer les achats, l'émission des billets et leur transfert.

Concepts clés :

Achat

Billet

QR Code

Propriétaire

Transfert

Règles métier principales :

Un billet est émis lorsqu'un achat est définitivement finalisé.

Un billet a un propriétaire actif unique.

Le transfert remplace le propriétaire sans créer de copie.

Le MVP ne permet pas la revente.

Exigences associées : EF-042 à EF-056.

5.7. ACCESS — Contrôle d'accès

Responsabilité : Gérer le contrôle des billets à l'entrée des événements.

Concepts clés :

Scan

Validation

Billet utilisé

Mode dégradé

Présence

Règles métier principales :

Un billet ne peut être validé qu'une seule fois.

Le mode dégradé privilégie la fiabilité sur le débit.

Le contexte de chaque contrôle est enregistré.

Exigences associées : EF-057 à EF-071.

5.8. FINANCE — Finance

Responsabilité : Gérer la clôture financière, les soldes et les retraits.

Concepts clés :

Clôture

Montant net

Solde

Retrait

Règles métier principales :

Le montant net est calculé à partir des ventes confirmées moins les remboursements et frais.

Un retrait ne peut excéder le solde disponible.

Un retrait échoué est restitué au solde.

Exigences associées : EF-105 à EF-114.

5.9. REFUND — Remboursement

Responsabilité : Gérer les remboursements et leur exécution.

Concepts clés :

Obligation de remboursement

Montant de référence

État du remboursement

Règles métier principales :

Le montant de référence correspond au montant effectivement payé.

Une obligation de remboursement ne produit qu'un seul remboursement effectif.

Le changement d'avis ne déclenche pas automatiquement un remboursement.

Exigences associées : EF-076, EF-080 à EF-087.

5.10. TRUST & SAFETY — Confiance et sécurité

Responsabilité : Gérer les signalements, l'analyse des risques et les mesures de sécurité.

Concepts clés :

Signalement

Analyse de risque

Suspension

Bannissement

Règles métier principales :

Un signalement n'est pas une preuve de fraude.

Les décisions de sécurité sont traçables.

Le bannissement d'une organisation n'entraîne pas automatiquement la suppression de ses événements.

Exigences associées : EF-088 à EF-098.

5.11. OBSERVATION — Observation et traçabilité

Responsabilité : Produire les statistiques et maintenir l'historique des opérations métier.

Concepts clés :

Statistique

Historique

Traçabilité

Événement métier

Règles métier principales :

Les statistiques ne modifient pas les données métier.

Les opérations importantes sont tracées avec leur contexte.

L'historique n'est pas réécrit par les modifications ultérieures.

Exigences associées : EF-099 à EF-104, EF-115 à EF-117.

5.12. COMMUNICATION — Communication

Responsabilité : Gérer la distribution des billets et les communications aux participants.

Concepts clés :

Mise à disposition

Email

Notification

Règles métier principales :

Le billet est accessible depuis le compte du participant.

Le billet peut être distribué par email.

Les communications importantes (annulation, report) sont envoyées aux participants concernés.

Exigences associées : EF-050, EF-075, EF-121 à EF-123.

6. Relations entre domaines
6.1. Chaîne métier principale
IDENTITY
    ↓
CATALOG
    ↓
DISCOVERY
    ↓
BOOKING
    ↓
PAYMENT
    ↓
TICKETING
    ↓
ACCESS
    ↓
FINANCE


Cette chaîne représente le parcours métier principal : un organisateur crée un événement, un participant le découvre, réserve, paie, reçoit un billet, le présente à l'entrée, et l'organisateur perçoit les fonds.

6.2. Domaines transversaux
TRUST & SAFETY ───────► CATALOG, IDENTITY
REFUND ───────────────► PAYMENT, TICKETING, FINANCE
OBSERVATION ──────────► Tous les domaines
COMMUNICATION ────────► TICKETING, CATALOG


Ces domaines interviennent à plusieurs étapes du parcours principal sans en faire partie intégrante.

6.3. Règles de collaboration

Un domaine ne modifie pas directement les données d'un autre domaine.

Les collaborations s'effectuent à partir de résultats métier explicites.

Les dépendances sont unidirectionnelles autant que possible.

Les événements métier permettent la communication asynchrone entre domaines.

7. Langage ubiquitaire

Le tableau suivant définit les termes métier standardisés utilisés dans tous les documents et le code.

Terme	Définition	À ne pas confondre avec
Événement	Manifestation organisée proposée à un public	Événement système, événement métier
Organisateur	Compte utilisateur autorisé à créer et gérer des événements	Organisation, Prestataire
Participant	Personne qui achète ou obtient un billet pour un événement	Visiteur, Client
Réservation	Blocage temporaire d'une disponibilité avant achat	Achat, Billet
Disponibilité	Capacité d'accueil encore attribuable pour un événement	Capacité totale
Paiement	Opération financière initiée par un participant	Achat, Règlement
Achat	Transaction confirmée donnant droit à un billet	Paiement, Réservation
Billet	Titre d'accès émis à la suite d'un achat	Réservation, QR Code
QR Code	Mécanisme d'identification et de contrôle du billet	Billet lui-même
Transfert	Changement de propriétaire actif d'un billet	Revente, Copie
Contrôle	Vérification du billet à l'entrée d'un événement	Validation technique
Présence	Statut d'un participant ayant effectivement accédé à l'événement	Billet vendu
Clôture	Opération de détermination du montant net dû à l'organisateur	Retrait, Paiement
Solde	Montant disponible pour retrait par l'organisateur	Chiffre d'affaires
Retrait	Opération de versement du solde à l'organisateur	Remboursement
Remboursement	Restitution du montant payé à un participant	Retrait, Annulation
Annulation	Arrêt définitif d'un événement	Report, Suspension
Report	Déplacement d'un événement à une nouvelle date	Annulation
Signalement	Alerte émise par un participant sur un événement ou organisateur	Preuve de fraude
Bannissement	Mesure d'exclusion d'une organisation	Suspension, Annulation
8. États métier consolidés

Les états métier définis dans les exigences fonctionnelles sont repris ici comme référence pour le modèle de domaine.

Objet métier	États
Événement	DRAFT, SUBMITTED, UNDER_REVIEW, VALIDATED, PUBLISHED, ONGOING, COMPLETED, CLOSED, ARCHIVED, CANCELLED
Réservation	PENDING, CONFIRMED, EXPIRED, CANCELLED
Paiement	PENDING, CONFIRMED, FAILED
Billet	ISSUED, USED, CANCELLED
Remboursement	PENDING, PROCESSING, FAILED, COMPLETED
Retrait	PENDING, PROCESSING, FAILED, COMPLETED
Organisation	ACTIVE, UNDER_REVIEW, SUSPENDED, BANNED
9. Invariants métier

Les invariants suivants doivent être préservés par le modèle de domaine dans toutes les circonstances.

9.1. Unicité de l'attribution
Une disponibilité
        ↓
Une attribution
        ↓
Un billet
        ↓
Un propriétaire actif

9.2. Idempotence du paiement
Une confirmation de paiement
        ↓
Un seul effet métier

9.3. Unicité du remboursement
Une obligation de remboursement
        ↓
Un seul remboursement effectif

9.4. Unicité de la validation
Un billet validé
        ↓
USED
        ↓
Aucune seconde validation réussie

9.5. Limitation des retraits
Un organisateur
        ↓
Ne peut retirer
        ↓
Plus que son solde disponible

9.6. Traçabilité des opérations
Une opération métier réalisée
        ↓
Reste traçable

9.7. Protection de la capacité attribuée
Une capacité
        ↓
Ne peut être réduite
        ↓
En dessous des billets déjà attribués

10. Cas particuliers métier
10.1. Réconciliation

La réconciliation constitue un cas métier spécifique qui traverse les domaines BOOKING, PAYMENT et REFUND.

Réservation PENDING
        ↓
5 minutes → EXPIRED
        ↓
Paiement confirmé tardivement
        ↓
Réconciliation
       │
       ├── Disponibilité disponible → Billet émis
       │
       └── Disponibilité indisponible → Remboursement


Ce cas illustre la nécessité de frontières claires : le domaine PAYMENT fournit le résultat, les domaines BOOKING et REFUND décident de la suite.

10.2. Contrôle dégradé

Le contrôle dégradé constitue un cas métier spécifique du domaine ACCESS.

État partagé fiable
        ↓
Plusieurs scanners actifs
        ↓
Contrôles parallèles

État partagé non fiable
        ↓
Mode dégradé
        ↓
Un seul scanner actif
        ↓
Contrôles séquentiels


Ce cas illustre un choix métier explicite : la fiabilité prime sur le débit lorsque la cohérence ne peut plus être garantie.

11. Ce que le DDD ne préjuge pas

Le modèle de domaine défini dans ce document ne préjuge pas :

de l'architecture technique (monolithe, microservices, etc.) ;

des technologies de développement ;

des bases de données ;

des frameworks ;

des API ;

de l'infrastructure ;

des mécanismes de déploiement.

Ces éléments seront traités dans les phases ultérieures du projet.

12. Résumé

Le Domain-Driven Design est retenu pour Eventix parce qu'il fournit un cadre adapté à :

la complexité des règles métier de la billetterie événementielle ;

la nécessité d'un langage partagé entre tous les membres de l'équipe ;

l'évolutivité du produit au-delà du MVP ;

l'intégration des contraintes spécifiques du marché camerounais ;

l'exigence de traçabilité des opérations métier.

Le modèle de domaine présenté dans ce document identifie douze domaines métier, leurs responsabilités, leurs relations et les invariants qu'ils doivent préserver.

Il constitue la fondation pour les documents détaillés de cette phase :

définition des agrégats ;

définition des entités ;

définition des objets de valeur ;

définition des événements métier.

13. Critères de qualité du document

Ce document doit respecter les propriétés suivantes :

chaque domaine possède une responsabilité unique et clairement délimitée ;

les frontières entre domaines sont explicites ;

le langage ubiquitaire est cohérent avec les documents des phases précédentes ;

les invariants métier sont identifiés et vérifiables ;

les états métier sont consolidés et sans ambiguïté ;

les cas particuliers métier sont explicitement traités ;

aucune décision technique n'est prise ou implicite."                   