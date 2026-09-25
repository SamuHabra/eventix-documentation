# Agrégats — Eventix

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
3. [Définition d'un agrégat](#3-définition-dun-agrégat)
4. [Règles de découpage](#4-règles-de-découpage)
5. [Agrégats par bounded context](#5-agrégats-par-bounded-context)
6. [Invariants assurés par les frontières](#6-invariants-assurés-par-les-frontières)
7. [Références inter-agrégats](#7-références-inter-agrégats)
8. [Résumé](#8-résumé)
9. [Critères de qualité du document](#9-critères-de-qualité-du-document)
10. [Statut](#10-statut)

---

# 1. Objectif

Ce document regroupe les entités et les objets de valeur définis dans `entites.md` et `objets-valeur.md` en agrégats cohérents. Un agrégat est une frontière de cohérence : tout ce qui se trouve à l'intérieur doit rester consistent après chaque opération métier. Pour chaque agrégat, ce document précise :

- l'entité racine et ses membres ;
- les invariants qui doivent être garantis à l'intérieur de la frontière ;
- les références vers d'autres agrégats, toujours par identité.

Il ne redéfinit ni les entités, ni les objets de valeur, ni les invariants déjà portés par les entités. Ces éléments sont utilisés par référence et supposés connus du lecteur.

---

# 2. Sources de référence

Ce document est dérivé principalement de :

- `05-domain-driven-design/entites.md` — entités, relations et invariants à faire respecter
- `05-domain-driven-design/objets-valeur.md` — objets de valeur rattachés aux entités

Toute définition d'entité, d'objet de valeur ou d'invariant mentionnée implicitement renvoie à ces documents sources.

---

# 3. Définition d'un agrégat

## 3.1. Caractéristiques

Un agrégat est un groupement d'entités et d'objets de valeur qui possède :

- **Une racine** : une seule entité, désignée comme racine, par laquelle tout accès à l'agrégat transite.
- **Une frontière de cohérence** : les invariants de l'agrégat sont garantis à la fin de chaque opération le modifiant.
- **Des membres** : entités et objets de valeur qui ne sont pas accessibles directement depuis l'extérieur ; l'extérieur ne connaît que leur identité ou leur valeur via la racine.

## 3.2. Rôle dans le modèle

Les entités portent l'identité et le cycle de vie ; les objets de valeur portent les mesures et les descriptions ; les agrégats délimitent les zones dans lesquelles ces éléments doivent rester cohérents ensemble. Les relations de composition identifiées dans `entites.md` constituent le point de départ du découpage, qui est affiné par les invariants.

---

# 4. Règles de découpage

Les règles suivantes ont guidé le regroupement :

| Règle | Énoncé |
|---|---|
| D1 — Invariant interne | Deux entités liées par un invariant devant rester vrai à tout instant appartiennent au même agrégat. |
| D2 — Référence par identité | Deux agrégats ne se référencent que par identifiant, jamais par objet complet. |
| D3 — Petites frontières | Un agrégat ne contient que ce que ses invariants exigent ; le doute tranche en faveur de deux agrégats. |
| D4 — Cycle de vie partagé | Une entité créée, archivée ou supprimée en même temps que sa racine en est membre. |
| D5 — Nommage | Un agrégat porte le nom du terme du langage ubiquitaire correspondant à sa racine. |

---

# 5. Agrégats par bounded context

## 5.1. BC-01 — Identity & Access Management

### Agrégat Utilisateur — racine : Utilisateur (ENT-IDENTITY-01)

| Membres | Objets de valeur |
|---|---|
| Compte participant (ENT-IDENTITY-02), Compte organisateur (ENT-IDENTITY-03) | Adresse email, Numéro de téléphone, Moyen d'authentification |

**Invariants assurés à la frontière** : l'existence d'au moins un moyen d'authentification ; la présence d'une autorisation organisateur avant activation du Compte organisateur.

### Agrégat Organisation — racine : Organisation (ENT-IDENTITY-04)

| Membres | Objets de valeur |
|---|---|
| — | Informations de vérification |

**Justification** : l'Organisation a son propre cycle de vie de vérification, indépendant du compte de l'utilisateur qui la porte ; elle référence le Compte organisateur par identité.

---

## 5.2. BC-02 — Event Catalog

### Agrégat Événement — racine : Événement (ENT-CATALOG-01)

| Membres | Objets de valeur |
|---|---|
| Espace (ENT-CATALOG-02), Zone (ENT-CATALOG-03), Catégorie de billet (ENT-CATALOG-04), Historique de configuration (ENT-CATALOG-05) | Nom d'événement, Description d'événement, Date d'événement, Heure d'événement, Prix, Quantité, Conditions de vente, Période de vente, Capacité |

**Invariants assurés à la frontière** : les capacités ne peuvent pas être réduites en dessous des billets déjà attribués — cet invariant relie l'Événement à ses Catégories de billet et impose une frontière commune ; les transitions d'état de l'Événement et l'arrêt automatique des ventes ; la non-réécriture de l'Historique de configuration.

**Justification** : Espace, Zone et Catégorie de billet partagent le cycle de vie de l'Événement (archivage commun), conformément à la règle D4 et aux relations de composition.

---

## 5.3. BC-03 — Event Discovery

### Pas d'agrégat de cohérence — Catalogue et Recherche

Le Catalogue expose des Événements publiés auxquels il se réfère par identité ; il ne possède pas d'invariant de cohérence propre — sa mise à jour est dérivée de la publication dans l'Agrégat Événement. La Recherche est une opération transitoire (émission, exécution, résultat) sans état à protéger.

| Entité | Rattachement |
|---|---|
| Catalogue (ENT-DISCOVERY-01) | Vue en lecture de l'Agrégat Événement |
| Recherche (ENT-DISCOVERY-02) | Aucune frontière ; opération |

**Objets de valeur associés** : Critères de recherche, Résultat de recherche.

---

## 5.4. BC-04 — Booking & Availability

### Agrégat Réservation — racine : Réservation (ENT-BOOKING-01)

| Membres | Objets de valeur |
|---|---|
| — | Quantité réservée, Délai d'expiration, Attribution |

**Invariants assurés à la frontière** : l'expiration après cinq minutes et la libération de la disponibilité sans échec de paiement.

### Agrégat Disponibilité — racine : Disponibilité (ENT-BOOKING-02)

| Membres | Objets de valeur |
|---|---|
| — | Capacité, Disponibilité visible |

**Invariants assurés à la frontière** : une même disponibilité n'est jamais attribuée simultanément à plusieurs achats valides.

**Justification** : la règle D3 sépare les deux agrégats. La Disponibilité concentre un invariant d'arbitrage à haute contention ; le fait qu'il soit garanti dans sa propre frontière évite d'élargir celle de l'Événement à chaque blocage temporaire. L'Agrégat Réservation référence la Disponibilité par identité.

---

## 5.5. BC-05 — Payment Processing

### Agrégat Paiement — racine : Paiement (ENT-PAYMENT-01)

| Membres | Objets de valeur |
|---|---|
| — | Montant du paiement, Moyen de paiement |

**Invariants assurés à la frontière** : la confirmation multiple ne produit qu'un seul effet métier.

### Agrégat Réconciliation — racine : Réconciliation (ENT-PAYMENT-02)

| Membres | Objets de valeur |
|---|---|
| — | Issue de réconciliation |

**Justification** : la Réconciliation a son propre cycle de vie (déclenchement → analyse → résolution) et référence le Paiement, la Réservation et la Disponibilité par identité. L'issue (billet ou remboursement) déclenche une opération dans l'agrégat cible sans jamais le modifier directement.

---

## 5.6. BC-06 — Ticketing & Fulfillment

### Agrégat Achat — racine : Achat (ENT-TICKETING-01)

| Membres | Objets de valeur |
|---|---|
| — | Montant de l'achat, Contenu de l'achat |

**Invariants assurés à la frontière** : la cohérence entre le contenu de l'achat et la réservation associée ; la correspondance entre l'achat et le paiement confirmé.

### Agrégat Billet — racine : Billet (ENT-TICKETING-02)

| Membres | Objets de valeur |
|---|---|
| Transfert (ENT-TICKETING-03) | Historique de propriété, QR Code |

**Invariants assurés à la frontière** : le propriétaire actif unique ; la non-validation d'un billet déjà validé ; la traçabilité des propriétaires successifs.

**Justification** : le Billet survit à son achat (transferts, contrôles, annulations) ; le rattacher à l'Agrégat Achat violerait la règle D4. L'Historique de propriété est reconstruit par les Transferts, membres de la même frontière. L'Agrégat Billet référence l'Événement et l'Achat par identité.

---

## 5.7. BC-07 — Access Control

### Agrégat Point d'entrée — racine : Point d'entrée (ENT-ACCESS-02)

| Membres | Objets de valeur |
|---|---|
| Contrôle (ENT-ACCESS-01), Présence (ENT-ACCESS-03) | Résultat de contrôle, Point et moment de passage |

**Invariants assurés à la frontière** : la cohérence entre un Contrôle validé et l'enregistrement de la Présence au même point d'entrée.

**Justification** : Contrôle et Présence naissent et s'achèvent avec le passage au point d'entrée (règle D4). L'Agrégat référence le Billet par identité : la décision de validation remonte à l'Agrégat Billet, seul habilité à faire évoluer son état.

---

## 5.8. BC-08 — Financial Settlement

### Agrégat Clôture — racine : Clôture (ENT-FINANCE-01)

| Membres | Objets de valeur |
|---|---|
| — | Montant net |

**Invariants assurés à la frontière** : la cohérence du calcul entre ventes confirmées, remboursements et frais. L'Agrégat Clôture référence l'Événement par identité et déclenche l'alimentation de l'Agrégat Solde organisateur.

### Agrégat Solde organisateur — racine : Solde organisateur (ENT-FINANCE-02)

| Membres | Objets de valeur |
|---|---|
| Retrait (ENT-FINANCE-03) | Position de solde, Montant de retrait |

**Invariants assurés à la frontière** : l'interdiction de retirer plus que le solde disponible ; la restitution au solde d'un retrait échoué.

**Justification** : la restitution d'un retrait échoué exige la cohérence atomique entre le Retrait et le Solde (règle D1) ; le Retrait est donc membre de l'agrégat malgré son cycle de vie propre.

---

## 5.9. BC-09 — Refund Management

### Agrégat Obligation de remboursement — racine : Obligation de remboursement (ENT-REFUND-01)

| Membres | Objets de valeur |
|---|---|
| Remboursement (ENT-REFUND-02) | Motif de remboursement, Montant remboursé, Montant de référence |

**Invariants assurés à la frontière** : une obligation ne produit qu'un seul remboursement effectif ; le montant remboursé ne dépasse pas le montant de référence.

**Justification** : l'unicité du remboursement effectif relie l'Obligation et le Remboursement dans une même frontière (règle D1).

---

## 5.10. BC-10 — Trust & Safety

### Agrégat Signalement — racine : Signalement (ENT-TRUST-01)

| Membres | Objets de valeur |
|---|---|
| — | Motif de signalement, Description de signalement |

### Agrégat Mesure de sécurité — racine : Mesure de sécurité (ENT-TRUST-02)

| Membres | Objets de valeur |
|---|---|
| — | Niveau de risque |

**Justification** : les deux agrégats ont des cycles de vie distincts (émission → analyse → décision d'un côté, décision → application → suivi de l'autre). La Mesure de sécurité référence sa cible — Événement, Compte ou Organisation — par identité et déclenche la transition d'état correspondante dans l'agrégat cible.

---

## 5.11. BC-11 — Analytics & Observability

### Agrégats à membre unique

| Entité racine | Justification |
|---|---|
| Événement métier (ENT-OBSERVATION-01) | Enregistrement émis par tous les contexts ; aucun invariant reliant plusieurs entités |
| Statistique (ENT-OBSERVATION-02) | Valeur calculée par période ; l'invariant de non-modification des données métier se lit comme une absence de référence vers d'autres agrégats |
| Historique (ENT-OBSERVATION-03) | Enregistrement permanent ; aucun membre |

**Objets de valeur associés** : Contenu d'événement métier, Période statistique, Valeur statistique.

---

## 5.12. BC-12 — Communication

### Agrégats à membre unique

| Entité racine | Justification |
|---|---|
| Distribution (ENT-COMMUNICATION-01) | Opération ponctuelle portant sur un billet référencé par identité |
| Notification (ENT-COMMUNICATION-02) | Message émis à partir d'un événement référencé par identité |

**Objets de valeur associés** : Destinataire, Contenu de notification.

---

# 6. Invariants assurés par les frontières

Le tableau croise les invariants portés par les entités avec les agrégats qui les garantissent. Les invariants sont énoncés dans `entites.md` et ne sont pas repris.

| Invariant | Frontière de garantie | Collaborateurs externes |
|---|---|---|
| Propriétaire actif unique du billet | Agrégat Billet | — |
| Non-double validation du billet | Agrégat Billet | Agrégat Point d'entrée (source de la décision) |
| Expiration de la réservation et libération | Agrégat Réservation | Agrégat Disponibilité (libération) |
| Non-attribution simultanée d'une disponibilité | Agrégat Disponibilité | Agrégat Réservation, Agrégat Achat |
| Unicité d'effet d'une confirmation de paiement | Agrégat Paiement | — |
| Unicité du remboursement effectif | Agrégat Obligation de remboursement | — |
| Retrait maximal et restitution d'un retrait échoué | Agrégat Solde organisateur | — |
| Non-réduction des capacités sous les billets attribués | Agrégat Événement | Agrégat Disponibilité (billets déjà attribués, par identité) |
| Non-réécriture de l'historique | Agrégat Événement (Historique de configuration), Agrégat Historique | — |
| Neutralité des statistiques | Agrégat Statistique (absence de référence sortante) | — |

**Note de cohérence** : l'invariant de non-réduction des capacités croise deux agrégats. La frontière de l'Agrégat Événement garantit la règle pour toute modification de configuration ; la lecture des billets déjà attribués se fait par identité depuis l'Agrégat Disponibilité. Le traitement est séquentiel : aucune modification de capacité ne se décide sans cette lecture.

---

# 7. Références inter-agrégats

Les références sortantes de chaque agrégat, toujours par identité conformément à la règle D2 :

| Agrégat | Référence vers |
|---|---|
| Utilisateur | — |
| Organisation | Compte organisateur |
| Événement | Compte organisateur |
| Réservation | Compte participant, Catégorie de billet, Disponibilité |
| Disponibilité | Catégorie de billet |
| Paiement | Réservation |
| Réconciliation | Paiement, Réservation, Disponibilité |
| Achat | Réservation, Paiement |
| Billet | Événement, Achat, Propriétaire (Compte participant) |
| Point d'entrée | Événement, Billet |
| Clôture | Événement, Solde organisateur |
| Solde organisateur | Compte organisateur |
| Obligation de remboursement | Paiement, Billet |
| Signalement | Participant, Événement ou Compte organisateur |
| Mesure de sécurité | Événement, Compte ou Organisation |
| Événement métier | Tous (émetteurs) |
| Statistique | Tous (sources agrégées) |
| Distribution | Billet |
| Notification | Événement |

---

# 8. Résumé

Ce document regroupe les trente-quatre entités et les objets de valeur d'Eventix en seize agrégats répartis dans les douze bounded contexts. Chaque frontière est justifiée par un invariant à garantir ou par un cycle de vie partagé, conformément aux règles de découpage D1 à D5. Les agrégats se référencent exclusivement par identité. Ce document constitue la base pour la définition des événements métier et des politiques de cohérence dans les phases ultérieures.

---

# 9. Critères de qualité du document

Ce document doit respecter les propriétés suivantes :

- chaque agrégat possède une racine unique et explicite ;
- chaque frontière est justifiée par un invariant ou un cycle de vie partagé ;
- aucune entité ne figure dans deux agrégats ;
- toute référence inter-agrégats se fait par identité ;
- aucune définition d'entité ou d'objet de valeur n'est reprise des documents sources ;
- aucune décision technique n'est prise ou implicite.

---

# 10. Statut

| Champ | Valeur |
|---|---|
| **Document** | `agregats.md` |
| **Version** | 1.0 |
| **Statut** | À valider par l'équipe |
| **Périmètre** | MVP Eventix |
| **Marché** | Cameroun |

| Principe | État |
|---|---|
| Racine unique par agrégat | ✅ APPLIQUÉ |
| Frontières justifiées | ✅ APPLIQUÉ |
| Appartenance exclusive des entités | ✅ VÉRIFIÉE |
| Références par identité | ✅ APPLIQUÉ |
| Cohérence avec les sources | ✅ RESPECTÉE |
| Décisions techniques | ⏳ NON PRÉJUGÉES |

---

## Notes de rédaction

### Cohérence avec les sources

Ce document ne répète ni les définitions des entités, ni celles des objets de valeur, ni les énoncés des invariants. Il organise ces éléments en frontières de cohérence et ajoute uniquement les justifications de découpage.

### Choix structurants

Trois choix méritent une validation par l'équipe : la séparation des agrégats Réservation et Disponibilité (règle D3 appliquée à un invariant d'arbitrage), l'indépendance de l'Agrégat Billet vis-à-vis de l'Agrégat Achat, et l'absence d'agrégat de cohérence en Event Discovery.

### Questions ouvertes

Les questions métier encore ouvertes identifiées dans les phases précédentes ne sont pas tranchées ici et restent référencées dans `questions-metier-ouvertes.md`.