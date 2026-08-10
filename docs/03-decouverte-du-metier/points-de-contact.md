# Points de contact métier — Eventix

**Version :** 1.0
**Statut :** Validé — MVP
**Marché initial :** Cameroun
**Dernière mise à jour :** 2026-08-10

---

# 1. Objectif

Ce document cartographie les principaux **points de contact entre Eventix et ses acteurs métier**.

Un point de contact représente un endroit où un acteur :

* consulte Eventix ;
* fournit une information ;
* reçoit une information ;
* réalise une opération ;
* reçoit une décision ;
* rencontre un problème ;
* interagit avec le support.

Ce document décrit le **comportement métier attendu**, et non l'implémentation technique.

---

# 2. Périmètre

## 2.1 MVP

Le MVP couvre principalement :

* découverte des événements ;
* consultation des événements ;
* achat de billets ;
* réservation temporaire ;
* paiement ;
* génération et récupération des billets ;
* événements gratuits ;
* contrôle des billets ;
* statistiques organisateur ;
* vérification et publication des événements ;
* notifications liées aux événements ;
* support par email.

## 2.2 Fonctionnalités futures

Les éléments suivants sont explicitement hors MVP :

* vente physique ;
* réseau de points physiques ;
* agents physiques ;
* Marketplace de revente ;
* Quiz Live ;
* recommandations avancées ;
* support par chat ;
* support téléphonique ;
* WhatsApp pour l'envoi des billets ;
* fonctionnement offline des points de vente.

---

# 3. Acteurs concernés

| Acteur                 | Rôle principal                           | MVP |
| ---------------------- | ---------------------------------------- | --: |
| Participant            | Découvrir et acheter des billets         |   ✅ |
| Acheteur               | Effectuer un achat                       |   ✅ |
| Propriétaire du billet | Détenir un billet                        |   ✅ |
| Organisateur           | Créer et gérer un événement              |   ✅ |
| Équipe organisateur    | Participer à la gestion selon les droits |   ✅ |
| Administrateur Eventix | Vérification et administration           |   ✅ |
| Contrôleur             | Vérifier les billets à l'entrée          |   ✅ |
| Point physique         | Vendre des billets                       |   ❌ |
| Agent physique         | Vendre des billets                       |   ❌ |
| Support Eventix        | Traiter les demandes                     |   ✅ |

---

# 4. Cartographie générale

```text
                         EVENTIX
                            │
          ┌─────────────────┼─────────────────┐
          │                 │                 │
          ↓                 ↓                 ↓
     Participant       Organisateur       Contrôleur
          │                 │                 │
          ↓                 ↓                 ↓
     Découverte          Dashboard          Scanner
     Achat               Configuration      Contrôle
     Paiement            Publication
     Billet              Statistiques
     Support             Support
```

---

# 5. Participant — Découverte

## PC01 — Découverte d'un événement

Le participant doit pouvoir découvrir les événements grâce à :

* catégories ;
* recherche ;
* filtres.

```text
Participant
    ↓
Eventix
    ├── Catégories
    ├── Recherche
    └── Filtres
            ↓
       Événements
```

### MVP

✅ Catégories
✅ Recherche
✅ Filtres

### Futur

Les recommandations personnalisées et la recherche intelligente pourront être ajoutées ultérieurement.

---

# 6. Participant — Consultation

## PC02 — Consultation de la fiche événement

Le participant doit pouvoir consulter les informations pertinentes configurées pour l'événement.

Notamment :

* nom ;
* description ;
* date ;
* heure ;
* lieu ;
* catégories de billets ;
* prix ;
* disponibilité ;
* informations relatives aux places lorsqu'elles existent.

La disponibilité présentée doit être suffisamment actuelle pour éviter de présenter comme disponible un billet qui ne l'est plus.

```text
Participant
     ↓
Fiche événement
     ↓
Informations
     +
Disponibilité
```

---

# 7. Participant — Achat

## PC03 — Réservation temporaire

Lorsqu'un participant sélectionne un billet payant, Eventix crée une réservation temporaire.

La réservation reste active pendant **5 minutes**.

