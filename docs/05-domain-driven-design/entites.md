# Entités — Eventix

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
3. [Définition d'une entité](#3-définition-dune-entité)
4. [Convention d'identification](#4-convention-didentification)
5. [Entités par bounded context](#5-entités-par-bounded-context)
6. [Cycle de vie des entités](#6-cycle-de-vie-des-entités)
7. [Relations entre entités](#7-relations-entre-entités)
8. [Invariants portés par les entités](#8-invariants-portés-par-les-entités)
9. [Résumé](#9-résumé)
10. [Critères de qualité du document](#10-critères-de-qualité-du-document)
11. [Statut](#11-statut)

---

# 1. Objectif

Ce document identifie les entités métier d'Eventix à partir des bounded contexts définis dans `bounded-contexts.md` et du langage ubiquitaire établi dans `langage-ubiquitaire.md`. Pour chaque entité, il précise :

- l'identité unique et stable ;
- le cycle de vie ;
- les responsabilités propres ;
- les relations avec les autres entités ;
- les invariants qu'elle porte ou contribue à préserver.

Il ne redéfinit ni les bounded contexts, ni les termes du langage ubiquitaire. Ces éléments sont utilisés par référence et supposés connus du lecteur.

---

# 2. Sources de référence

Ce document est dérivé principalement de :

- `05-domain-driven-design/bounded-contexts.md` — frontières des douze bounded contexts
- `05-domain-driven-design/langage-ubiquitaire.md` — vocabulaire métier par contexte

Toute définition, frontière ou règle métier mentionnée implicitement renvoie à ces documents sources.

---

# 3. Définition d'une entité

## 3.1. Caractéristiques

Une entité est un objet métier qui possède :

- **Une identité unique** : deux entités de même type sont distinguées par leur identité, pas par leurs attributs.
- **Un cycle de vie** : elle est créée, évolue à travers des états, et peut être archivée ou supprimée.
- **Une continuité** : elle persiste à travers les changements d'état et les opérations qui la concernent.

## 3.2. Distinction avec les objets de valeur

| Entité | Objet de valeur |
|---|---|
| Identité unique et stable | Identité définie par ses attributs |
| Cycle de vie avec états | Immuable après création |
| Égalité par identifiant | Égalité par valeur |
| Exemple : Billet, Événement | Exemple : Prix, Adresse email |

## 3.3. Règle de nomination

Le nom d'une entité est un terme du langage ubiquitaire. Aucune entité ne peut porter un nom absent du glossaire métier ou du document de langage ubiquitaire.

---

# 4. Convention d'identification

Chaque entité possède un identifiant unique :

| Préfixe | Type d'entité |
|---|---|
| `ENT-IDENTITY` | Entités du contexte Identity & Access Management |
| `ENT-CATALOG` | Entités du contexte Event Catalog |
| `ENT-DISCOVERY` | Entités du contexte Event Discovery |
| `ENT-BOOKING` | Entités du contexte Booking & Availability |
| `ENT-PAYMENT` | Entités du contexte Payment Processing |
| `ENT-TICKETING` | Entités du contexte Ticketing & Fulfillment |
| `ENT-ACCESS` | Entités du contexte Access Control |
| `ENT-FINANCE` | Entités du contexte Financial Settlement |
| `ENT-REFUND` | Entités du contexte Refund Management |
| `ENT-TRUST` | Entités du contexte Trust & Safety |
| `ENT-OBSERVATION` | Entités du contexte Analytics & Observability |
| `ENT-COMMUNICATION` | Entités du contexte Communication |

---

# 5. Entités par bounded context

## 5.1. BC-01 — Identity & Access Management

### ENT-IDENTITY-01 — Utilisateur

| Champ | Valeur |
|---|---|
| **Définition** | Personne physique disposant d'un compte Eventix |
| **Identité** | Identifiant unique généré à la création du compte |
| **Cycle de vie** | Création → Activation → Suspension éventuelle → Désactivation |
| **Responsabilités** | S'authentifier, exercer les capacités de participant ou d'organisateur |

**Attributs principaux** : email ou téléphone, mot de passe, informations complémentaires.

**Invariants portés** : Un utilisateur possède au moins un moyen d'authentification (email ou téléphone).

---

### ENT-IDENTITY-02 — Compte participant

| Champ | Valeur |
|---|---|
| **Définition** | Capacité d'un utilisateur à découvrir et participer à des événements |
| **Identité** | Identifiant unique lié à l'utilisateur |
| **Cycle de vie** | Création → Activation → Suspension éventuelle |
| **Responsabilités** | Consulter l'historique participant, gérer les billets |

**Relations** : Appartient à un Utilisateur (ENT-IDENTITY-01).

---

### ENT-IDENTITY-03 — Compte organisateur

| Champ | Valeur |
|---|---|
| **Définition** | Capacité d'un utilisateur à créer et gérer des événements |
| **Identité** | Identifiant unique lié à l'utilisateur |
| **Cycle de vie** | Demande → Autorisation → Activation → Révocation éventuelle |
| **Responsabilités** | Créer des événements, consulter l'historique organisateur |

**Relations** : Appartient à un Utilisateur (ENT-IDENTITY-01).

**Invariants portés** : L'autorisation organisateur est nécessaire avant toute création d'événement.

---

### ENT-IDENTITY-04 — Organisation

| Champ | Valeur |
|---|---|
| **Définition** | Entité juridique ou informelle soumise à vérification |
| **Identité** | Identifiant unique généré à la création |
| **Cycle de vie** | Création → Vérification → Activation → Suspension ou bannissement |
| **Responsabilités** | Porter la crédibilité de l'organisateur, être soumise à vérification |

**Attributs principaux** : nom, informations de vérification, état.

**États** : `ACTIVE`, `UNDER_REVIEW`, `SUSPENDED`, `BANNED`.

---

## 5.2. BC-02 — Event Catalog

### ENT-CATALOG-01 — Événement

| Champ | Valeur |
|---|---|
| **Définition** | Activité organisée à laquelle des participants peuvent assister |
| **Identité** | Identifiant unique généré à la création |
| **Cycle de vie** | `DRAFT` → `SUBMITTED` → `UNDER_REVIEW` → `VALIDATED` → `PUBLISHED` → `ONGOING` → `COMPLETED` → `CLOSED` → `ARCHIVED` |
| **Responsabilités** | Porter la configuration, le cycle de vie et la visibilité de l'événement |

**Attributs principaux** : nom, description, date, heure, lieu, capacité, catégories de billets, prix, périodes de vente.

**États** : `DRAFT`, `SUBMITTED`, `UNDER_REVIEW`, `VALIDATED`, `PUBLISHED`, `ONGOING`, `COMPLETED`, `CLOSED`, `ARCHIVED`, `CANCELLED`.

**Invariants portés** : Un événement refusé ne peut pas être publié. Les ventes sont arrêtées automatiquement à l'heure de début.

**Relations** : Créé par un Compte organisateur (ENT-IDENTITY-03), associé à des Espaces (ENT-CATALOG-02), des Catégories de billet (ENT-CATALOG-04).

---

### ENT-CATALOG-02 — Espace

| Champ | Valeur |
|---|---|
| **Définition** | Lieu physique ou virtuel accueillant un événement |
| **Identité** | Identifiant unique généré à la création |
| **Cycle de vie** | Création → Association à un événement → Archivage avec l'événement |
| **Responsabilités** | Décrire le lieu de l'événement |

**Attributs principaux** : nom, adresse, capacité totale, type.

---

### ENT-CATALOG-03 — Zone

| Champ | Valeur |
|---|---|
| **Définition** | Subdivision d'un espace avec capacité propre |
| **Identité** | Identifiant unique généré à la création |
| **Cycle de vie** | Création → Association à un espace → Archivage avec l'événement |
| **Responsabilités** | Délimiter une capacité d'accueil spécifique |

**Attributs principaux** : nom, capacité, espace associé.

**Relations** : Appartient à un Espace (ENT-CATALOG-02).

---

### ENT-CATALOG-04 — Catégorie de billet

| Champ | Valeur |
|---|---|
| **Définition** | Type de billet commercialisé pour un événement |
| **Identité** | Identifiant unique généré à la création |
| **Cycle de vie** | Création → Association à un événement → Archivage avec l'événement |
| **Responsabilités** | Définir les conditions de vente d'un type de billet |

**Attributs principaux** : nom, prix, quantité, conditions, caractéristiques.

**Relations** : Associée à un Événement (ENT-CATALOG-01), à une Zone (ENT-CATALOG-03).

---

### ENT-CATALOG-05 — Historique de configuration

| Champ | Valeur |
|---|---|
| **Définition** | Trace non réécrite des opérations de configuration d'un événement |
| **Identité** | Identifiant unique généré à chaque opération |
| **Cycle de vie** | Enregistrement → Conservation permanente |
| **Responsabilités** | Préserver la traçabilité des modifications |

**Attributs principaux** : événement, opération, date, auteur, ancienne valeur, nouvelle valeur.

**Invariants portés** : L'historique n'est jamais réécrit par les modifications ultérieures.

---

## 5.3. BC-03 — Event Discovery

### ENT-DISCOVERY-01 — Catalogue

| Champ | Valeur |
|---|---|
| **Définition** | Ensemble des événements publiés et accessibles à la consultation |
| **Identité** | Identifiant unique du catalogue (catalogue global) |
| **Cycle de vie** | Mise à jour continue → Archivage des événements expirés |
| **Responsabilités** | Exposer les événements publiés aux participants |

**Relations** : Contient des Événements publiés (ENT-CATALOG-01).

---

### ENT-DISCOVERY-02 — Recherche

| Champ | Valeur |
|---|---|
| **Définition** | Demande de recherche émise par un participant |
| **Identité** | Identifiant unique généré à l'émission |
| **Cycle de vie** | Émission → Exécution → Résultat |
| **Responsabilités** | Transformer les critères du participant en sélection |

**Attributs principaux** : critères, date d'émission, résultat.

---

## 5.4. BC-04 — Booking & Availability

### ENT-BOOKING-01 — Réservation

| Champ | Valeur |
|---|---|
| **Définition** | Opération de blocage temporaire d'une disponibilité avant achat |
| **Identité** | Identifiant unique généré à la création |
| **Cycle de vie** | `PENDING` → `CONFIRMED` ou `EXPIRED` ou `CANCELLED` |
| **Responsabilités** | Bloquer temporairement une disponibilité, expirer après délai |

**Attributs principaux** : événement, catégorie, quantité, date de création, date d'expiration.

**États** : `PENDING`, `CONFIRMED`, `EXPIRED`, `CANCELLED`.

**Invariants portés** : Une réservation expire après cinq minutes. L'expiration libère la disponibilité sans constituer un échec de paiement.

**Relations** : Porte sur une Catégorie de billet (ENT-CATALOG-04), appartient à un Compte participant (ENT-IDENTITY-02).

---

### ENT-BOOKING-02 — Disponibilité

| Champ | Valeur |
|---|---|
| **Définition** | Quantité ou ensemble de places pouvant encore être attribuées |
| **Identité** | Identifiant unique lié à la catégorie et à l'événement |
| **Cycle de vie** | Initialisation → Décrémentation → Épuisement ou libération |
| **Responsabilités** | Tenir le compte des places encore attribuables |

**Attributs principaux** : catégorie, quantité totale, quantité disponible, quantité bloquée.

**Invariants portés** : Une même disponibilité n'est jamais attribuée simultanément à plusieurs achats valides.

**Relations** : Associée à une Catégorie de billet (ENT-CATALOG-04).

---

## 5.5. BC-05 — Payment Processing

### ENT-PAYMENT-01 — Paiement

| Champ | Valeur |
|---|---|
| **Définition** | Opération financière par laquelle l'acheteur règle le montant associé à une vente |
| **Identité** | Identifiant unique généré à l'initiation |
| **Cycle de vie** | `PENDING` → `CONFIRMED` ou `FAILED` |
| **Responsabilités** | Initier le paiement, recevoir la confirmation, traiter l'échec |

**Attributs principaux** : réservation, montant, moyen de paiement, date d'initiation, date de confirmation.

**États** : `PENDING`, `CONFIRMED`, `FAILED`.

**Invariants portés** : Une confirmation de paiement reçue plusieurs fois ne produit qu'un seul effet métier.

**Relations** : Associé à une Réservation (ENT-BOOKING-01).

---

### ENT-PAYMENT-02 — Réconciliation

| Champ | Valeur |
|---|---|
| **Définition** | Processus de rapprochement des informations d'une opération de paiement |
| **Identité** | Identifiant unique généré au déclenchement |
| **Cycle de vie** | Déclenchement → Analyse → Résolution |
| **Responsabilités** | Déterminer l'issue d'un paiement tardif |

**Attributs principaux** : paiement, réservation, disponibilité, issue (billet ou remboursement).

**Relations** : Porte sur un Paiement (ENT-PAYMENT-01).

---

## 5.6. BC-06 — Ticketing & Fulfillment

### ENT-TICKETING-01 — Achat

| Champ | Valeur |
|---|---|
| **Définition** | Opération par laquelle un acheteur obtient un ou plusieurs billets |
| **Identité** | Identifiant unique généré à la finalisation |
| **Cycle de vie** | Finalisation → Confirmation → Archivage |
| **Responsabilités** | Finaliser la transaction, donner droit à l'émission de billets |

**Attributs principaux** : réservation, paiement, acheteur, montant, date.

**Relations** : Associé à une Réservation (ENT-BOOKING-01), à un Paiement (ENT-PAYMENT-01).

---

### ENT-TICKETING-02 — Billet

| Champ | Valeur |
|---|---|
| **Définition** | Titre permettant l'accès à un événement selon les conditions définies |
| **Identité** | Identifiant unique généré à l'émission, QR Code associé |
| **Cycle de vie** | `ISSUED` → `USED` ou `CANCELLED` |
| **Responsabilités** | Porter le droit d'accès, identifier le propriétaire, être contrôlé |

**Attributs principaux** : événement, catégorie, propriétaire, QR Code, date d'émission.

**États** : `ISSUED`, `USED`, `CANCELLED`.

**Invariants portés** : Un billet a un propriétaire actif unique. Un billet validé ne peut pas être validé une seconde fois.

**Relations** : Associé à un Événement (ENT-CATALOG-01), à un Achat (ENT-TICKETING-01), à un Propriétaire (ENT-IDENTITY-02).

---

### ENT-TICKETING-03 — Transfert

| Champ | Valeur |
|---|---|
| **Définition** | Changement de propriétaire actif d'un billet sans création de copie |
| **Identité** | Identifiant unique généré à l'opération |
| **Cycle de vie** | Demande → Validation → Exécution → Archivage |
| **Responsabilités** | Modifier le propriétaire actif, conserver l'historique |

**Attributs principaux** : billet, ancien propriétaire, nouveau propriétaire, date.

**Relations** : Porte sur un Billet (ENT-TICKETING-02).

---

## 5.7. BC-07 — Access Control

### ENT-ACCESS-01 — Contrôle

| Champ | Valeur |
|---|---|
| **Définition** | Opération de vérification d'un billet au moment de l'entrée |
| **Identité** | Identifiant unique généré à l'opération |
| **Cycle de vie** | Scan → Validation → Décision → Enregistrement |
| **Responsabilités** | Vérifier l'authenticité, l'événement et l'état du billet |

**Attributs principaux** : billet, événement, contrôleur, point d'entrée, date, heure, résultat.

**Relations** : Porte sur un Billet (ENT-TICKETING-02).

---

### ENT-ACCESS-02 — Point d'entrée

| Champ | Valeur |
|---|---|
| **Définition** | Emplacement spécifique auquel un contrôleur est affecté |
| **Identité** | Identifiant unique généré à la création |
| **Cycle de vie** | Création → Association à un événement → Archivage |
| **Responsabilités** | Délimiter un accès physique à l'événement |

**Attributs principaux** : nom, événement, capacité, contrôleurs associés.

**Relations** : Associé à un Événement (ENT-CATALOG-01).

---

### ENT-ACCESS-03 — Présence

| Champ | Valeur |
|---|---|
| **Définition** | Fait qu'un participant soit effectivement entré dans l'événement |
| **Identité** | Identifiant unique généré à l'enregistrement |
| **Cycle de vie** | Enregistrement → Conservation permanente |
| **Responsabilités** | Constater l'entrée effective d'un participant |

**Attributs principaux** : billet, événement, date, heure, point d'entrée.

**Relations** : Associée à un Billet (ENT-TICKETING-02), à un Point d'entrée (ENT-ACCESS-02).

---

## 5.8. BC-08 — Financial Settlement

### ENT-FINANCE-01 — Clôture

| Champ | Valeur |
|---|---|
| **Définition** | Opération de détermination du montant net dû à l'organisateur |
| **Identité** | Identifiant unique généré au déclenchement |
| **Cycle de vie** | Préparation → Calcul → Validation → Archivage |
| **Responsabilités** | Calculer le montant net, déclencher la mise à disposition du solde |

**Attributs principaux** : événement, ventes confirmées, remboursements, frais, montant net.

**Relations** : Porte sur un Événement (ENT-CATALOG-01).

---

### ENT-FINANCE-02 — Solde organisateur

| Champ | Valeur |
|---|---|
| **Définition** | Montant résultant des opérations financières de l'organisateur |
| **Identité** | Identifiant unique lié au compte organisateur |
| **Cycle de vie** | Initialisation → Alimentation → Retraits → Solde final |
| **Responsabilités** | Tenir le montant disponible pour retrait |

**Attributs principaux** : organisateur, montant disponible, montant bloqué, historique des opérations.

**Invariants portés** : Un organisateur ne peut retirer plus que son solde disponible.

**Relations** : Appartient à un Compte organisateur (ENT-IDENTITY-03).

---

### ENT-FINANCE-03 — Retrait

| Champ | Valeur |
|---|---|
| **Définition** | Opération de versement du solde à l'organisateur |
| **Identité** | Identifiant unique généré à la demande |
| **Cycle de vie** | `PENDING` → `PROCESSING` → `COMPLETED` ou `FAILED` |
| **Responsabilités** | Demander le versement, suivre l'exécution, traiter l'échec |

**Attributs principaux** : solde, montant, date de demande, date d'exécution, état.

**États** : `PENDING`, `PROCESSING`, `FAILED`, `COMPLETED`.

**Invariants portés** : Un retrait échoué est restitué au solde.

**Relations** : Porte sur un Solde organisateur (ENT-FINANCE-02).

---

## 5.9. BC-09 — Refund Management

### ENT-REFUND-01 — Obligation de remboursement

| Champ | Valeur |
|---|---|
| **Définition** | Situation métier entraînant une restitution due |
| **Identité** | Identifiant unique généré à la détermination |
| **Cycle de vie** | Détermination → Création → Exécution → Archivage |
| **Responsabilités** | Qualifier la situation, déclencher le remboursement |

**Attributs principaux** : paiement, billet, motif, montant de référence.

**Relations** : Associée à un Paiement (ENT-PAYMENT-01), à un Billet (ENT-TICKETING-02).

---

### ENT-REFUND-02 — Remboursement

| Champ | Valeur |
|---|---|
| **Définition** | Opération de restitution tout ou partie d'un montant payé |
| **Identité** | Identifiant unique généré à la création |
| **Cycle de vie** | `PENDING` → `PROCESSING` → `COMPLETED` ou `FAILED` |
| **Responsabilités** | Calculer le montant, exécuter le versement, traiter l'échec |

**Attributs principaux** : obligation, montant, date de création, date d'exécution, état.

**États** : `PENDING`, `PROCESSING`, `FAILED`, `COMPLETED`.

**Invariants portés** : Une obligation de remboursement ne produit qu'un seul remboursement effectif.

**Relations** | Porte sur une Obligation de remboursement (ENT-REFUND-01).

---

## 5.10. BC-10 — Trust & Safety

### ENT-TRUST-01 — Signalement

| Champ | Valeur |
|---|---|
| **Définition** | Alerte émise par un participant sur un événement ou un organisateur |
| **Identité** | Identifiant unique généré à l'émission |
| **Cycle de vie** | Émission → Analyse → Décision → Archivage |
| **Responsabilités** | Porter l'alerte, alimenter l'analyse de risque |

**Attributs principaux** : émetteur, cible (événement ou organisateur), motif, description, date.

**Relations** : Émis par un Participant, cible un Événement (ENT-CATALOG-01) ou un Organisateur (ENT-IDENTITY-03).

---

### ENT-TRUST-02 — Mesure de sécurité

| Champ | Valeur |
|---|---|
| **Définition** | Décision appliquée suite à l'analyse de risque |
| **Identité** | Identifiant unique généré à la décision |
| **Cycle de vie** | Décision → Application → Suivi → Archivage |
| **Responsabilités** | Restreindre, suspendre ou bannir selon le niveau de risque |

**Attributs principaux** : cible, type (maintien, suspension, annulation, bannissement), justification, date.

**Relations** | S'applique à un Événement (ENT-CATALOG-01), un Compte (ENT-IDENTITY-01) ou une Organisation (ENT-IDENTITY-04).

---

## 5.11. BC-11 — Analytics & Observability

### ENT-OBSERVATION-01 — Événement métier

| Champ | Valeur |
|---|---|
| **Définition** | Fait marquant survenu dans un domaine |
| **Identité** | Identifiant unique généré à l'émission |
| **Cycle de vie** | Émission → Collecte → Exploitation → Archivage |
| **Responsabilités** | Porter la trace d'une opération métier importante |

**Attributs principaux** : type, domaine source, date, acteur, objet concerné, résultat.

**Relations** : Émis par tous les bounded contexts.

---

### ENT-OBSERVATION-02 — Statistique

| Champ | Valeur |
|---|---|
| **Définition** | Information calculée à partir des données Eventix |
| **Identité** | Identifiant unique généré au calcul |
| **Cycle de vie** | Calcul → Publication → Mise à jour |
| **Responsabilités** | Agréger les données d'activité pour le pilotage |

**Attributs principaux** : type (ventes, entrées, taux de remplissage), période, valeur.

**Invariants portés** : Les statistiques ne modifient pas les données métier.

---

### ENT-OBSERVATION-03 — Historique

| Champ | Valeur |
|---|---|
| **Définition** | Enregistrement chronologique des opérations importantes |
| **Identité** | Identifiant unique généré à l'enregistrement |
| **Cycle de vie** | Enregistrement → Conservation permanente |
| **Responsabilités** | Préserver la traçabilité des opérations |

**Attributs principaux** : opération, acteur, date, contexte, résultat.

**Invariants portés** : L'historique n'est jamais réécrit.

---

## 5.12. BC-12 — Communication

### ENT-COMMUNICATION-01 — Distribution

| Champ | Valeur |
|---|---|
| **Définition** | Envoi du billet par email au participant |
| **Identité** | Identifiant unique généré à l'envoi |
| **Cycle de vie** | Préparation → Envoi → Confirmation → Archivage |
| **Responsabilités** | Acheminer le billet au participant |

**Attributs principaux** : billet, destinataire, canal (email), date, statut.

**Relations** : Porte sur un Billet (ENT-TICKETING-02).

---

### ENT-COMMUNICATION-02 — Notification

| Champ | Valeur |
|---|---|
| **Définition** | Message informant d'un changement important |
| **Identité** | Identifiant unique généré à l'émission |
| **Cycle de vie** | Émission → Envoi → Confirmation → Archivage |
| **Responsabilités** | Informer les participants des annulations et reports |

**Attributs principaux** : type (annulation, report), événement, destinataires, date.

**Relations** : Porte sur un Événement (ENT-CATALOG-01).

---

# 6. Cycle de vie des entités

## 6.1. Entités à cycle de vie long

| Entité | Durée de vie | État final |
|---|---|---|
| Utilisateur | Permanente | Désactivation |
| Compte participant | Permanente | Suspension |
| Compte organisateur | Permanente | Révocation |
| Organisation | Permanente | Bannissement |
| Événement | Mois à années | Archivage |
| Billet | Semaines à mois | USED ou CANCELLED |
| Solde organisateur | Permanente | Solde final |

## 6.2. Entités à cycle de vie court

| Entité | Durée de vie | État final |
|---|---|---|
| Réservation | 5 minutes | EXPIRED ou CONFIRMED |
| Paiement | Minutes à heures | CONFIRMED ou FAILED |
| Réconciliation | Heures | Résolution |
| Contrôle | Secondes | Enregistrement |
| Transfert | Minutes | Exécution |
| Distribution | Minutes | Confirmation |
| Notification | Minutes | Confirmation |

## 6.3. Entités à conservation permanente

| Entité | Justification |
|---|---|
| Historique de configuration | Traçabilité réglementaire |
| Événement métier | Audit et analyse |
| Historique | Traçabilité des opérations |
| Présence | Preuve d'accès |
| Signalement | Traçabilité des décisions de sécurité |

---

# 7. Relations entre entités

## 7.1. Relations de composition

| Entité composante | Entité composée | Cardinalité |
|---|---|---|
| Zone | Espace | 1..n |
| Catégorie de billet | Événement | 1..n |
| Point d'entrée | Événement | 0..n |
| Billet | Achat | 1..n |

## 7.2. Relations d'association

| Entité source | Entité cible | Cardinalité | Nature |
|---|---|---|---|
| Événement | Compte organisateur | n..1 | Création |
| Réservation | Compte participant | n..1 | Appartenance |
| Réservation | Catégorie de billet | n..1 | Porte sur |
| Paiement | Réservation | 1..1 | Associe |
| Billet | Propriétaire | n..1 | Appartenance |
| Billet | Événement | n..1 | Accès |
| Contrôle | Billet | n..1 | Vérifie |
| Présence | Billet | 1..1 | Constate |
| Retrait | Solde organisateur | n..1 | Porte sur |
| Remboursement | Obligation | 1..1 | Exécute |

## 7.3. Relations transversales

| Entité source | Entité cible | Nature |
|---|---|---|
| Signalement | Événement ou Organisateur | Cible |
| Mesure de sécurité | Événement, Compte ou Organisation | S'applique à |
| Événement métier | Toutes | Émis par |
| Statistique | Toutes | Agrège |

---

# 8. Invariants portés par les entités

| Invariant | Entité principale | Entités contributrices |
|---|---|---|
| Un billet a un propriétaire actif unique | Billet | Transfert |
| Un billet validé ne peut pas être validé une seconde fois | Billet | Contrôle |
| Une réservation expire après cinq minutes | Réservation | — |
| Une même disponibilité n'est jamais attribuée simultanément à plusieurs achats valides | Disponibilité | Réservation, Achat |
| Une confirmation de paiement reçue plusieurs fois ne produit qu'un seul effet métier | Paiement | — |
| Une obligation de remboursement ne produit qu'un seul remboursement effectif | Remboursement | Obligation de remboursement |
| Un organisateur ne peut retirer plus que son solde disponible | Solde organisateur | Retrait |
| Un retrait échoué est restitué au solde | Retrait | Solde organisateur |
| Les capacités ne peuvent pas être réduites en dessous des billets déjà attribués | Événement | Catégorie de billet |
| L'historique n'est jamais réécrit | Historique | Toutes |
| Les statistiques ne modifient pas les données métier | Statistique | Toutes |

---

# 9. Résumé

Ce document identifie trente-quatre entités métier réparties dans les douze bounded contexts d'Eventix. Chaque entité possède une identité unique, un cycle de vie et des responsabilités propres. Les relations entre entités sont explicites et les invariants qu'elles portent sont identifiés. Ce document constitue la base pour la définition des objets de valeur, des agrégats et des événements métier dans les phases ultérieures.

---

# 10. Critères de qualité du document

Ce document doit respecter les propriétés suivantes :

- chaque entité possède une identité unique et stable ;
- chaque entité possède un cycle de vie explicite ;
- les responsabilités de chaque entité sont clairement délimitées ;
- les relations entre entités sont explicites et typées ;
- les invariants portés sont identifiés et vérifiables ;
- aucune décision technique n'est prise ou implicite.

---

# 11. Statut

| Champ | Valeur |
|---|---|
| **Document** | `entites.md` |
| **Version** | 1.0 |
| **Statut** | À valider par l'équipe |
| **Périmètre** | MVP Eventix |
| **Marché** | Cameroun |

| Principe | État |
|---|---|
| Identité unique par entité | ✅ APPLIQUÉ |
| Cycle de vie explicite | ✅ DÉFINI |
| Responsabilités délimitées | ✅ DÉFINIES |
| Relations explicites | ✅ DÉFINIES |
| Invariants identifiés | ✅ PORTÉS |
| Décisions techniques | ⏳ NON PRÉJUGÉES |

---

## Notes de rédaction

### Cohérence avec les sources

Ce document ne répète pas les définitions des bounded contexts ni les termes du langage ubiquitaire. Il les utilise comme références et identifie les entités métier qui en découlent.

### Granularité

La granularité retenue est celle de l'entité métier avec identité propre. Les objets de valeur (Prix, Adresse email, QR Code) seront traités dans un document ultérieur.

### Questions ouvertes

Les questions métier encore ouvertes identifiées dans les phases précédentes ne sont pas tranchées ici et restent référencées dans `questions-metier-ouvertes.md`.