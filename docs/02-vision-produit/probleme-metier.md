# Problème métier

##  Contexte

L'organisation d'événements repose aujourd'hui principalement sur des outils dispersés comme les réseaux sociaux, les messageries instantanées, les paiements mobiles et les fichiers de suivi manuels.

Cette organisation fragmentée rend difficile la gestion efficace des événements, aussi bien pour les organisateurs que pour les participants.



## 1. Problèmes  généraux


## Problèmes rencontrés par les organisateurs

### Gestion des participants

- Difficulté à savoir précisément qui a acheté ou réservé une place.
- Absence d'un système centralisé de suivi des participants.
- Risque d'erreurs dans les listes manuelles.

### Sécurisation des accès

- Difficulté à vérifier rapidement les entrées.
- Risque de fraude ou d'utilisation de faux justificatifs.
- Manque d'un système fiable de validation des billets.

### Suivi des ventes et des performances

- Difficulté à connaître en temps réel l'état des ventes.
- Manque de visibilité sur le remplissage de l'événement.
- Difficulté à analyser les résultats après l'événement.

---

## Problèmes rencontrés par les participants

- Difficulté à trouver facilement les événements qui correspondent à leurs intérêts.
- Parcours d'achat parfois complexe.
- Manque d'une expérience numérique moderne avant, pendant et après l'événement.

---

## Problèmes rencontrés par les autres acteurs

Les équipes d'organisation, contrôleurs et partenaires rencontrent également des difficultés liées au manque d'outils adaptés pour collaborer, suivre les opérations et améliorer l'expérience événementielle.

---

## Conséquences

Ces problèmes provoquent :

- Une organisation plus longue et plus complexe.
- Une augmentation des risques d'erreurs et de fraude.
- Une mauvaise visibilité sur la réussite d'un événement.
- Une expérience participant moins fluide.
- Une difficulté à améliorer les événements futurs grâce aux données.

---

## Opportunité pour Eventix

Eventix répond à ce problème en proposant une plateforme centralisée permettant de gérer l'ensemble du cycle de vie d'un événement :

- Avant : découverte, communication, réservation et vente.
- Pendant : contrôle d'accès, animation et interaction.
- Après : analyse, statistiques et amélioration continue.

L'objectif est de rendre l'organisation événementielle plus simple, sécurisée et intelligente.





## 2. Problèmes principaux


Les acteurs de l'événementiel rencontrent des difficultés à gérer l'ensemble du cycle de vie d'un événement à cause de processus manuels, d'un manque de centralisation des informations et d'un manque de données fiables.

Cela entraîne une perte de temps, des risques d'erreur, des problèmes de sécurité et une expérience utilisateur limitée.
---

### 2.1 Gestion dispersée de la billetterie

Les ventes peuvent être réalisées par plusieurs canaux :

* vente en ligne ;
* points physiques ;
* équipes de vente.

Sans gestion centralisée, les informations de disponibilité et de vente peuvent devenir incohérentes.

**Problème :**

> Comment garantir une vision unique et fiable des billets disponibles, réservés et vendus quel que soit le canal de vente ?

---

### 2.2 Risque de double vente

Plusieurs personnes peuvent tenter d'obtenir simultanément la même place.

Exemple :

```text
Client A ──┐
           ├──→ Place A42
Client B ──┘
```

**Problème :**

> Comment garantir qu'une même place ou unité de capacité ne puisse pas être attribuée simultanément à plusieurs participants ?

---

### 2.3 Gestion complexe des espaces

Un événement peut comporter :

* plusieurs espaces ;
* plusieurs zones ;
* des places numérotées ;
* des zones sans places numérotées ;
* des capacités différentes ;
* différents types de billets.

**Problème :**

> Comment permettre à un organisateur de représenter fidèlement la configuration réelle de son événement et de contrôler sa capacité ?

---

### 2.4 Gestion des équipes

Un événement peut nécessiter plusieurs intervenants :

* administrateurs ;
* gestionnaires ;
* vendeurs ;
* contrôleurs ;
* employés ;
* responsables de points physiques.

Tous ne doivent pas avoir les mêmes droits.

**Problème :**

> Comment permettre à une organisation de donner à chaque utilisateur uniquement les accès nécessaires à son travail ?

Les accès peuvent être limités à un événement ou à un périmètre particulier.

---

### 2.5 Gestion des points physiques

Un organisateur peut vouloir vendre des billets physiquement tout en conservant la gestion centralisée des ventes.

Le point physique ne doit pas constituer une billetterie indépendante.

**Problème :**

> Comment permettre à un agent physique de vendre un billet réel tout en utilisant le même inventaire que la vente en ligne ?

---

### 2.6 Gestion des réservations

Une place peut être sélectionnée par un participant avant que le paiement ne soit confirmé.

Elle ne doit pas rester bloquée indéfiniment.

**Problème :**

> Comment gérer temporairement une réservation sans empêcher inutilement d'autres ventes ?

Une réservation peut donc expirer indépendamment de l'état du paiement.

---

### 2.7 Séparation réservation, paiement et billet

Une réservation, un paiement et un billet représentent des réalités métier différentes.

Un paiement peut notamment rester en attente alors que la réservation arrive à expiration.

**Problème :**

> Comment gérer correctement ces différents cycles de vie sans confondre leurs états ?

---

### 2.8 Paiements incertains

Un paiement peut être :