```text
Sélection
   ↓
PENDING
   ↓
5 minutes
   ↓
Paiement
```

À l'expiration :

```text
PENDING
   ↓
EXPIRED
   ↓
Disponibilité libérée
```

L'expiration de la réservation ne signifie pas automatiquement que le paiement a échoué.

---

# 8. Participant — Réception du billet

## PC04 — Réception du billet

Après confirmation du paiement, le participant doit pouvoir :

* télécharger son billet depuis Eventix ;
* recevoir son billet par email.

```text
Paiement confirmé
       ↓
Billet généré
       ├── Téléchargement
       └── Email
```

### MVP

✅ Téléchargement
✅ Email

### Futur

❌ WhatsApp

---

# 9. Participant — Récupération après anomalie

## PC05 — Récupération d'un billet après paiement

Le compte Eventix constitue le point de contact principal permettant de retrouver les billets achetés.

En cas d'anomalie, le participant peut contacter le support.

```text
Paiement
   ↓
Compte Eventix
   ↓
Mes billets
   │
   └── Anomalie
          ↓
       Support
```

---

# 10. Participant — Création d'identité

## PC06 — Compte créé lors du premier achat

Lors du premier achat, Eventix crée ou initialise automatiquement une **identité participant** associée aux informations de contact fournies.

Cette identité permet de centraliser :

* les billets ;
* les achats ;
* l'historique ;
* les participations.

```text
Premier achat
     ↓
Identité participant
     ↓
Compte Eventix
     ├── Billets
     ├── Achats
     ├── Historique
     └── Participations
```

L'identité participant et le mécanisme d'authentification sont des concepts distincts.

Eventix doit éviter de créer plusieurs identités pour une même personne lorsque celle-ci peut être reconnue de manière fiable.

---

# 11. Contrôle — Accès à l'événement

## PC10 — Résultat du contrôle

Lorsqu'un contrôleur scanne un billet, Eventix retourne :

* `VALIDE` si le billet peut être utilisé ;
* `INVALIDE` si le billet ne peut pas être utilisé ;
* une raison explicite en cas de refus.

Exemples :

```text
✅ VALIDE

❌ INVALIDE
   → Billet déjà utilisé

❌ INVALIDE
   → Billet annulé

❌ INVALIDE
   → Billet inexistant

❌ INVALIDE
   → Billet non valable pour cet événement
```

Le contrôleur ne doit accéder qu'aux informations nécessaires à son activité.

---

# 12. Contrôle — Habilitation

## PC11 — Validité du billet indépendante de sa catégorie

L'habilitation d'un contrôleur porte sur son périmètre de contrôle et non sur la catégorie du billet.

Un contrôleur habilité à contrôler une entrée peut contrôler tout billet valide accepté à cette entrée.

```text
Contrôleur
    ↓
Entrée A
    ├── Standard ✅
    ├── VIP      ✅
    └── Premium  ✅
```

La catégorie du billet ne constitue donc pas une permission individuelle.

```text
Habilitation de l'agent
        ≠
Catégorie du billet
```

---

# 13. Contrôle — Réutilisation

## PC12 — Billet déjà utilisé

Lorsqu'un billet est validé une première fois, son utilisation est enregistrée.

Toute tentative ultérieure doit être refusée.

```text
Premier scan
     ↓
VALIDÉ
     ↓
USED
     ↓
Second scan
     ↓
❌ REFUS
     ↓
Billet déjà utilisé
```

Le système peut fournir les informations nécessaires au contrôle, notamment l'heure et le point d'entrée du premier contrôle.

---

# 14. Statistiques

## PC13 — Statistiques multi-niveaux du MVP

Le MVP doit permettre à l'organisateur de suivre notamment :

* billets vendus ;
* ventes en ligne ;
* catégories de billets ;
* revenus générés ;
* billets disponibles ;
* billets utilisés ;
* participants entrés.

```text
                  ÉVÉNEMENT
                      │
          ┌───────────┼───────────┐
          ↓           ↓           ↓
        VENTES    CATÉGORIES   CONTRÔLE
          │           │           │
        Online       VIP        Entrées
                     Standard
```

