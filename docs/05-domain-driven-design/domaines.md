
# Domaines métier — Eventix

## 1. Objet du document

Ce document identifie et structure les **grands domaines métier d'Eventix**.

L'objectif est de répondre à la question :

> **Quelles sont les grandes responsabilités métier qui composent Eventix, et comment peuvent-elles être regroupées de manière cohérente ?**

Cette décomposition constitue une étape stratégique du Domain-Driven Design (DDD).

Elle servira notamment de base à :

* l'identification des sous-domaines ;
* l'identification du **Core Domain** ;
* la définition des **Bounded Contexts** ;
* la construction de la **Context Map** ;
* la définition du langage ubiquitaire ;
* la conception future de l'architecture logicielle.

### Important

Les domaines identifiés dans ce document sont des **domaines métier candidats**.

Ils ne constituent pas encore nécessairement des Bounded Contexts techniques.

Un domaine métier peut éventuellement :

* correspondre à un seul Bounded Context ;
* être réparti entre plusieurs Bounded Contexts ;
* être regroupé avec une autre partie du métier après analyse plus approfondie.

---

# 2. Rappel de la vision métier d'Eventix

Eventix est une plateforme numérique de gestion et de vente de billets pour des événements.

Sa chaîne métier principale peut être représentée ainsi :

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

Cette chaîne représente le parcours central de la valeur :

> permettre à un organisateur de proposer un événement, de vendre des accès, de délivrer des billets et de contrôler l'accès des participants.

Elle permet également de constater que plusieurs concepts doivent rester distincts.

Par exemple :

```text
Réservation ≠ Paiement ≠ Billet
```

Une réservation peut exister avant le paiement.

Un paiement peut être confirmé sans que le traitement métier soit terminé.

Un billet représente ensuite le droit d'accès qui résulte du processus métier.

Ces distinctions sont importantes pour éviter un modèle métier artificiellement simplifié.

---

# 3. Méthode de décomposition

Les domaines ont été identifiés en croisant plusieurs dimensions.

## 3.1 Fonctionnalités

Quelles sont les grandes choses qu'Eventix doit permettre de faire ?

Exemples :

* créer un événement ;
* configurer une offre ;
* vendre des billets ;
* recevoir un paiement ;
* délivrer un billet ;
* contrôler un billet ;
* suivre les participants.

---

## 3.2 Capacités métier

Quelle capacité durable l'entreprise doit-elle posséder ?

Exemples :

* gérer ses événements ;
* vendre des accès ;
* gérer les transactions ;
* contrôler les accès ;
* gérer ses participants.

La capacité métier est plus stable qu'une fonctionnalité.

---

## 3.3 Processus métier

Comment la valeur circule-t-elle dans le système ?

Exemple :

```text
Créer événement
      ↓
Configurer offre
      ↓
Publier
      ↓
Réserver
      ↓
Payer
      ↓
Émettre billet
      ↓
Distribuer billet
      ↓
Scanner billet
      ↓
Autoriser accès
```

---

## 3.4 Règles métier

La décomposition tient également compte des règles déjà identifiées dans l'analyse métier.

Exemples :

* expiration d'une réservation en attente ;
* confirmation idempotente d'un paiement ;
* modification future des prix ;
* transfert d'un billet lors d'un changement de date ;
* retrait organisateur après l'événement ;
* validation d'un billet lors de l'accès.

---

## 3.5 Complexité métier

Certaines parties du métier nécessitent davantage de logique et de protection contre les erreurs.

Exemples :

* concurrence sur les places disponibles ;
* expiration des réservations ;
* confirmation tardive d'un paiement ;
* doublons de paiement ;
* validation d'un même billet ;
* fonctionnement du contrôle avec une connectivité limitée.

---

## 3.6 Valeur stratégique

Enfin, les domaines sont analysés selon leur importance pour la proposition de valeur d'Eventix.

L'objectif n'est donc pas seulement de demander :

> « Que fait le logiciel ? »

mais également :

> « Où Eventix crée-t-il sa valeur et où se trouve sa complexité métier différenciante ? »

---

# 4. Vue d'ensemble des domaines

La décomposition candidate retenue est la suivante :

| Domaine                      | Responsabilité principale                                             | Classification initiale |
| ---------------------------- | --------------------------------------------------------------------- | ----------------------- |
| **Gestion des événements**   | Concevoir, configurer et publier les événements                       | Supporting              |
| **Billetterie**              | Construire l'offre, gérer les disponibilités, réservations et billets | **Core**                |
| **Transactions**             | Gérer les opérations financières liées aux ventes                     | **Core**                |
| **Contrôle & accès**         | Vérifier les billets et gérer l'accès à l'événement                   | **Core**                |
| **Participants & suivi**     | Gérer les participants et leur participation                          | Supporting              |
| **Identité & communication** | Gérer les identités, rôles, accès et communications                   | Generic / Supporting    |
| **Statistiques & pilotage**  | Produire une vision de suivi et de pilotage                           | Transversal             |

Cette classification est **préliminaire**.

Elle pourra évoluer lors de l'analyse des sous-domaines et des Bounded Contexts.

---

# 5. Domaine 1 — Gestion des événements

## 5.1 Responsabilité

Ce domaine concerne la capacité d'Eventix à permettre à un organisateur de :

* créer un événement ;
* définir ses informations ;
* le configurer ;
* préparer son offre ;
* le publier ;
* suivre son état.

Il représente le **contexte métier de l'événement** sur lequel viennent se greffer les autres capacités.

---

## 5.2 Capacités principales

```text
Gestion des événements
├── Création d'événement
├── Configuration
├── Informations événement
├── Paramétrage
├── Publication
├── Modification
└── Suivi de l'état de l'événement
```

---

## 5.3 Concepts métier candidats

* Événement
* Organisateur
* Lieu
* Date
* Heure
* Statut de l'événement
* Configuration
* Publication

Ces concepts devront être précisés dans le futur langage ubiquitaire.

---

## 5.4 Responsabilités

Le domaine doit notamment déterminer :