* initié ;
* en attente ;
* réussi ;
* échoué ;
* confirmé tardivement.

Le client peut être débité alors qu'Eventix n'a pas encore reçu la confirmation.

**Problème :**

> Comment garantir qu'une transaction financière soit correctement traitée même lorsque les confirmations sont retardées, répétées ou interrompues ?

---

### 2.9 Réconciliation des paiements

Un paiement peut être confirmé après l'expiration d'une réservation.

Dans ce cas, Eventix doit déterminer si le billet peut encore être attribué ou si une compensation doit être effectuée.

**Problème :**

> Comment réconcilier une transaction financière avec l'état réel de l'inventaire ?

---

### 2.10 Remboursements

L'annulation d'un événement peut concerner un très grand nombre de transactions.

Le traitement manuel de chaque remboursement n'est pas adapté à une plateforme destinée à fonctionner à grande échelle.

**Problème :**

> Comment traiter les remboursements de manière automatisée, traçable et résiliente, tout en permettant une intervention humaine pour les exceptions ?

---

### 2.11 Annulation d'un événement

Lorsqu'un événement est annulé, plusieurs éléments peuvent être concernés simultanément :

* billets ;
* paiements ;
* remboursements ;
* participants ;
* points physiques ;
* historique des transactions.

**Problème :**

> Comment gérer les conséquences d'une annulation sans perdre l'historique ni créer d'incohérences financières ou opérationnelles ?

---

### 2.12 Report d'un événement

Un événement peut être reporté plutôt qu'annulé.

Les billets déjà vendus doivent normalement rester valides pour la nouvelle date.

Cependant, un changement de :

* lieu ;
* capacité ;
* configuration ;
* places ;
* conditions ;

peut rendre le transfert plus complexe.

**Problème :**

> Comment transférer les billets existants vers un événement reporté tout en préservant les droits des participants et la cohérence de l'inventaire ?

---

### 2.13 Modification des prix

Un organisateur peut modifier le prix d'un billet après le début des ventes.

Les transactions déjà réalisées ne doivent pas être modifiées.

**Problème :**

> Comment faire évoluer les prix futurs tout en conservant l'historique exact des prix réellement payés ?

---

### 2.14 Tarification évolutive

Les organisateurs peuvent utiliser différentes stratégies :

* prix fixe ;
* paliers de prix ;
* périodes promotionnelles ;
* Early Bird ;
* prix différents selon les catégories.

**Problème :**

> Comment déterminer le prix applicable à une vente tout en garantissant que le prix associé à une réservation temporaire reste cohérent pendant son processus d'achat ?

---

### 2.15 Contrôle d'accès

Un billet doit permettre de déterminer si son détenteur peut entrer à l'événement.

Un même billet ne doit pas pouvoir être utilisé plusieurs fois.

**Problème :**

> Comment garantir un contrôle d'accès fiable et empêcher la réutilisation frauduleuse d'un billet ?

---

### 2.16 Suivi de présence

Eventix doit pouvoir distinguer :

```text
Billet vendu
    ≠
Participant présent
```

Un billet peut être vendu sans avoir été utilisé.

**Problème :**

> Comment suivre la présence réelle des participants sans confondre vente, possession du billet et accès à l'événement ?

---

### 2.17 Transfert des billets

Un acheteur peut acheter un billet pour une autre personne ou souhaiter transférer son billet.

Le transfert doit pouvoir être contrôlé et conservé dans l'historique.

**Problème :**

> Comment permettre le transfert d'un billet tout en maintenant sa traçabilité et les éventuelles restrictions définies pour l'événement ?

---

### 2.18 Événements à temporalité différente

Certains événements sont préparés plusieurs mois à l'avance.

D'autres, comme certaines foires, peuvent fonctionner selon le modèle :

```text
Achat
 ↓
Paiement
 ↓
Émission du billet
 ↓
Entrée immédiate
```

**Problème :**

> Comment gérer aussi bien les événements à vente anticipée que les événements où l'achat et l'utilisation du billet sont presque simultanés ?

---

## 3. Problème central

Tous ces problèmes convergent vers une difficulté principale :

> **Permettre à Eventix de gérer de manière fiable et cohérente l'ensemble du cycle de vie d'un événement, de sa création jusqu'au contrôle des participants, tout en coordonnant plusieurs canaux de vente, plusieurs utilisateurs, des opérations financières et des contraintes de capacité.**

---

## 4. Conséquence pour la conception

Ces problèmes impliquent qu'Eventix devra notamment garantir :

* une source de vérité cohérente pour l'inventaire ;
* une séparation claire entre réservation, paiement et billet ;
* la traçabilité des opérations ;
* la gestion des droits et responsabilités ;
* la conservation de l'historique des transactions ;
* la gestion des situations exceptionnelles ;
* la capacité à traiter les opérations automatiquement lorsque leur volume augmente.

Les solutions techniques permettant de satisfaire ces exigences seront étudiées dans les phases d'analyse des besoins, de DDD et de conception du système.

---

## 5. Hors périmètre à ce stade

Ce document ne définit pas encore :

* l'architecture technique ;
* les technologies ;
* la base de données ;
* les microservices ;
* l'infrastructure cloud ;
* les mécanismes de cache ;
* les files de messages ;
* les stratégies de scalabilité ;
* les détails d'implémentation.

Ces décisions seront prises dans les phases correspondantes.