### MVP

Les statistiques sont principalement centrées sur :

* l'événement ;
* les ventes en ligne ;
* les catégories ;
* les revenus ;
* le contrôle d'accès.

### Futur

Les statistiques par :

* point physique ;
* agent physique ;
* canal physique ;

seront ajoutées avec la vente physique.

Les recommandations automatiques sont également hors MVP.

---

# 15. Annulation

## PC14 — Communication lors d'une annulation

Lorsqu'un événement est annulé, Eventix informe automatiquement les participants concernés.

Le participant peut consulter depuis son compte :

* le statut de l'événement ;
* son billet ;
* les informations relatives au remboursement lorsque celui-ci est applicable.

```text
Événement annulé
       ↓
Eventix
       ↓
Notification
       ↓
Compte participant
       ├── Billet
       ├── Statut
       └── Remboursement
```

Le remboursement intervient conformément aux règles métier définies lorsqu'il est lié à une responsabilité de l'organisateur.

---

# 16. Report

## PC15 — Communication lors d'un report

Lorsqu'un événement est reporté, Eventix informe automatiquement le participant de la nouvelle date.

Le billet existant est conservé et transféré automatiquement vers la nouvelle date.

Le participant n'a pas besoin de réacheter son billet.

```text
Événement reporté
       ↓
Notification
       ↓
Participant
       ↓
Billet conservé
       ↓
Nouvelle date
```

La communication est donc automatique, tandis que la conservation du billet relève de la règle métier.

---

# 17. Support participant

## PC16 — Support participant

Dans le MVP, le support participant est accessible **par email**.

```text
Participant
     ↓
Problème
     ↓
Email Support Eventix
     ↓
Équipe Eventix
```

### MVP

✅ Email

### Futur

❌ Chat
❌ Téléphone

---

# 18. Support organisateur

## PC17 — Support organisateur

L'organisateur dispose d'un support dédié par email.

Le support peut traiter notamment les problèmes liés à :

* création d'événement ;
* configuration ;
* publication ;
* billetterie ;
* statistiques ;
* opérations financières.

```text
Organisateur
     ↓
Support Organisateur
     ↓
Email
     ↓
Équipe Eventix
```

---

# 19. Vérification

## PC18 — Suivi de la vérification

L'organisateur doit pouvoir suivre depuis son dashboard l'état de vérification de son événement.

Eventix informe l'organisateur lorsqu'une décision est prise.

```text
Organisateur
     ↓
Dashboard
     ↓
Vérification
     ├── En attente
     ├── Validé
     └── Refusé
```

Le support n'est pas nécessaire pour connaître l'état normal de la vérification.

---

# 20. Publication

## PC19 — Publication contrôlée par l'organisateur

Après validation par Eventix, l'événement n'est pas automatiquement publié.

L'organisateur doit effectuer explicitement l'action **Publier**.

```text
Création
   ↓
Soumission
   ↓
Vérification Eventix
   ↓
Validé
   ↓
Organisateur → Publier
   ↓
Événement public
```

La validation Eventix et la publication sont donc deux opérations distinctes.

---

# 21. Statistiques quasi temps réel

## PC20 — Actualisation automatique

Dans le MVP, les statistiques sont actualisées automatiquement avec un délai acceptable.

L'organisateur n'a pas besoin de recharger manuellement son dashboard.

```text
Vente
  ↓
Eventix
  ↓
Mise à jour
  ↓
Dashboard
```

Le terme métier privilégié est **quasi temps réel**.

Il ne suppose pas une garantie de latence nulle.

---

# 22. Événement gratuit

## PC21 — Obtention d'un billet gratuit

Pour un événement gratuit, le participant peut obtenir directement son billet sans passer par un paiement.

```text
Événement gratuit
       ↓
Choix billet
       ↓
Obtenir
       ↓
Billet
       ├── Téléchargement
       └── Email
```

Une opération gratuite ne nécessite donc pas de transaction financière.

---

# 23. Disponibilité pendant une réservation

## PC22 — Disponibilité temporairement bloquée