* ce qu'est un événement ;
* dans quel état se trouve un événement ;
* quand un événement peut être publié ;
* quelles informations sont nécessaires à sa publication ;
* quelles modifications sont autorisées selon son état.

---

## 5.5 Complexité

La complexité est principalement liée aux règles de cycle de vie.

Exemple :

```text
Brouillon
   ↓
Configuré
   ↓
Publié
   ↓
En cours
   ↓
Terminé
```

Les états exacts restent à confirmer dans l'analyse métier.

---

## 5.6 Valeur stratégique

La gestion des événements est indispensable à Eventix, mais elle ne constitue pas nécessairement la principale différenciation stratégique de la plateforme.

Elle est donc classée provisoirement :

> **Supporting Domain**

---

## 5.7 Frontière candidate

Ce domaine doit rester centré sur :

> **l'existence et la configuration de l'événement**

Il ne doit pas absorber automatiquement :

* le paiement ;
* la réservation ;
* le contrôle des billets ;
* les comptes utilisateurs.

---

# 6. Domaine 2 — Billetterie

## 6.1 Responsabilité

La billetterie constitue l'un des domaines centraux d'Eventix.

Elle concerne la capacité à transformer un événement en **offre d'accès vendable**, puis à gérer le processus permettant d'obtenir un billet.

---

## 6.2 Capacités principales

```text
Billetterie
├── Définition des offres
├── Types de billets
├── Tarification
├── Disponibilité
├── Réservation
├── Expiration des réservations
├── Émission des billets
└── Gestion du statut des billets
```

---

## 6.3 Concepts métier candidats

* Offre
* Type de billet
* Tarif
* Quota
* Disponibilité
* Réservation
* Billet
* Statut du billet
* Identifiant du billet
* QR code

---

## 6.4 Responsabilité métier fondamentale

Le domaine doit pouvoir répondre à des questions comme :

> Combien de billets sont disponibles ?

> Quel type de billet peut être acheté ?

> Quel prix s'applique à une nouvelle vente ?

> Une réservation est-elle encore valide ?

> Quand un billet peut-il être émis ?

> Quel est l'état actuel d'un billet ?

---

## 6.5 Règle importante : réservation et billet

Une réservation ne doit pas être assimilée à un billet.

```text
Réservation
    ↓
Paiement
    ↓
Émission
    ↓
Billet
```

La réservation représente une **intention temporaire de réserver une capacité**.

Le billet représente un **droit d'accès délivré**.

---

## 6.6 Problème de concurrence

La disponibilité est un point de complexité majeur.

Exemple :

```text
10 places disponibles

Participant A → demande 6 places
Participant B → demande 6 places
```

Le domaine doit empêcher que les deux opérations consomment simultanément les mêmes capacités.

Cela fait de la billetterie une zone fortement sensible aux problèmes de concurrence.

---

## 6.7 Réservation temporaire

Une réservation en attente doit avoir une durée limitée.

Règle actuellement retenue :

> Une réservation en attente expire après **5 minutes**.

Cette règle devra être intégrée au modèle métier et non simplement considérée comme une minuterie technique.

---

## 6.8 Modification du prix

Une modification de prix ne doit pas nécessairement modifier les ventes déjà effectuées.

Règle retenue :

> Un nouveau prix s'applique aux ventes futures.

Les ventes déjà réalisées conservent leurs conditions historiques.

---

## 6.9 Changement de date

Lorsqu'un événement change de date, la règle métier actuellement retenue prévoit :

> Le billet est automatiquement transféré vers la nouvelle date par défaut.

Les exceptions et mécanismes de remboursement restent à préciser.

---

## 6.10 Valeur stratégique

La billetterie constitue une partie fondamentale de la proposition de valeur d'Eventix.

Elle contient également une forte complexité métier.

Classification provisoire :

> **Core Domain**

---

# 7. Domaine 3 — Transactions

## 7.1 Responsabilité

Le domaine Transactions concerne la gestion du cycle financier associé aux opérations commerciales d'Eventix.

Il doit notamment permettre de gérer :

* l'initiation du paiement ;
* la réception de la confirmation ;
* l'état de la transaction ;
* les erreurs ;
* les remboursements ;
* les règlements aux organisateurs.

---

## 7.2 Capacités principales

```text
Transactions
├── Initiation du paiement
├── Suivi du paiement
├── Confirmation
├── Gestion des états
├── Idempotence
├── Remboursement
├── Règlement
└── Retrait organisateur
```

---

## 7.3 Concepts métier candidats

* Transaction
* Paiement
* Confirmation
* Référence de transaction
* Statut de transaction
* Remboursement
* Règlement
* Organisateur bénéficiaire

---

## 7.4 Séparation réservation / transaction

La séparation suivante est fondamentale :

```text
Réservation
      │
      │ déclenche
      ▼
Transaction
      │
      │ confirme
      ▼
Billetterie
```

Une réservation n'est donc pas un paiement.

Cette séparation permettra notamment de gérer les situations où :

* le paiement échoue ;
* le paiement arrive en retard ;
* le paiement est confirmé deux fois ;
* le paiement est confirmé alors que la réservation a expiré ;
* le système reçoit plusieurs notifications pour la même transaction.

---

## 7.5 Idempotence

Une règle importante déjà identifiée :

> Les confirmations de paiement doivent être traitées de manière idempotente.

Exemple :

```text
Confirmation #1 → paiement confirmé
Confirmation #2 → aucune seconde opération financière
Confirmation #3 → aucune duplication
```

Le domaine doit donc posséder une notion fiable d'identité de transaction.

---

## 7.6 Remboursements

Les remboursements ne doivent pas être confondus avec l'annulation d'un billet.

Il faudra distinguer :

```text
Annulation métier
       ↓
Décision de remboursement
       ↓
Opération financière
```

Les règles précises restent à approfondir.

---

## 7.7 Retrait organisateur

Les organisateurs ne doivent pas nécessairement pouvoir retirer immédiatement les fonds issus des ventes.

Règle retenue :

