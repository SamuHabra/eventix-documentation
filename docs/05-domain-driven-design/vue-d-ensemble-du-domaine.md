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