Lorsqu'une disponibilité est réservée temporairement, elle n'est plus présentée comme disponible pour les autres participants pendant la durée de la réservation.

```text
10 billets
   ↓
3 réservés
   ↓
7 disponibles
   ↓
5 minutes
```

À l'issue du délai :

```text
Paiement confirmé
→ Attribution

OU

Expiration
→ Libération
```

Cette règle contribue à empêcher la survente.

---

# 24. Paiement explicitement échoué

## PC23 — Libération immédiate

Lorsqu'un paiement est explicitement confirmé comme échoué, Eventix libère immédiatement la réservation.

```text
PENDING
   ↓
Paiement échoué
   ↓
Réservation libérée
   ↓
Disponibilité restaurée
```

Cette situation doit être distinguée d'un paiement dont le résultat est encore inconnu.

```text
Paiement échoué
→ résultat connu

Paiement inconnu
→ résultat inconnu
→ réconciliation nécessaire
```

---

# 25. Paiement confirmé après expiration

## PC24 — Réconciliation automatique

Si une réservation est arrivée à expiration mais que le paiement est ensuite confirmé, Eventix ne considère pas automatiquement le paiement comme échoué.

Eventix lance une réconciliation.

```text
Réservation
    ↓
EXPIRED
    ↓
Paiement confirmé
    ↓
Réconciliation
    │
    ├── Billet encore disponible
    │       ↓
    │   Attribution
    │
    └── Billet déjà attribué
            ↓
       Remboursement
```

Le participant est informé du résultat.

Cette règle est fondamentale :

> **L'expiration concerne la réservation. Le paiement est confirmé séparément.**

---

# 26. Parcours métier principaux

## 26.1 Parcours — Achat payant

```text
Participant
    ↓
Recherche
    ↓
Fiche événement
    ↓
Choix billet
    ↓
Réservation 5 min
    ↓
Paiement
    │
    ├── Réussi
    │      ↓
    │   Billet
    │
    ├── Échoué
    │      ↓
    │   Libération
    │
    └── Inconnu
           ↓
       Réconciliation
```

---

## 26.2 Parcours — Achat gratuit

```text
Participant
    ↓
Événement gratuit
    ↓
Choix billet
    ↓
Obtention
    ↓
Billet
    ↓
Email + téléchargement
```

---

## 26.3 Parcours — Contrôle

```text
Participant
    ↓
Entrée
    ↓
QR Code
    ↓
Contrôleur
    ↓
Eventix
    │
    ├── VALIDE
    │      ↓
    │    Accès
    │
    └── INVALIDE
           ↓
       Motif du refus
```

---

## 26.4 Parcours — Événement reporté

```text
Organisateur
    ↓
Report
    ↓
Eventix
    ↓
Notification
    ↓
Participant
    ↓
Billet conservé
    ↓
Nouvelle date
```

---

## 26.5 Parcours — Événement annulé

```text
Organisateur / Eventix
        ↓
Annulation
        ↓
Eventix
        ↓
Participants concernés
        ↓
Notification
        ↓
Suivi depuis le compte
        ↓
Remboursement si applicable
```

---

# 27. Points de contact exclus du MVP

Les points de contact suivants existent dans la vision Eventix mais ne doivent pas être considérés comme faisant partie du MVP.

## Vente physique

```text
Participant
    ↓
Point physique
    ↓
Agent Eventix
    ↓
Eventix
```

Cette architecture sera étudiée dans une version future.

---

## Marketplace

La Marketplace permettra ultérieurement de mettre en relation :

```text
Propriétaire du billet
        ↓
Marketplace Eventix
        ↓
Nouvel acheteur
```

Elle sera étudiée comme un domaine métier spécifique.

---

## Quiz Live

Le Quiz Live constituera un futur domaine fonctionnel.

Il ne doit pas être mélangé au domaine de billetterie du MVP.

---

## Support multicanal

Chat et téléphone pourront être ajoutés ultérieurement.

---

# 28. Principes issus des points de contact

## PC-P01 — Eventix comme point central

Les opérations importantes doivent passer par Eventix afin de garantir la cohérence des données et la traçabilité.