> Le retrait organisateur intervient après la fin de l'événement.

Cette règle constitue une contrainte métier importante.

---

## 7.8 Valeur stratégique

La transaction constitue une partie critique de la chaîne de valeur et présente une complexité importante.

Classification provisoire :

> **Core Domain**

---

# 8. Domaine 4 — Contrôle & accès

## 8.1 Responsabilité

Ce domaine concerne la transformation du billet en **accès réel à l'événement**.

Il comprend notamment :

* le scan ;
* la vérification ;
* la validation ;
* la détection d'un billet déjà utilisé ;
* l'autorisation ou le refus d'accès.

---

## 8.2 Capacités principales

```text
Contrôle & accès
├── Scan
├── Lecture du billet
├── Vérification
├── Validation
├── Détection des doublons
├── Autorisation d'accès
└── Suivi du contrôle
```

---

## 8.3 Concepts métier candidats

* Contrôle
* Billet
* Validation
* Accès
* Scanner
* Statut de contrôle
* Point d'entrée
* Passage

---

## 8.4 Exemple de processus

```text
Scanner
   ↓
Lire QR code
   ↓
Identifier billet
   ↓
Vérifier validité
   ↓
Vérifier utilisation
   ↓
Valider
   ↓
Autoriser / Refuser l'accès
```

---

## 8.5 Problème de double utilisation

Un billet valide ne doit normalement pas permettre plusieurs entrées lorsque le modèle d'accès prévoit une entrée unique.

Exemple :

```text
Premier scan
→ VALIDE
→ accès autorisé

Deuxième scan
→ DÉJÀ UTILISÉ
→ accès refusé
```

Cette règle devra être explicitement modélisée.

---

## 8.6 Contrainte de connectivité

Le contrôle est particulièrement sensible à la connectivité réseau.

Pour le MVP, une seule unité de scan est actuellement privilégiée afin de réduire la complexité et d'améliorer la fiabilité opérationnelle.

La stratégie exacte de synchronisation et de fonctionnement hors ligne devra être étudiée ultérieurement.

---

## 8.7 Valeur stratégique

Le contrôle constitue une partie importante de l'expérience réelle d'Eventix.

Une plateforme qui vend correctement un billet mais échoue à l'entrée produit une rupture majeure dans la chaîne de valeur.

Classification provisoire :

> **Core Domain**

---

# 9. Domaine 5 — Participants & suivi

## 9.1 Responsabilité

Ce domaine concerne les personnes participant aux événements et les informations associées à leur participation.

---

## 9.2 Capacités principales

```text
Participants & suivi
├── Identification du participant
├── Recherche
├── Consultation
├── Association à un événement
├── Suivi de participation
└── Historique
```

---

## 9.3 Concepts métier candidats

* Participant
* Participation
* Événement
* Billet
* Historique de participation

---

## 9.4 Distinction importante

Un utilisateur Eventix n'est pas nécessairement un participant.

Par exemple :

```text
Utilisateur
   ↓
achète un billet
   ↓
devient participant à un événement
```

Il faut donc éviter de fusionner automatiquement :

> **Utilisateur = Participant**

Le participant est une notion métier liée à un événement.

---

## 9.5 Valeur stratégique

La gestion des participants est importante pour les organisateurs mais constitue principalement une capacité complémentaire à la chaîne centrale.

Classification provisoire :

> **Supporting Domain**

---

# 10. Domaine 6 — Identité & communication

Ce domaine regroupe provisoirement deux capacités fortement transversales :

* l'identité et les accès ;
* la communication avec les utilisateurs.

Cette association devra être réévaluée lors de l'identification des Bounded Contexts.

---

## 10.1 Identité & accès

### Responsabilité

Gérer :

* les comptes ;
* l'authentification ;
* les rôles ;
* les permissions ;
* les accès aux fonctionnalités ;
* l'identification des organisateurs.

### Concepts candidats

* Utilisateur
* Compte
* Rôle
* Permission
* Session
* Organisateur
* Vérification

### Cas particulier : vérification organisateur

Eventix prévoit une vérification des organisateurs.

Cette capacité est différente d'une simple authentification.

```text
Authentification
→ Qui es-tu ?

Autorisation
→ Que peux-tu faire ?

Vérification organisateur
→ Es-tu réellement autorisé à agir comme organisateur ?
```

Cette distinction devra être conservée dans le modèle.

---

## 10.2 Communication

### Responsabilité

Transmettre les informations nécessaires aux utilisateurs.

Exemples :

* confirmation ;
* billet ;
* notification ;
* information sur l'événement ;
* changement d'information.

### MVP

Le canal actuellement retenu pour la livraison du billet est :

> **Email**

WhatsApp n'est pas inclus dans le MVP.

---

## 10.3 Classification

L'identité et les communications sont généralement des capacités relativement génériques.

Classification provisoire :

> **Generic / Supporting**

Une analyse ultérieure déterminera si elles doivent être séparées en deux domaines distincts.

---

# 11. Statistiques & pilotage

## 11.1 Positionnement

Les statistiques constituent une capacité importante d'Eventix.

Cependant, elles sont différentes des domaines transactionnels précédents.

Elles **consomment principalement des informations produites par d'autres domaines**.

Exemple :

```text
Billetterie ───────┐
Transactions ──────┤
Contrôle ──────────┼──→ Statistiques
Participants ──────┘
```

---

## 11.2 Capacités

* suivi des ventes ;
* suivi des billets ;
* suivi des participants ;
* fréquentation ;
* statistiques événement ;
* indicateurs organisateur.

---

## 11.3 Pourquoi ne pas en faire immédiatement un domaine Core ?

Parce que les statistiques ne constituent pas directement le processus principal de création de valeur :

```text
Vendre
→ délivrer
→ contrôler
→ accéder
```

Les statistiques donnent une **vision** de ces opérations.

Elles peuvent néanmoins devenir stratégiquement importantes pour Eventix, notamment si la plateforme développe des capacités avancées d'analyse.

---

## 11.4 Position retenue

Pour l'instant :

