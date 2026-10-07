# Objets de valeur — Eventix

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
3. [Définition d'un objet de valeur](#3-définition-dun-objet-de-valeur)
4. [Règles communes](#4-règles-communes)
5. [Objets de valeur par bounded context](#5-objets-de-valeur-par-bounded-context)
6. [Objets de valeur dérivés du langage ubiquitaire](#6-objets-de-valeur-dérivés-du-langage-ubiquitaire)
7. [Immutabilité et recréation](#7-immutabilité-et-recréation)
8. [Résumé](#8-résumé)
9. [Critères de qualité du document](#9-critères-de-qualité-du-document)
10. [Statut](#10-statut)

---

# 1. Objectif

Ce document identifie les objets de valeur d'Eventix en complément des entités définies dans `entites.md`. Un objet de valeur est un concept métier dépourvu d'identité propre : il est entièrement défini par ses attributs, il est immuable après création et il est remplacé, jamais modifié. Les entités d'Eventix portent dans leurs attributs des données qui ne méritent pas d'identité stable — prix, dates, adresses, quantités, motifs, périodes — mais qui méritent une définition, des règles de validation et un nom du langage ubiquitaire.

Pour chaque objet de valeur, ce document précise :

- les attributs qui le composent ;
- les règles de validation à la construction ;
- les entités ou concepts du langage ubiquitaire qui l'utilisent.

Il ne redéfinit ni les entités, ni les termes du langage ubiquitaire. Ces éléments sont utilisés par référence et supposés connus du lecteur.

---

# 2. Sources de référence

Ce document est dérivé principalement de :

- `05-domain-driven-design/entites.md` — entités et attributs dont les objets de valeur sont extraits
- `05-domain-driven-design/langage-ubiquitaire.md` — vocabulaire métier auquel chaque objet de valeur doit emprunter son nom

Toute définition de terme, tout état d'entité ou toute règle métier mentionnée implicitement renvoie à ces documents sources.

---

# 3. Définition d'un objet de valeur

## 3.1. Caractéristiques

Un objet de valeur est un objet métier qui possède :

- **Aucune identité propre** : deux objets de valeur sont égaux si leurs attributs sont égaux, quelle que soit leur origine.
- **L'immutabilité** : une fois construit, il n'est jamais modifié. Toute variation produit un nouvel objet.
- **La validité à la construction** : il ne peut exister que dans un état valide ; la validation intervient à la création, jamais après.
- **La substituabilité** : un objet de valeur peut être remplacé par un autre de même valeur sans effet sur le comportement métier.

## 3.2. Rôle dans le modèle

Les objets de valeur complètent les entités : ce sont les entités qui portent l'identité et le cycle de vie, ce sont les objets de valeur qui portent les mesures, les descriptions et les périodes. La distinction est celle établie dans `entites.md` ; elle n'est pas reprise ici.

---

# 4. Règles communes

Les règles suivantes s'appliquent à tous les objets de valeur d'Eventix :

| Règle | Énoncé |
|---|---|
| R1 — Nommage | Le nom d'un objet de valeur est un terme du langage ubiquitaire ou une composition explicite de termes du langage ubiquitaire. |
| R2 — Immutabilité | Un objet de valeur n'expose aucune opération de modification. Toute évolution produit une nouvelle instance. |
| R3 — Validation | Un objet de valeur invalide ne peut pas être construit. La construction échoue ou rejette la valeur. |
| R4 — Égalité | L'égalité entre objets de valeur se fonde exclusivement sur leurs attributs. |
| R5 — Cycle de vie | Un objet de valeur n'a ni création, ni archivage propres : sa durée de vie est celle de l'entité ou de l'attribut qui le contient. |
| R6 — Aucune décision technique | Ce document n'impose ni type de stockage, ni format de sérialisation, ni bibliothèque de validation. |

---

# 5. Objets de valeur par bounded context

Les objets de valeur ci-dessous sont extraits des attributs des entités définies dans `entites.md`. Lorsqu'un attribut correspond à un terme défini dans le langage ubiquitaire, la définition du terme est référencée et non reprise.

## 5.1. BC-01 — Identity & Access Management

| Objet de valeur | Attributs | Validation | Utilisé par |
|---|---|---|---|
| Adresse email | adresse | Format email valide | Utilisateur (ENT-IDENTITY-01) |
| Numéro de téléphone | indicatif, numéro | Indicatif camerounais (+237) ou indicatif reconnu ; format numérique | Utilisateur (ENT-IDENTITY-01) |
| Moyen d'authentification | type (email ou téléphone), référence | Au moins un moyen requis pour un utilisateur actif — invariant porté par Utilisateur | Utilisateur (ENT-IDENTITY-01) |

## 5.2. BC-02 — Event Catalog

| Objet de valeur | Attributs | Validation | Utilisé par |
|---|---|---|---|
| Nom d'événement | libellé | Non vide, longueur bornée | Événement (ENT-CATALOG-01) |
| Description d'événement | texte | Peut être vide | Événement (ENT-CATALOG-01) |
| Date d'événement | jour, mois, année | Date valide du calendrier | Événement (ENT-CATALOG-01) |
| Heure d'événement | heure, minute | Plage horaire valide — l'invariant d'arrêt automatique des ventes à l'heure de début est porté par Événement | Événement (ENT-CATALOG-01) |
| Lieu | nom, adresse, type (physique ou virtuel) | Adresse requise pour un lieu physique ; référence d'accès requise pour un lieu virtuel | Espace (ENT-CATALOG-02) |
| Mode d'accès | sur place, direct, VOD | Au moins un mode associé à une catégorie ; le billet hérite des modes autorisés | Événement (ENT-CATALOG-01), Catégorie de billet (ENT-CATALOG-04), Billet (ENT-TICKETING-02) |
| Réduction promotionnelle | valeur, conditions | Valeur définie par l'organisateur ; conditions valides et applicables à la commande | Code promotionnel (ENT-CATALOG-07), Achat (ENT-TICKETING-01) |
| Position de siège | repère, position graphique | Référence à une place numérotée du plan de salle | Plan de salle (ENT-CATALOG-06), Billet (ENT-TICKETING-02) |
| Prix | montant, devise | Montant nul ou positif — la devise du MVP est celle du marché camerounais | Catégorie de billet (ENT-CATALOG-04), Paiement (ENT-PAYMENT-01) |
| Type de catégorie de billet | Standard ou pass multi-jours | Type pris en charge par la configuration de billetterie | Catégorie de billet (ENT-CATALOG-04) |
| Quantité | nombre | Strictement positive pour une catégorie commercialisée | Catégorie de billet (ENT-CATALOG-04) |
| Conditions de vente | texte | Peut être vide ; précise les restrictions de la catégorie | Catégorie de billet (ENT-CATALOG-04) |
| Période de vente | date de début, date de fin | Date de fin postérieure ou égale à la date de début | Événement (ENT-CATALOG-01), Catégorie de billet (ENT-CATALOG-04) |
| Dates de validité du pass | date de début, date de fin | Dates incluses dans la période de l'événement et dans l'ordre chronologique | Catégorie de billet (ENT-CATALOG-04), Billet (ENT-TICKETING-02) |
| Quota d'entrées | nombre maximal, nombre consommé | Nombre maximal strictement positif ; nombre consommé compris entre zéro et le maximum | Catégorie de billet (ENT-CATALOG-04), Billet (ENT-TICKETING-02) |
| Informations de vérification | éléments soumis, nature de l'élément | Coherentes avec l'état de vérification porté par Organisation | Organisation (ENT-IDENTITY-04) |

## 5.3. BC-03 — Event Discovery

| Objet de valeur | Attributs | Validation | Utilisé par |
|---|---|---|---|
| Critères de recherche | mots-clés, filtres, période | Au moins un critère non vide ; le terme Filtre est défini dans le langage ubiquitaire | Recherche (ENT-DISCOVERY-02) |
| Résultat de recherche | événements sélectionnés, nombre de résultats | Contient uniquement des événements publiés | Recherche (ENT-DISCOVERY-02) |

## 5.4. BC-04 — Booking & Availability

| Objet de valeur | Attributs | Validation | Utilisé par |
|---|---|---|---|
| Quantité réservée | nombre | Strictement positive | Réservation (ENT-BOOKING-01) |
| Délai d'expiration | durée, date de création | La durée de cinq minutes et la libération de la disponibilité sont des invariants portés par Réservation ; le délai est un paramètre, non une constante implicite | Réservation (ENT-BOOKING-01) |
| Attribution | disponibilité ou place numérotée, bénéficiaire, date | Référence une disponibilité ou un siège existant ; l'arbitrage simultané est un invariant porté par Disponibilité | Réservation (ENT-BOOKING-01), Achat (ENT-TICKETING-01) |

## 5.5. BC-05 — Payment Processing

| Objet de valeur | Attributs | Validation | Utilisé par |
|---|---|---|---|
| Montant du paiement | montant, devise | Strictement positif | Paiement (ENT-PAYMENT-01) |
| Moyen de paiement | type (Mobile Money dans le MVP), opérateur, référence | Le terme Mobile Money est défini dans le langage ubiquitaire ; l'opérateur doit être reconnu | Paiement (ENT-PAYMENT-01) |
| Issue de réconciliation | issue (billet ou remboursement), justification | Issue requise ; les deux issues possibles renvoient aux entités Billet et Remboursement | Réconciliation (ENT-PAYMENT-02) |

## 5.6. BC-06 — Ticketing & Fulfillment

| Objet de valeur | Attributs | Validation | Utilisé par |
|---|---|---|---|
| Montant de l'achat | montant total, devise | Positif ou nul — un montant nul correspond à un billet gratuit sans don ; le montant total distingue le billet et le don éventuel | Achat (ENT-TICKETING-01) |
| Montant du don | montant, devise | Strictement positif ; devise identique à celle de la commande | Achat (ENT-TICKETING-01) |
| Contenu de l'achat | billets, quantité par catégorie, code promotionnel éventuel, source de suivi éventuelle | Au moins un billet ; cohérent avec la réservation associée ; réduction et attribution conservées séparément du prix nominal | Achat (ENT-TICKETING-01) |
| Historique de propriété | propriétaires successifs, dates de transfert | Le terme est défini dans le langage ubiquitaire ; chronologique et non réécrit | Billet (ENT-TICKETING-02) |

## 5.7. BC-07 — Access Control

| Objet de valeur | Attributs | Validation | Utilisé par |
|---|---|---|---|
| Résultat de contrôle | issue (validation ou refus), motif | Issue requise en cas de refus | Contrôle (ENT-ACCESS-01) |
| Point et moment de passage | point d'entrée, date, heure | Référence un point d'entrée existant | Contrôle (ENT-ACCESS-01), Présence (ENT-ACCESS-03) |

## 5.8. BC-08 — Financial Settlement

| Objet de valeur | Attributs | Validation | Utilisé par |
|---|---|---|---|
| Montant net | ventes confirmées, remboursements, frais | Résultat du calcul porté par Clôture ; les termes Montant net et Frais Eventix sont définis dans le langage ubiquitaire | Clôture (ENT-FINANCE-01) |
| Position de solde | montant disponible, montant bloqué | Montants positifs ou nuls ; le terme Montant bloqué est défini dans le langage ubiquitaire ; l'invariant de retrait maximal est porté par Solde organisateur | Solde organisateur (ENT-FINANCE-02) |
| Montant de retrait | montant, devise | Positif et inférieur ou égal au solde disponible | Retrait (ENT-FINANCE-03) |

## 5.9. BC-09 — Refund Management

| Objet de valeur | Attributs | Validation | Utilisé par |
|---|---|---|---|
| Motif de remboursement | motif | Non vide ; distinc à porter du motif de signalement, terme du langage ubiquitaire | Obligation de remboursement (ENT-REFUND-01) |
| Montant remboursé | montant, devise | Positif et inférieur ou égal au montant de référence — terme défini dans le langage ubiquitaire | Remboursement (ENT-REFUND-02) |

## 5.10. BC-10 — Trust & Safety

| Objet de valeur | Attributs | Validation | Utilisé par |
|---|---|---|---|
| Motif de signalement | motif prédéfini | Motif issu de la liste prédéfinie — terme défini dans le langage ubiquitaire | Signalement (ENT-TRUST-01) |
| Description de signalement | texte | Facultative ; complète le motif sans le remplacer — terme défini dans le langage ubiquitaire | Signalement (ENT-TRUST-01) |
| Niveau de risque | classification | Issu de l'analyse de risque — terme défini dans le langage ubiquitaire | Mesure de sécurité (ENT-TRUST-02) |

## 5.11. BC-11 — Analytics & Observability

| Objet de valeur | Attributs | Validation | Utilisé par |
|---|---|---|---|
| Période statistique | date de début, date de fin | Date de fin postérieure ou égale à la date de début | Statistique (ENT-OBSERVATION-02) |
| Valeur statistique | valeur, unité | Unité cohérente avec le type de la statistique | Statistique (ENT-OBSERVATION-02) |
| Source de suivi | lien, partenaire ou campagne | Référence à un Lien de suivi existant ; règle d'attribution définie avant calcul | Lien de suivi (ENT-OBSERVATION-04), Statistique (ENT-OBSERVATION-02) |
| Contenu d'événement métier | type, domaine source, acteur, objet concerné, résultat | Type et domaine source non vides | Événement métier (ENT-OBSERVATION-01) |

## 5.12. BC-12 — Communication

| Objet de valeur | Attributs | Validation | Utilisé par |
|---|---|---|---|
| Destinataire | adresse email | Format email valide — le terme Email est défini dans le langage ubiquitaire | Distribution (ENT-COMMUNICATION-01), Notification (ENT-COMMUNICATION-02) |
| Contenu de notification | type (annulation ou report), événement, message | Type requis ; le terme Information participant est défini dans le langage ubiquitaire | Notification (ENT-COMMUNICATION-02) |

---

# 6. Objets de valeur dérivés du langage ubiquitaire

Certains termes du langage ubiquitaire désignent des concepts sans identité propre ; ils prennent corps comme objets de valeur lorsqu'ils matérialisent un attribut d'une entité. Leur signification est celle du langage ubiquitaire et n'est pas reprise ici ; seule la contrainte de modélisation est ajoutée.

| Terme du langage ubiquitaire | Contrat d'objet de valeur |
|---|---|
| Capacité | Quantité bornée, positive, attachée à une zone ou à une catégorie de billet ; la réduction sous les billets déjà attribués est un invariant porté par Événement |
| Disponibilité visible | Quantité restante affichable au participant ; dérivée de Disponibilité (ENT-BOOKING-02), sans cycle de vie propre |
| Place | Référence à un emplacement physique ; identifiée par ses attributs, non par un identifiant stable |
| Place numérotée | Place dont le numéro fait partie de la valeur |
| QR Code | Code visuel associé à un billet ; sa valeur dérive de l'identité du billet et n'en est pas indépendante |
| Montant net | Résultat d'un calcul, jamais saisi directement |
| Solde disponible | Portion du solde immédiatement retirable — terme défini dans le langage ubiquitaire |
| Montant de référence | Montant effectivement payé servant de borne aux remboursements — terme défini dans le langage ubiquitaire |
| Frais Eventix | Montant retenu selon les conditions commerciales — terme défini dans le langage ubiquitaire |
| Motif de signalement | Motif prédéfini obligatoire — terme défini dans le langage ubiquitaire |
| Niveau de risque | Classification issue de l'analyse — terme défini dans le langage ubiquitaire |

---

# 7. Immutabilité et recréation

Les objets de valeur ne sont jamais modifiés. Les situations d'évolution se résolvent par substitution :

| Situation | Traitement |
|---|---|
| Correction d'un prix avant publication | Création d'un nouvel objet Prix ; le précédent demeure dans l'historique de configuration |
| Déplacement d'un événement | Nouvelles Date d'événement et Heure d'événement ; les valeurs précédentes restent traçables |
| Changement de propriétaire d'un billet | Le Transfert (entité) enregistre l'opération ; l'historique de propriété (objet de valeur) est reconstruit, non modifié |
| Mise à jour d'une statistique | Nouvelle Valeur statistique pour la période ; les valeurs antérieures sont conservées |
| Échec d'un retrait | L'invariant de restitution au solde porté par Retrait s'exprime par la création d'une nouvelle Position de solde |

---

# 8. Résumé

Ce document complète les trente-quatre entités d'Eventix en identifiant les objets de valeur extraits de leurs attributs et des termes du langage ubiquitaire dépourvus d'identité propre. Chaque objet de valeur est nommé d'après le langage ubiquitaire, validé à la construction, immuable et substituable. Ce document constitue la base pour la définition des agrégats dans la phase ultérieure : un agrégat sera formé d'une entité racine et des objets de valeur qu'elle contient.

---

# 9. Critères de qualité du document

Ce document doit respecter les propriétés suivantes :

- chaque objet de valeur est dépourvu d'identité propre ;
- chaque objet de valeur est immuable et validé à la construction ;
- chaque objet de valeur emprunte son nom au langage ubiquitaire ;
- chaque objet de valeur est rattaché à au moins une entité ou un terme du langage ubiquitaire ;
- aucune règle métier portée par une entité n'est redéfinie ici ;
- aucune décision technique n'est prise ou implicite.

---

# 10. Statut

| Champ | Valeur |
|---|---|
| **Document** | `objets-valeur.md` |
| **Version** | 1.0 |
| **Statut** | À valider par l'équipe |
| **Périmètre** | MVP Eventix |
| **Marché** | Cameroun |

| Principe | État |
|---|---|
| Aucune identité propre | ✅ APPLIQUÉ |
| Immutabilité | ✅ APPLIQUÉE |
| Validation à la construction | ✅ APPLIQUÉE |
| Nommage issu du langage ubiquitaire | ✅ APPLIQUÉ |
| Rattachement aux entités | ✅ DÉFINI |
| Décisions techniques | ⏳ NON PRÉJUGÉES |

---

## Notes de rédaction

### Cohérence avec les sources

Ce document ne répète ni les définitions des entités ni celles des termes du langage ubiquitaire. Il extrait des attributs des entités les concepts sans identité propre et n'ajoute que ce qui leur est propre : composition, validation, contrats d'usage.

### Granularité

La granularité retenue est celle du concept sans identité propre réutilisable. Les attributs purement techniques ou les valeurs libres sans règles (identifiants opaques, libellés arbitraires) ne sont pas élevés au rang d'objet de valeur.

### Questions ouvertes

Les questions métier encore ouvertes identifiées dans les phases précédentes ne sont pas tranchées ici et restent référencées dans `questions-metier-ouvertes.md`.