---

## PC-P02 — Ne pas confondre canal et métier

Un canal de communication ou de vente ne doit pas créer une nouvelle définition du billet.

Exemple futur :

```text
Vente en ligne
Vente physique
     ↓
Même billet métier Eventix
```

---

## PC-P03 — Le compte centralise l'historique

L'identité participant doit permettre de retrouver les opérations et billets associés.

---

## PC-P04 — Le support est un mécanisme d'exception

Le participant ou l'organisateur doit pouvoir effectuer normalement ses opérations sans devoir contacter le support.

---

## PC-P05 — Les décisions doivent être visibles

Lorsqu'une opération nécessite une décision Eventix, son état doit être accessible à l'acteur concerné.

Exemple :

```text
Vérification
→ Dashboard
→ Statut
```

---

## PC-P06 — Les canaux ne doivent pas créer de survente

Les différentes sources de vente doivent respecter la même disponibilité métier.

---

# 29. Tableau récapitulatif des décisions

| ID   | Point de contact          |  MVP  | Décision                                   |
| ---- | ------------------------- | :---: | ------------------------------------------ |
| PC01 | Découverte événement      |   ✅   | Catégories + recherche + filtres           |
| PC02 | Fiche événement           |   ✅   | Informations + disponibilité               |
| PC03 | Réservation               |   ✅   | 5 minutes                                  |
| PC04 | Réception billet          |   ✅   | Téléchargement + email                     |
| PC05 | Récupération billet       |   ✅   | Compte + support si anomalie               |
| PC06 | Identité participant      |   ✅   | Compte/identité créé lors du premier achat |
| PC07 | Vente physique            |   ❌   | Flux futur                                 |
| PC08 | Inventaire multi-canaux   | Futur | Inventaire centralisé                      |
| PC09 | Billet physique           |   ❌   | Représentation du billet Eventix           |
| PC10 | Contrôle                  |   ✅   | Valide/invalide + raison                   |
| PC11 | Habilitation contrôleur   |   ✅   | Indépendante de la catégorie               |
| PC12 | Réutilisation billet      |   ✅   | Second scan refusé                         |
| PC13 | Statistiques              |   ✅   | Multi-niveaux du MVP                       |
| PC14 | Annulation                |   ✅   | Notification + suivi                       |
| PC15 | Report                    |   ✅   | Notification + billet conservé             |
| PC16 | Support participant       |   ✅   | Email                                      |
| PC17 | Support organisateur      |   ✅   | Email dédié                                |
| PC18 | Vérification              |   ✅   | Dashboard + notification                   |
| PC19 | Publication               |   ✅   | Action explicite de l'organisateur         |
| PC20 | Statistiques              |   ✅   | Quasi temps réel                           |
| PC21 | Billet gratuit            |   ✅   | Génération directe                         |
| PC22 | Disponibilité             |   ✅   | Bloquée pendant réservation                |
| PC23 | Paiement échoué           |   ✅   | Libération immédiate                       |
| PC24 | Paiement après expiration |   ✅   | Réconciliation                             |

---

# 30. Séparation stricte MVP / Futur

```text
                         EVENTIX
                            │
              ┌─────────────┴─────────────┐
              │                           │
             MVP                         FUTUR
              │                           │
       Découverte                    Vente physique
       Achat                         Agents
       Paiement                      Points physiques
       Billet                        Marketplace
       Contrôle                      Quiz Live
       Statistiques                  Support chat
       Vérification                  Téléphone
       Publication                   WhatsApp
       Support email                 Recommandations
       Annulation
       Report
```

Cette séparation doit être respectée dans les prochaines phases afin d'éviter d'introduire prématurément de la complexité dans la conception du MVP.

---

# 31. Statut du document

**Version :** 1.0
**Statut :** Validé
**Périmètre :** Découverte métier — Eventix
**MVP :** Oui
**Marché initial :** Cameroun

Les décisions contenues dans ce document constituent la référence actuelle des points de contact métier.

Toute nouvelle fonctionnalité importante doit être identifiée comme **MVP** ou **future** avant d'être intégrée aux processus métier.