> **Capacité transversale de pilotage**

Elle sera réévaluée lors de la modélisation des sous-domaines.

---

# 12. Synthèse des responsabilités

```text
                         EVENTIX
                            │
        ┌───────────────────┼────────────────────┐
        │                   │                    │
        ▼                   ▼                    ▼
 Gestion événement      Billetterie        Transactions
        │                   │                    │
        │                   ▼                    │
        │              Réservation               │
        │                   │                    │
        │                   └───────┬────────────┘
        │                           ▼
        │                         Billet
        │                           │
        │                           ▼
        │                    Contrôle & accès
        │                           │
        │                           ▼
        │                         Accès
        │
        ├──────────────→ Participants & suivi
        │
        ├──────────────→ Identité & communication
        │
        └──────────────→ Statistiques & pilotage
```

Cette représentation montre que les domaines ne sont pas indépendants.

Ils participent à une chaîne métier commune tout en possédant des responsabilités différentes.

---

# 13. Analyse Core / Supporting / Generic

## 13.1 Core Domain

Les candidats principaux sont :

### Billetterie

Parce qu'elle porte :

* la vente des accès ;
* la disponibilité ;
* les réservations ;
* l'émission des billets ;
* une forte complexité métier.

### Transactions

Parce qu'elles portent :

* les paiements ;
* les confirmations ;
* l'idempotence ;
* les remboursements ;
* les règlements.

### Contrôle & accès

Parce qu'il transforme le billet vendu en accès réel et comporte des contraintes opérationnelles fortes.

---

## 13.2 Supporting Domains

### Gestion des événements

Nécessaire pour le fonctionnement d'Eventix mais moins différenciante.

### Participants & suivi

Importante pour les organisateurs mais secondaire par rapport à la chaîne transactionnelle.

---

## 13.3 Generic / Supporting

### Identité

Une partie des mécanismes d'identité et d'authentification peut potentiellement être traitée comme une capacité générique.

### Communication

L'envoi d'emails et de notifications peut également reposer sur des mécanismes génériques.

---

# 14. Frontières conceptuelles provisoires

Les frontières suivantes sont proposées comme hypothèses :

```text
┌──────────────────────────────┐
│ Gestion des événements       │
│                              │
│ Événement / configuration    │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│ Billetterie                  │
│                              │
│ Offre / disponibilité /      │
│ réservation / billet         │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│ Transactions                 │
│                              │
│ Paiement / remboursement /   │
│ règlement                    │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│ Contrôle & accès             │
│                              │
│ Validation / accès           │
└──────────────────────────────┘
```

Autour de cette chaîne :

```text
Participants & suivi
Identité & communication
Statistiques & pilotage
```

Ces frontières sont **conceptuelles**.

Elles ne signifient pas encore :

> « chaque boîte sera obligatoirement un microservice ».

Cette décision appartient à une étape architecturale ultérieure.

---

# 15. Relations métier principales

## 15.1 Événement → Billetterie

Un événement fournit le contexte dans lequel une offre de billetterie est définie.

```text
Événement
   ↓
Offre de billetterie
```

---

## 15.2 Billetterie → Transactions

Une réservation peut conduire à une opération de paiement.

```text
Réservation
      ↓
Transaction
```

---

## 15.3 Transactions → Billetterie

La confirmation d'une transaction peut permettre la finalisation du processus d'achat et l'émission du billet.

---

## 15.4 Billetterie → Contrôle

Le billet délivré devient une information nécessaire au contrôle.

```text
Billet
  ↓
Contrôle
  ↓
Accès
```

---

## 15.5 Tous les domaines → Statistiques

Les différents domaines produisent des informations susceptibles d'être utilisées pour le pilotage.

```text
Événements ──────┐
Billetterie ─────┤
Transactions ────┤
Participants ────┤──→ Pilotage
Contrôle ────────┘
```

---

# 16. Décisions retenues

Les décisions suivantes sont retenues à ce stade.

### Décision D-01

Eventix est décomposé en grands domaines métier plutôt qu'en simples fonctionnalités.

### Décision D-02

La décomposition repose sur une combinaison de :

* capacités ;
* processus ;
* fonctionnalités ;
* règles ;
* complexité ;
* valeur stratégique.

### Décision D-03

La chaîne métier centrale est :

> **Événement → Offre → Réservation → Transaction → Billet → Distribution → Contrôle → Accès**

### Décision D-04

Les notions suivantes restent explicitement distinctes :

> **Réservation ≠ Paiement ≠ Billet**

### Décision D-05

Les principaux candidats au Core Domain sont :

* Billetterie ;
* Transactions ;
* Contrôle & accès.

### Décision D-06

Statistiques & pilotage est actuellement considéré comme une capacité transversale et non comme un domaine Core autonome.

### Décision D-07

Les domaines identifiés ne sont pas encore assimilés à des Bounded Contexts.

---

# 17. Points restant à approfondir

La présente décomposition soulève plusieurs questions qui devront être traitées dans les documents suivants.

## 17.1 Billetterie

* La réservation appartient-elle entièrement à la billetterie ?
* La gestion des quotas appartient-elle au même sous-domaine ?
* Le billet constitue-t-il un sous-domaine distinct ?
* Comment modéliser les différents types de billets ?

## 17.2 Transactions

* Quelle est exactement la frontière entre paiement et règlement ?
* Qui déclenche un remboursement ?
* Comment gérer une confirmation de paiement après expiration d'une réservation ?
* Quel est le rôle exact d'Eventix dans la transaction financière ?

## 17.3 Contrôle

* Le contrôle doit-il être considéré comme une partie de la billetterie ou comme un domaine séparé ?
* Quelle stratégie de fonctionnement hors ligne est nécessaire ?
* Comment gérer plusieurs scanners dans une évolution future ?

## 17.4 Participants

* Quelle différence exacte entre utilisateur, acheteur et participant ?
* Le participant est-il une entité du domaine Participants ou une projection issue de la billetterie ?

## 17.5 Statistiques

* Les statistiques sont-elles uniquement une vue analytique ?
* Certaines statistiques doivent-elles devenir des capacités métier ?
* Faut-il distinguer statistiques opérationnelles et analytiques ?

---

# 18. Conséquence pour la prochaine étape DDD

Cette décomposition fournit maintenant une base pour rechercher les **sous-domaines**.

La prochaine question ne sera plus :

> « Quelles fonctionnalités possède Eventix ? »

mais :

> **« À l'intérieur de chaque grand domaine, quelles capacités métier distinctes existent réellement ? »**

Exemple :

```text
BILLETTERIE
│
├── Gestion de l'offre
├── Tarification
├── Disponibilité
├── Réservation
├── Émission
└── Gestion du billet
```

Il faudra ensuite déterminer lesquelles sont :

* Core ;
* Supporting ;
* Generic ;
* stratégiquement différenciantes ;
* complexes ;
* candidates à une frontière autonome.

Cette étape permettra ensuite de revenir sur le **Core Domain** avec beaucoup plus de précision avant de définir les **Bounded Contexts**.




# Domaines — Eventix

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
3. [Convention d'identification](#3-convention-didentification)
4. [Domaine — IDENTITY](#4-domaine--identity)
5. [Domaine — CATALOG](#5-domaine--catalog)
6. [Domaine — DISCOVERY](#6-domaine--discovery)
7. [Domaine — BOOKING](#7-domaine--booking)
8. [Domaine — PAYMENT](#8-domaine--payment)
9. [Domaine — TICKETING](#9-domaine--ticketing)
10. [Domaine — ACCESS](#10-domaine--access)
11. [Domaine — FINANCE](#11-domaine--finance)
12. [Domaine — REFUND](#12-domaine--refund)
13. [Domaine — TRUST & SAFETY](#13-domaine--trust--safety)
14. [Domaine — OBSERVATION](#14-domaine--observation)
15. [Domaine — COMMUNICATION](#15-domaine--communication)
16. [Résumé des frontières](#16-résumé-des-frontières)
17. [Critères de qualité du document](#17-critères-de-qualité-du-document)
18. [Statut](#18-statut)

---

# 1. Objectif

Ce document définit les domaines métier d'Eventix identifiés dans la vue d'ensemble du domaine, complétés par le domaine transversal de cybersécurité interne. Pour chaque domaine, il précise :

- la responsabilité principale ;
- les concepts clés ;
- les règles métier spécifiques ;
- les exigences fonctionnelles associées ;
- les dépendances vers les autres domaines.

Il ne répète pas les définitions, les états métier ou les invariants déjà établis dans la vue d'ensemble du domaine. Il les utilise comme référence et les décline au niveau de chaque domaine.

---

# 2. Sources de référence

Ce document est dérivé principalement de :

- `05-domain-driven-design/vue-d-ensemble-du-domaine.md`

Les exigences fonctionnelles référencées proviennent de :

- `04-analyse-des-besoins/exigences-fonctionnelles.md`

---

# 3. Convention d'identification

Chaque domaine possède un identifiant unique :

| Identifiant | Domaine |
|---|---|
| `DOM-01` | IDENTITY |
| `DOM-02` | CATALOG |
| `DOM-03` | DISCOVERY |
| `DOM-04` | BOOKING |
| `DOM-05` | PAYMENT |
| `DOM-06` | TICKETING |
| `DOM-07` | ACCESS |
| `DOM-08` | FINANCE |
| `DOM-09` | REFUND |
| `DOM-10` | TRUST & SAFETY |
| `DOM-11` | OBSERVATION |
| `DOM-12` | COMMUNICATION |
| `DOM-13` | CYBERSECURITY OPERATIONS |

---

# 4. Domaine — IDENTITY

| Champ | Valeur |
|---|---|
| **Identifiant** | `DOM-01` |
| **Nom** | IDENTITY |
| **Responsabilité** | Gérer les comptes utilisateurs, les organisations et leurs capacités |

## 4.1. Concepts clés

- **Utilisateur** : personne physique disposant d'un compte Eventix.
- **Compte participant** : capacité d'un utilisateur à découvrir et participer à des événements.
- **Compte organisateur** : capacité d'un utilisateur à créer et gérer des événements.
- **Organisation** : entité juridique ou informelle pouvant être soumise à vérification.
- **Capacité** : ensemble des droits associés à un compte.

## 4.2. Règles métier spécifiques

- Un même compte utilisateur peut exercer les capacités de participant et d'organisateur.
- L'accès aux capacités d'organisateur nécessite une autorisation.
- Une organisation peut être soumise à vérification, suspension ou bannissement.
- Les informations de sécurité et de fraude ne sont pas exposées aux utilisateurs.
- Les informations complémentaires d'un compte peuvent être ajoutées après la création minimale.

## 4.3. Exigences associées

- EF-001 à EF-007 : gestion du compte participant et capacités organisateur
- EF-014 : vérification de l'organisateur
- EF-095 à EF-098 : bannissement et mesures associées

## 4.4. Dépendances

- **Aucune dépendance entrante** dans le cycle principal.
- **Fournit** : identité des acteurs à tous les autres domaines.

---

# 5. Domaine — CATALOG

| Champ | Valeur |
|---|---|
| **Identifiant** | `DOM-02` |
| **Nom** | CATALOG |
| **Responsabilité** | Gérer le cycle de vie des événements et leur configuration |

## 5.1. Concepts clés

- **Événement** : manifestation organisée proposée à un public.
- **Brouillon** : état initial permettant le travail préparatoire.
- **Soumission** : demande de vérification avant publication.
- **Vérification** : contrôle de crédibilité de l'événement et de l'organisateur.
- **Publication** : mise à disposition publique de l'événement.
- **Espace** : lieu physique ou virtuel accueillant l'événement.
- **Zone** : subdivision d'un espace avec capacité propre.
- **Capacité** : nombre maximal de participants pour une zone ou catégorie.
- **Catégorie de billet** : type de billet associé à une disponibilité.

## 5.2. Règles métier spécifiques

- Un événement commence dans un état ne permettant pas sa publication publique.
- La publication est conditionnée à une vérification positive.
- Un événement refusé ne peut pas être publié.
- Les ventes sont automatiquement arrêtées lorsque l'événement atteint son début.
- Aucune nouvelle vente normale n'est possible après le début ou la fermeture définitive des ventes.
- L'archivage ne détruit pas l'historique métier.
- Une modification ultérieure ne réécrit pas l'historique des opérations déjà réalisées.
- Une réduction de capacité incompatible avec les billets déjà attribués est refusée.
- Un événement ayant des opérations irréversibles ne peut pas être supprimé définitivement.

## 5.3. Exigences associées

- EF-008 à EF-020 : gestion du cycle de vie de l'événement
- EF-021 à EF-024 : espaces, zones et disponibilités
- EF-124 à EF-125 : suppression et conservation

## 5.4. Dépendances

- **Dépend de** : IDENTITY (organisateur autorisé).
- **Fournit** : événements publiés à DISCOVERY, BOOKING, TICKETING, ACCESS.

---

# 6. Domaine — DISCOVERY

| Champ | Valeur |
|---|---|
| **Identifiant** | `DOM-03` |
| **Nom** | DISCOVERY |
| **Responsabilité** | Permettre aux participants de découvrir et consulter les événements |

## 6.1. Concepts clés

- **Recherche** : fonction permettant de trouver des événements.
- **Filtre** : critère de sélection appliqué aux résultats.
- **Détail événement** : informations complètes d'un événement.
- **Disponibilité visible** : capacité restante affichée au participant.

## 6.2. Règles métier spécifiques

- La découverte ne nécessite pas de compte utilisateur.
- La disponibilité affichée tient compte des billets vendus et des réservations en cours.
- Seuls les événements publiés et accessibles sont visibles.

## 6.3. Exigences associées

- EF-025 à EF-029 : recherche, consultation, filtrage et disponibilité

## 6.4. Dépendances

- **Dépend de** : CATALOG (événements publiés).
- **Fournit** : parcours de découverte au participant.

---

# 7. Domaine — BOOKING

| Champ | Valeur |
|---|---|
| **Identifiant** | `DOM-04` |
| **Nom** | BOOKING |
| **Responsabilité** | Gérer les réservations temporaires et la disponibilité des billets |

## 7.1. Concepts clés

- **Réservation** : blocage temporaire d'une disponibilité avant achat.
- **Disponibilité** : capacité d'accueil encore attribuable.
- **Blocage temporaire** : mise en attente d'une disponibilité.
- **Expiration** : libération automatique après délai.

## 7.2. Règles métier spécifiques

- Une réservation bloque temporairement une disponibilité.
- Une réservation expire après cinq minutes.
- L'expiration libère la disponibilité sans constituer un échec de paiement.
- Une même disponibilité ne peut pas être attribuée simultanément à plusieurs achats valides.
- Une réservation ne constitue pas un achat.

## 7.3. Exigences associées

- EF-030 à EF-034 : création, blocage, expiration et libération des réservations

## 7.4. Dépendances

- **Dépend de** : CATALOG (événement et catégories configurées).
- **Fournit** : réservation confirmée ou expirée à PAYMENT.

---

# 8. Domaine — PAYMENT

| Champ | Valeur |
|---|---|
| **Identifiant** | `DOM-05` |
| **Nom** | PAYMENT |
| **Responsabilité** | Gérer les opérations de paiement et leur réconciliation |

## 8.1. Concepts clés

- **Paiement** : opération financière initiée par un participant.
- **Confirmation** : accusé de réception positif du service de paiement.
- **Échec** : opération n'ayant pas abouti.
- **Réconciliation** : traitement d'un paiement arrivant après expiration de la réservation.
- **Mobile Money** : moyen de paiement principal du MVP.

## 8.2. Règles métier spécifiques

- Le MVP supporte exclusivement Mobile Money.
- Une confirmation de paiement reçue plusieurs fois ne produit qu'un seul effet métier.
- Un paiement échoué est enregistré et traité.
- Un paiement confirmé après expiration de la réservation déclenche une réconciliation.
- La réconciliation détermine si la disponibilité est encore attribuable, déjà attribuée, ou si un remboursement doit être déclenché.
- Le paiement tardif n'est pas ignoré uniquement parce que la réservation a expiré.

## 8.3. Exigences associées

- EF-035 à EF-041 : initiation, suivi, confirmation, échec et réconciliation

## 8.4. Dépendances

- **Dépend de** : BOOKING (réservation valide).
- **Fournit** : résultat de paiement à TICKETING et REFUND.

---

# 9. Domaine — TICKETING

| Champ | Valeur |
|---|---|
| **Identifiant** | `DOM-06` |
| **Nom** | TICKETING |
| **Responsabilité** | Gérer les achats, l'émission des billets et leur transfert |

## 9.1. Concepts clés

- **Achat** : transaction confirmée donnant droit à un billet.
- **Billet** : titre d'accès émis à la suite d'un achat.
- **QR Code** : mécanisme d'identification et de contrôle du billet.
- **Propriétaire** : détenteur actif d'un billet.
- **Transfert** : changement de propriétaire actif.

## 9.2. Règles métier spécifiques

- Un billet est émis lorsqu'un achat est définitivement finalisé.
- La finalisation d'un achat ne doit pas être confondue avec le règlement financier ultérieur.
- Un billet a un propriétaire actif unique.
- Le transfert remplace le propriétaire sans créer de copie du billet.
- L'historique des changements de propriété est conservé.
- Le MVP ne fournit pas de mécanisme de revente.
- Un billet gratuit consomme la disponibilité correspondante.
- Si le paiement est confirmé mais que l'émission échoue techniquement, l'émission est reprise sans recréer l'achat ni débiter à nouveau.

## 9.3. Exigences associées

- EF-042 à EF-052 : achat, émission, consultation et billet gratuit
- EF-053 à EF-056 : transfert de billet

## 9.4. Dépendances

- **Dépend de** : PAYMENT (paiement confirmé), CATALOG (événement).
- **Fournit** : billet émis à ACCESS et COMMUNICATION.

---

# 10. Domaine — ACCESS

| Champ | Valeur |
|---|---|
| **Identifiant** | `DOM-07` |
| **Nom** | ACCESS |
| **Responsabilité** | Gérer le contrôle des billets à l'entrée des événements |

## 10.1. Concepts clés

- **Scan** : lecture du QR Code d'un billet.
- **Validation** : vérification de l'authenticité et de l'état du billet.
- **Billet utilisé** : billet ayant déjà donné accès.
- **Mode dégradé** : fonctionnement avec un seul scanner actif.
- **Présence** : statut d'un participant ayant effectivement accédé.

## 10.2. Règles métier spécifiques

- Un billet ne peut être validé qu'une seule fois.
- Un billet déjà utilisé, annulé ou appartenant à un autre événement est refusé.
- Le contexte de chaque contrôle est enregistré.
- Plusieurs scanners sont possibles lorsque l'état partagé est fiable.
- Lorsque l'état partagé n'est plus fiable, un seul scanner actif est autorisé.
- La fiabilité prime sur le débit lorsque la cohérence ne peut plus être garantie.
- Les opérations réalisées en mode dégradé sont réintégrées lorsque la synchronisation est rétablie.

## 10.3. Exigences associées

- EF-057 à EF-071 : affectation, scan, validation, mode dégradé et réintégration

## 10.4. Dépendances

- **Dépend de** : TICKETING (billets émis), CATALOG (événement et points d'entrée).
- **Fournit** : statut de présence à OBSERVATION.

---

# 11. Domaine — FINANCE

| Champ | Valeur |
|---|---|
| **Identifiant** | `DOM-08` |
| **Nom** | FINANCE |
| **Responsabilité** | Gérer la clôture financière, les soldes et les retraits |

## 11.1. Concepts clés

- **Clôture** : opération de détermination du montant net dû à l'organisateur.
- **Montant net** : ventes confirmées moins remboursements et frais.
- **Solde** : montant disponible pour retrait.
- **Retrait** : opération de versement du solde à l'organisateur.

## 11.2. Règles métier spécifiques

- Le montant net est calculé à partir des ventes confirmées moins les remboursements et frais applicables.
- Les règles précises concernant les frais, commissions et taxes restent à préciser.
- Après la clôture, le montant net disponible est ajouté au solde retirable.
- Un retrait ne peut excéder le solde disponible.
- Un retrait échoué est restitué au solde.
- Plusieurs retraits successifs sont autorisés tant qu'un solde suffisant reste disponible.

## 11.3. Exigences associées

- EF-105 à EF-114 : clôture, solde, retrait et suivi des retraits

## 11.4. Dépendances

- **Dépend de** : TICKETING (achats finalisés), REFUND (remboursements traités).
- **Fournit** : solde disponible à l'organisateur.

---

# 12. Domaine — REFUND

| Champ | Valeur |
|---|---|
| **Identifiant** | `DOM-09` |
| **Nom** | REFUND |
| **Responsabilité** | Gérer les remboursements et leur exécution |

## 12.1. Concepts clés

- **Obligation de remboursement** : situation métier entraînant une restitution due.
- **Montant de référence** : montant effectivement payé pour la transaction concernée.
- **État du remboursement** : statut de l'exécution du remboursement.

## 12.2. Règles métier spécifiques

- Le montant de référence correspond au montant effectivement payé.
- Une obligation de remboursement ne produit qu'un seul remboursement effectif.
- Un remboursement échoué techniquement conserve l'obligation et permet une nouvelle tentative.
- Un volume important de remboursements peut être traité progressivement.
- Le changement d'avis, la non-présentation ou le souhait de ne plus participer ne déclenchent pas automatiquement un remboursement lorsque l'événement est maintenu.
- Une annulation imputable à l'organisateur rend les billets concernés éligibles au remboursement.

## 12.3. Exigences associées

- EF-076 : éligibilité au remboursement après annulation
- EF-080 à EF-087 : détermination, création, calcul, suivi et idempotence des remboursements

## 12.4. Dépendances

- **Dépend de** : PAYMENT (paiement confirmé à rembourser), TICKETING (billets concernés).
- **Fournit** : remboursements traités à FINANCE.

---

# 13. Domaine — TRUST & SAFETY

| Champ | Valeur |
|---|---|
| **Identifiant** | `DOM-10` |
| **Nom** | TRUST & SAFETY |
| **Responsabilité** | Gérer les signalements, l'analyse des risques et les mesures de sécurité |

## 13.1. Concepts clés

- **Signalement** : alerte émise par un participant.
- **Analyse de risque** : évaluation du niveau de menace identifié.
- **Suspension** : mesure temporaire de restriction.
- **Bannissement** : mesure définitive d'exclusion.

## 13.2. Règles métier spécifiques

- Un signalement comporte un motif prédéfini obligatoire et une description facultative.
- Un signalement n'est pas considéré automatiquement comme une preuve de fraude.
- La réponse est adaptée au niveau de risque identifié.
- Les décisions de sécurité sont traçables.
- Le bannissement d'une organisation n'entraîne pas automatiquement la suppression de ses événements existants.
- Après évaluation, une mesure appropriée est appliquée : maintien, suspension, annulation ou nouvelle vérification.

## 13.3. Exigences associées

- EF-088 à EF-094 : signalement, analyse et traçabilité
- EF-095 à EF-098 : bannissement et mesures associées

## 13.4. Dépendances

- **Dépend de** : IDENTITY (organisation signalée), CATALOG (événements concernés).
- **Fournit** : décisions de sécurité à CATALOG et IDENTITY.

---

# 14. Domaine — OBSERVATION

| Champ | Valeur |
|---|---|
| **Identifiant** | `DOM-11` |
| **Nom** | OBSERVATION |
| **Responsabilité** | Produire les statistiques et maintenir l'historique des opérations métier |

## 14.1. Concepts clés

- **Statistique** : donnée agrégée relative à l'activité.
- **Historique** : enregistrement chronologique des opérations.
- **Traçabilité** : capacité à retracer une opération et son contexte.
- **Événement métier** : fait marquant survenu dans un domaine.

## 14.2. Règles métier spécifiques

- Les statistiques ne modifient pas les données métier.
- Les opérations importantes sont tracées avec leur contexte.
- L'historique n'est pas réécrit par les modifications ultérieures.
- Les ventes, entrées et participants présents sont distingués des billets vendus.

## 14.3. Exigences associées

- EF-099 à EF-104 : statistiques et pilotage
- EF-115 à EF-117 : historique et traçabilité

## 14.4. Dépendances

- **Dépend de** : tous les domaines (événements métier émis).
- **Fournit** : statistiques et historique aux organisateurs et à Eventix.

---

# 15. Domaine — COMMUNICATION

| Champ | Valeur |
|---|---|
| **Identifiant** | `DOM-12` |
| **Nom** | COMMUNICATION |
| **Responsabilité** | Gérer la distribution des billets et les communications aux participants |

## 15.1. Concepts clés

- **Mise à disposition** : accès au billet depuis le compte participant.
- **Email** : canal de distribution du billet.
- **Notification** : message informant d'un changement important.

## 15.2. Règles métier spécifiques

- Le billet est accessible depuis le compte du participant après émission.
- Le billet peut être distribué par email.
- Les participants concernés par une annulation ou un report sont informés.
- Les modalités détaillées de communication respectent les capacités du MVP.

## 15.3. Exigences associées

- EF-050 : billet par email
- EF-075 : information des participants
- EF-121 à EF-123 : mise à disposition, distribution et information

## 15.4. Dépendances

- **Dépend de** : TICKETING (billets émis), CATALOG (événements annulés ou reportés).
- **Fournit** : communications aux participants.

---

## 15.5 Domaine transversal — CYBERSECURITY OPERATIONS

| Champ | Valeur |
|---|---|
| **Identifiant** | `DOM-13` |
| **Nom** | CYBERSECURITY OPERATIONS |
| **Responsabilité** | Superviser les signaux de sécurité du système Eventix, qualifier les incidents et tracer les décisions humaines de réponse |
| **Catégorie** | Soutien |
| **MVP** | Oui, EF-145 à EF-147 |

Ce domaine interne ne couvre ni la vérification/fraude événementielle (TRUST & SAFETY), ni les statistiques et l'observabilité métier (OBSERVATION). Il ne détient pas les comptes, événements, billets ou données financières sur lesquels une réponse pourrait porter.

### Sous-domaines et frontières

- `SD-13-1` — Collecte et normalisation des signaux de sécurité.
- `SD-13-2` — Triage et investigation des alertes/incidents cyber.
- `SD-13-3` — Décision humaine, suivi et traçabilité de la réponse.

**Dépend de :** événements minimisés des contextes autorisés et habilitations de BC-01.
**Fournit :** alertes, dossiers d'incident et décisions consignées ; une mesure est exécutée par le propriétaire de l'actif, jamais par écriture directe du domaine cyber.

# 16. Résumé des frontières

| Domaine | Dépend de | Fournit à |
|---|---|---|
| IDENTITY | — | Tous les domaines |
| CATALOG | IDENTITY | DISCOVERY, BOOKING, TICKETING, ACCESS |
| DISCOVERY | CATALOG | Participant |
| BOOKING | CATALOG | PAYMENT |
| PAYMENT | BOOKING | TICKETING, REFUND |
| TICKETING | PAYMENT, CATALOG | ACCESS, COMMUNICATION |
| ACCESS | TICKETING, CATALOG | OBSERVATION |
| FINANCE | TICKETING, REFUND | Organisateur |
| REFUND | PAYMENT, TICKETING | FINANCE |
| TRUST & SAFETY | IDENTITY, CATALOG | CATALOG, IDENTITY |
| OBSERVATION | Tous les domaines | Organisateur, Eventix |
| COMMUNICATION | TICKETING, CATALOG | Participant |
| CYBERSECURITY OPERATIONS | Identité et signaux des contextes autorisés | Modules propriétaires des actifs ; responsables habilités |

---

# 17. Critères de qualité du document

Ce document doit respecter les propriétés suivantes :

- chaque domaine possède une responsabilité unique et clairement délimitée ;
- les concepts clés sont définis sans ambiguïté ;
- les règles métier spécifiques sont vérifiables ;
- les exigences associées sont référencées sans être répétées ;
- les dépendances sont explicites et unidirectionnelles ;
- aucune décision technique n'est prise ou implicite.

---

# 18. Statut

| Champ | Valeur |
|---|---|
| **Document** | `domaines.md` |
| **Version** | 1.0 |
| **Statut** | À valider par l'équipe |
| **Périmètre** | MVP Eventix |
| **Marché** | Cameroun |

| Principe | État |
|---|---|
| Responsabilité unique par domaine | ✅ APPLIQUÉ |
| Concepts clés définis | ✅ DÉFINIS |
| Règles métier spécifiques | ✅ IDENTIFIÉES |
| Exigences associées référencées | ✅ RÉFÉRENCÉES |
| Dépendances explicites | ✅ DÉFINIES |
| Décisions techniques | ⏳ NON PRÉJUGÉES |

---

## Notes de rédaction

### Cohérence avec la vue d'ensemble

Ce document ne répète pas les définitions, les états métier ou les invariants déjà établis dans `vue-d-ensemble-du-domaine.md`. Il les décline au niveau de chaque domaine en précisant les concepts, les règles spécifiques et les dépendances.

### Granularité

La granularité retenue est celle du domaine métier. Chaque domaine pourra faire l'objet d'un document détaillé supplémentaire dans les phases ultérieures de cette phase 05.

### Questions ouvertes

Les questions métier encore ouvertes identifiées dans les phases précédentes ne sont pas tranchées ici. Elles restent référencées dans `questions-metier-ouvertes.md`.
