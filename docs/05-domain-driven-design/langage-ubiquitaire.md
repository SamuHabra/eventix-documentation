# Langage ubiquitaire — Eventix

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
3. [Principes du langage ubiquitaire](#3-principes-du-langage-ubiquitaire)
4. [Termes par bounded context](#4-termes-par-bounded-context)
5. [Termes transversaux](#5-termes-transversaux)
6. [Termes interdits et remplacements](#6-termes-interdits-et-remplacements)
7. [Règles d'usage](#7-règles-d'usage)
8. [Évolution du langage](#8-évolution-du-langage)
9. [Résumé](#9-résumé)
10. [Critères de qualité du document](#10-critères-de-qualité-du-document)
11. [Statut](#11-statut)

---

# 1. Objectif

Ce document définit le langage ubiquitaire d'Eventix en reliant le glossaire métier établi dans la phase 03 aux bounded contexts définis dans la phase 05. Pour chaque bounded context, il précise :

- les termes métier qui appartiennent à ce contexte ;
- la signification de chaque terme dans ce contexte ;
- les termes à proscrire dans ce contexte ;
- les relations avec les termes des contextes voisins.

Il ne redéfinit pas les termes du glossaire métier ni les frontières des bounded contexts. Ces éléments sont utilisés par référence et supposés connus du lecteur.

---

# 2. Sources de référence

Ce document est dérivé principalement de :

- `03-decouverte-du-metier/glossaire-metier.md` — vocabulaire métier officiel
- `05-domain-driven-design/bounded-contexts.md` — frontières des treize bounded contexts

Toute définition, frontière ou classification mentionnée implicitement renvoie à ces documents sources.

---

# 3. Principes du langage ubiquitaire

## 3.1. Un terme, une signification

Chaque terme métier possède une signification unique et stable dans l'ensemble du projet. Un même terme ne désigne jamais deux concepts différents selon le contexte.

## 3.2. Un concept, un terme

Chaque concept métier possède un terme unique. Les synonymes sont proscrits. Si deux termes désignent le même concept, l'un est retenu et l'autre est abandonné.

## 3.3. Le terme appartient à un contexte

Chaque terme est rattaché au bounded contexte qui en détient la définition autoritaire. Les autres contextes qui utilisent ce terme en empruntent la signification sans la modifier.

## 3.4. Le terme précède le code

Aucun nom de classe, de méthode, de variable ou de base de données ne doit être créé avant que le terme métier correspondant ne soit défini dans ce document ou dans le glossaire métier.

## 3.5. Le terme est partagé

Le langage ubiquitaire est utilisé de manière identique par tous les membres de l'équipe : produit, design, développement, opérations, finance. Aucun glossaire parallèle n'est toléré.

---

# 4. Termes par bounded context

## 4.1. BC-01 — Identity & Access Management

### Termes définis dans ce contexte

| Terme | Signification dans ce contexte | Contextes emprunteurs |
|---|---|---|
| **Utilisateur** | Personne physique disposant d'un compte Eventix | Tous |
| **Compte participant** | Capacité d'un utilisateur à découvrir et participer à des événements | Tous |
| **Compte organisateur** | Capacité d'un utilisateur à créer et gérer des événements | BC-02, BC-08, BC-11 |
| **Organisation** | Entité juridique ou informelle soumise à vérification | BC-02, BC-10 |
| **Capacité** | Ensemble des droits associés à un compte | Tous |
| **Autorisation organisateur** | Décision d'octroi ou de révocation de la capacité organisateur | BC-02 |
| **Vérification d'organisation** | Processus de contrôle de crédibilité d'une organisation | BC-02, BC-10 |
| **Suspension** | Mesure temporaire limitant l'utilisation d'un compte ou d'une organisation | BC-02, BC-10 |
| **Bannissement** | Mesure définitive d'exclusion d'une organisation | BC-02, BC-10 |

### Termes proscrits dans ce contexte

| Terme proscrit | Remplacement | Raison |
|---|---|---|
| Client | Utilisateur | Évite la confusion avec le participant |
| Fournisseur | Organisateur | Évite la confusion avec un partenaire |
| Compte vendeur | Compte organisateur | Le terme vendeur n'existe pas dans le glossaire |

---

## 4.2. BC-02 — Event Catalog

### Termes définis dans ce contexte

| Terme | Signification dans ce contexte | Contextes emprunteurs |
|---|---|---|
| **Événement** | Activité organisée à laquelle des participants peuvent assister | Tous |
| **Organisateur** | Acteur responsable de la création et de l'organisation d'un événement | Tous |
| **Brouillon** | État initial d'un événement en préparation | BC-03 |
| **Soumission** | Demande de vérification avant publication | — |
| **Vérification d'événement** | Processus de contrôle d'un événement avant sa publication | BC-10 |
| **Publication** | Mise à disposition publique d'un événement validé | BC-03 |
| **Événement publié** | Événement rendu visible publiquement | BC-03, BC-04, BC-06, BC-07 |
| **Événement masqué** | Événement existant mais non visible publiquement | BC-03 |
| **Événement archivé** | Événement conservé dans l'historique mais non actif | BC-11 |
| **Événement annulé** | Événement qui ne se déroulera pas | BC-06, BC-09, BC-12 |
| **Événement reporté** | Événement déplacé à une nouvelle date | BC-06, BC-12 |
| **Espace** | Lieu physique ou virtuel accueillant un événement | — |
| **Zone** | Subdivision d'un espace avec capacité propre | — |
| **Capacité** | Nombre maximal de participants pour une zone ou catégorie | BC-04 |
| **Catégorie de billet** | Type de billet commercialisé pour un événement | BC-04, BC-06 |
| **Place** | Emplacement physique auquel un billet donne accès | — |
| **Place numérotée** | Place identifiée individuellement par un numéro | — |
| **Plan de salle** | Représentation interactive des places, zones et de leur état de disponibilité | BC-03, BC-04 |
| **Mode d'accès** | Droit d'accès associé à un billet : sur place, direct ou VOD | BC-06, BC-12 |
| **Code promotionnel** | Code permettant d'appliquer une réduction à une commande selon des conditions définies | BC-05, BC-06 |
| **Lien de suivi** | Lien partageable qui associe une visite ou une commande à une source de promotion | BC-11 |
| **Disponibilité** | Quantité ou ensemble de places pouvant encore être attribuées | BC-03, BC-04 |
| **Inventaire** | Ensemble des disponibilités gérées par Eventix | BC-04 |
| **Historique de configuration** | Trace non réécrite des opérations de configuration | BC-11 |

### Termes proscrits dans ce contexte

| Terme proscrit | Remplacement | Raison |
|---|---|---|
| Activité | Événement | Le glossaire impose « événement » |
| Ticket | Billet | Le glossaire impose « billet » en français |
| Salle | Espace | « Salle » est un cas particulier d'espace |
| Tarif | Catégorie de billet | Le tarif est une propriété, pas le concept |

---

## 4.3. BC-03 — Event Discovery

### Termes définis dans ce contexte

| Terme | Signification dans ce contexte | Contextes emprunteurs |
|---|---|---|
| **Recherche** | Fonction permettant de trouver des événements | — |
| **Filtre** | Critère de sélection appliqué aux résultats | — |
| **Détail événement** | Informations complètes d'un événement consultables | — |
| **Disponibilité visible** | Capacité restante affichée au participant | — |
| **Catalogue** | Ensemble des événements publiés et accessibles | — |

### Termes proscrits dans ce contexte

| Terme proscrit | Remplacement | Raison |
|---|---|---|
| Recherche d'activité | Recherche d'événement | Cohérence avec le glossaire |
| Liste | Catalogue | « Liste » est une présentation, pas le concept |

---

## 4.4. BC-04 — Booking & Availability

### Termes définis dans ce contexte

| Terme | Signification dans ce contexte | Contextes emprunteurs |
|---|---|---|
| **Réservation** | Opération de blocage temporaire d'une disponibilité | BC-05 |
| **Réservation PENDING** | Réservation temporairement active et non finalisée | BC-05 |
| **Réservation EXPIRED** | Réservation dont le délai est dépassé | BC-05 |
| **Blocage temporaire** | Mise en attente d'une disponibilité | — |
| **Expiration** | Libération automatique après délai | — |
| **Annulation de réservation** | Libération sur initiative du participant | — |
| **Attribution** | Association d'une disponibilité à un billet ou un propriétaire | BC-06 |
| **Arbitrage** | Garantie qu'une même disponibilité n'est jamais attribuée simultanément à plusieurs achats | — |

### Termes proscrits dans ce contexte

| Terme proscrit | Remplacement | Raison |
|---|---|---|
| Pré-réservation | Réservation | Le terme « pré-réservation » n'existe pas dans le glossaire |
| Blocage définitif | Attribution | Le blocage est temporaire par définition |

---

## 4.5. BC-05 — Payment Processing

### Termes définis dans ce contexte

| Terme | Signification dans ce contexte | Contextes emprunteurs |
|---|---|---|
| **Paiement** | Opération financière par laquelle l'acheteur règle le montant associé à une vente | BC-06, BC-09 |
| **Paiement PENDING** | Paiement dont le résultat définitif n'est pas encore connu | — |
| **Paiement CONFIRMED** | Paiement dont Eventix a reçu une confirmation fiable | BC-06 |
| **Paiement FAILED** | Paiement dont la transaction n'a pas abouti | — |
| **Mobile Money** | Moyen de paiement électronique utilisé dans le MVP | — |
| **Confirmation de paiement** | Accusé de réception positif du service de paiement | BC-06 |
| **Échec de paiement** | Opération n'ayant pas abouti | — |
| **Réconciliation** | Processus de rapprochement des informations d'une opération | BC-09 |
| **Paiement tardif** | Paiement confirmé après expiration de la réservation | BC-09 |
| **Idempotence** | Propriété d'une opération produisant un seul effet métier malgré des répétitions | — |

### Termes proscrits dans ce contexte

| Terme proscrit | Remplacement | Raison |
|---|---|---|
| Transaction | Paiement | « Transaction » est ambigu et doit être précisé |
| Paiement validé | Paiement confirmé | Le glossaire distingue confirmation et validation |
| Débit | Paiement | « Débit » est un terme technique, pas métier |

---

## 4.6. BC-06 — Ticketing & Fulfillment

### Termes définis dans ce contexte

| Terme | Signification dans ce contexte | Contextes emprunteurs |
|---|---|---|
| **Achat** | Opération par laquelle un acheteur obtient un ou plusieurs billets | BC-08, BC-11 |
| **Acheteur** | Personne qui réalise l'opération d'achat | BC-05, BC-08 |
| **Vente** | Opération commerciale enregistrée à la suite de l'obtention d'un billet | BC-08, BC-11 |
| **Vente en ligne** | Vente réalisée via les interfaces en ligne | BC-11 |
| **Billet** | Titre permettant l'accès à un événement selon les conditions définies | BC-07, BC-12 |
| **Billet gratuit** | Billet dont le prix est nul | — |
| **Billet payant** | Billet dont le prix est supérieur à zéro | — |
| **Billet ISSUED** | Billet émis et consultable | BC-07 |
| **Billet USED** | Billet ayant déjà été validé pour l'accès | BC-07 |
| **Billet CANCELLED** | Billet devenu inutilisable | BC-07 |
| **QR Code** | Code visuel associé à un billet pour son identification | BC-07 |
| **Propriétaire du billet** | Personne à laquelle le billet est actuellement attribué | BC-07 |
| **Transfert de billet** | Changement de propriétaire sans création de copie | — |
| **Historique de propriété** | Historique des propriétaires successifs d'un billet | BC-11 |
| **Finalisation d'achat** | Opération confirmant l'achat lorsque les conditions sont réunies | BC-08 |
| **Émission de billet** | Création du billet à la suite d'un achat finalisé | BC-07, BC-12 |

### Termes proscrits dans ce contexte

| Terme proscrit | Remplacement | Raison |
|---|---|---|
| Ticket | Billet | Le glossaire impose « billet » |
| Pass | Billet | Synonyme proscrit |
| Émetteur | Eventix | Le billet est émis par Eventix, pas par un émetteur distinct |

---

## 4.7. BC-07 — Access Control

### Termes définis dans ce contexte

| Terme | Signification dans ce contexte | Contextes emprunteurs |
|---|---|---|
| **Contrôle d'accès** | Opération de vérification d'un billet au moment de l'entrée | BC-11 |
| **Contrôleur** | Personne autorisée à effectuer le contrôle des billets | — |
| **Scanner** | Outil utilisé par un contrôleur pour lire le QR Code | — |
| **Scan** | Action de lecture du QR Code | — |
| **Validation du billet** | Confirmation qu'un billet peut être utilisé pour accéder | BC-06 |
| **Billet utilisé** | Billet ayant déjà donné accès | BC-06 |
| **Billet invalide** | Billet ne satisfaisant pas les conditions d'utilisation | — |
| **Entrée** | Point physique d'accès à l'événement | — |
| **Point d'entrée** | Emplacement spécifique auquel un contrôleur est affecté | — |
| **Présence** | Fait qu'un participant soit effectivement entré | BC-11 |
| **Mode dégradé** | Fonctionnement avec un seul scanner actif | — |
| **Réintégration** | Rejeu des opérations après resynchronisation | — |

### Termes proscrits dans ce contexte

| Terme proscrit | Remplacement | Raison |
|---|---|---|
| Validation technique | Validation du billet | La validation est métier, pas technique |
| Check-in | Contrôle d'accès | Anglicisme proscrit |
| Gate | Point d'entrée | Anglicisme proscrit |

---

## 4.8. BC-08 — Financial Settlement

### Termes définis dans ce contexte

| Terme | Signification dans ce contexte | Contextes emprunteurs |
|---|---|---|
| **Clôture** | Opération de détermination du montant net dû à l'organisateur | — |
| **Montant net** | Ventes confirmées moins remboursements et frais | — |
| **Solde organisateur** | Montant résultant des opérations financières de l'organisateur | — |
| **Solde disponible** | Montant immédiatement retirable | — |
| **Retrait** | Opération de versement du solde à l'organisateur | — |
| **Retrait PENDING** | Retrait en attente de traitement | — |
| **Retrait PROCESSING** | Retrait en cours d'exécution | — |
| **Retrait FAILED** | Retrait n'ayant pas abouti | — |
| **Retrait COMPLETED** | Retrait effectué avec succès | — |
| **Settlement** | Opération de règlement de l'organisateur après réconciliation | — |
| **Frais Eventix** | Montants retenus par Eventix selon les conditions commerciales | — |
| **Montant bloqué** | Montant temporairement retenu du settlement | — |

### Termes proscrits dans ce contexte

| Terme proscrit | Remplacement | Raison |
|---|---|---|
| Paiement organisateur | Settlement | Le paiement est l'opération du participant, le settlement est celui de l'organisateur |
| Commission | Frais Eventix | « Commission » est un terme commercial, « frais » est le terme métier |
| Portefeuille | Solde organisateur | « Portefeuille » suggère une fonctionnalité de paiement |

---

## 4.9. BC-09 — Refund Management

### Termes définis dans ce contexte

| Terme | Signification dans ce contexte | Contextes emprunteurs |
|---|---|---|
| **Remboursement** | Opération de restitution tout ou partie d'un montant payé | BC-08, BC-11 |
| **Obligation de remboursement** | Situation métier entraînant une restitution due | — |
| **Éligibilité au remboursement** | État indiquant qu'une opération satisfait les conditions de remboursement | — |
| **Montant de référence** | Montant effectivement payé pour la transaction concernée | — |
| **Remboursement PENDING** | Remboursement en attente | — |
| **Remboursement PROCESSING** | Remboursement en cours d'exécution | — |
| **Remboursement FAILED** | Remboursement n'ayant pas abouti techniquement | — |
| **Remboursement COMPLETED** | Remboursement effectué | — |
| **Remboursement progressif** | Traitement des remboursements par lots | — |
| **Retentative** | Nouvelle tentative après échec technique | — |

### Termes proscrits dans ce contexte

| Terme proscrit | Remplacement | Raison |
|---|---|---|
| Remboursement automatique | Remboursement | Tout remboursement suit un processus, rien n'est automatique sans règle |
| Annulation de paiement | Remboursement | L'annulation concerne l'événement, le remboursement concerne le montant |

---

## 4.10. BC-10 — Trust & Safety

### Termes définis dans ce contexte

| Terme | Signification dans ce contexte | Contextes emprunteurs |
|---|---|---|
| **Signalement** | Alerte émise par un participant sur un événement ou un organisateur | — |
| **Motif de signalement** | Raison prédéfinie obligatoire du signalement | — |
| **Description de signalement** | Précision facultative ajoutée au motif | — |
| **Analyse de risque** | Évaluation du niveau de menace identifié | — |
| **Niveau de risque** | Classification de la gravité potentielle | — |
| **Mesure de sécurité** | Décision appliquée suite à l'analyse | BC-01, BC-02 |
| **Suspension** | Mesure temporaire de restriction | BC-01, BC-02 |
| **Bannissement** | Mesure définitive d'exclusion | BC-01, BC-02 |
| **Fraude** | Comportement visant à tromper pour obtenir un avantage indu | — |
| **Faux événement** | Événement présenté comme réel mais frauduleux | — |
| **Traçabilité** | Capacité à retracer l'origine et l'historique d'une opération | BC-11 |

### Termes proscrits dans ce contexte

| Terme proscrit | Remplacement | Raison |
|---|---|---|
| Plainte | Signalement | La plainte est un concept juridique, le signalement est une alerte |
| Bannissement de compte | Bannissement | Le bannissement porte sur l'organisation, pas sur le compte |

---

## 4.11. BC-11 — Analytics & Observability

### Termes définis dans ce contexte

| Terme | Signification dans ce contexte | Contextes emprunteurs |
|---|---|---|
| **Statistique** | Information calculée à partir des données Eventix | — |
| **Vente en temps réel** | Information reflétant l'état actuel des ventes | — |
| **Taux de remplissage** | Proportion des disponibilités attribuées par rapport à la capacité | — |
| **Performance d'événement** | Ensemble d'indicateurs d'évaluation d'un événement | — |
| **Performance de catégorie** | Indicateurs d'analyse des ventes d'une catégorie | — |
| **Événement métier** | Fait marquant survenu dans un domaine | Tous |
| **Historique** | Enregistrement chronologique des opérations | — |
| **Journal** | Séquence ordonnée des événements métier | — |
| **Audit** | Examen des opérations pour vérification ou recherche d'anomalies | — |

### Termes proscrits dans ce contexte

| Terme proscrit | Remplacement | Raison |
|---|---|---|
| Métrique | Statistique | « Métrique » est un terme technique, « statistique » est le terme métier |
| Log | Journal | Anglicisme proscrit |
| Rapport | Statistique | Le rapport est une présentation, la statistique est la donnée |

---

## 4.12. BC-12 — Communication

### Termes définis dans ce contexte

| Terme | Signification dans ce contexte | Contextes emprunteurs |
|---|---|---|
| **Mise à disposition** | Accès au billet depuis le compte participant | — |
| **Distribution** | Envoi du billet par email | — |
| **Notification** | Message informant d'un changement important | — |
| **Email** | Canal de distribution utilisé | — |
| **Information participant** | Communication relative à un événement annulé ou reporté | — |

### Termes proscrits dans ce contexte

| Terme proscrit | Remplacement | Raison |
|---|---|---|
| Envoi | Distribution | « Envoi » est générique, « distribution » est le terme métier |
| Alerting | Notification | Anglicisme proscrit |
| Newsletter | Information participant | La newsletter est un concept marketing, pas métier |

---

## 4.13. BC-13 — Cybersecurity Operations

### Termes définis dans ce contexte

| Terme | Signification dans ce contexte | Contextes emprunteurs |
|---|---|---|
| **Signal de sécurité** | Fait technique observé, transmis par une source autorisée et contextualisé avec une provenance | Tous les BC sources |
| **Alerte cybersécurité** | Signal ou groupe de signaux demandant un examen humain | — |
| **Incident cybersécurité** | Dossier d'investigation regroupant alertes, constats et qualification | BC-01 à BC-12 pour les références d'actifs |
| **Qualification** | Conclusion documentée : confirmé, faux positif ou non concluant | — |
| **Décision de réponse cyber** | Décision humaine, habilitée, justifiée et traçable portant sur une réponse au système | Module propriétaire de l'actif |
| **Couverture de détection** | Sources et périodes pour lesquelles des signaux sont effectivement disponibles | — |

### Termes à ne pas confondre

| Terme | Frontière |
|---|---|
| **Signalement métier** | Relève de BC-10 Trust & Safety ; ce n'est pas une alerte cyber |
| **Mesure de sécurité métier** | Décision de BC-10, par exemple une restriction d'organisation ; elle n'est pas une décision cyber |
| **Événement métier / statistique** | Relève de BC-11 ; ne remplace ni les signaux techniques ni le journal d'audit de BC-13 |
| **Alerte** | N'est pas, à elle seule, la preuve qu'une attaque est confirmée |

### Règle de langage

Éviter « Eventix est sûr car aucune alerte n'est affichée ». Décrire la couverture des sources et leurs limites ; l'absence de signal reçu ne prouve pas l'absence d'attaque.

# 5. Termes transversaux

Certains termes sont utilisés dans plusieurs bounded contexts avec une signification identique. Ils sont définis une seule fois et référencés par tous les contextes concernés.

| Terme | Définition | Contextes utilisateurs |
|---|---|---|
| **Événement** | Activité organisée à laquelle des participants peuvent assister | Tous |
| **Participant** | Personne qui souhaite participer ou participe à un événement | Tous |
| **Organisateur** | Acteur responsable de la création et de l'organisation d'un événement | BC-01, BC-02, BC-08, BC-11 |
| **Billet** | Titre permettant l'accès à un événement | BC-06, BC-07, BC-12 |
| **Disponibilité** | Quantité ou ensemble de places pouvant encore être attribuées | BC-02, BC-03, BC-04 |
| **Paiement** | Opération financière de règlement d'une vente | BC-05, BC-06, BC-09 |
| **Remboursement** | Restitution tout ou partie d'un montant payé | BC-08, BC-09, BC-11 |
| **Présence** | Fait qu'un participant soit effectivement entré | BC-07, BC-11 |
| **Traçabilité** | Capacité à retracer l'origine et l'historique d'une opération | BC-10, BC-11 |

---

# 6. Termes interdits et remplacements

Les termes suivants sont interdits dans toute documentation, conversation ou code Eventix. Leur usage systématique est remplacé par le terme du glossaire.

| Terme interdit | Remplacement | Contexte d'interdiction |
|---|---|---|
| Ticket | Billet | Tous |
| Client | Utilisateur ou Participant selon le sens | Tous |
| Activité | Événement | Tous |
| Check-in | Contrôle d'accès | Tous |
| Gate | Point d'entrée | Tous |
| Log | Journal | Tous |
| Métrique | Statistique | Tous |
| Débit | Paiement | Tous |
| Portefeuille | Solde organisateur | Tous |
| Commission | Frais Eventix | Tous |
| Plainte | Signalement | Tous |
| Newsletter | Information participant | Tous |
| Alerting | Notification | Tous |
| Pass | Billet | Tous |
| Salle | Espace | Tous |
| Tarif | Catégorie de billet | Tous |
| Pré-réservation | Réservation | Tous |
| Marketplace | Marketplace de revente | Tous (hors MVP) |
| Quiz Live | Quiz Live | Tous (hors MVP) |
| Reels | Reels | Tous (hors MVP) |
| QR Cloud | QR Cloud | Tous (hors MVP) |

---

# 7. Règles d'usage

## 7.1. Dans la documentation

Tout document produit dans le cadre d'Eventix utilise exclusivement les termes définis dans ce document et dans le glossaire métier. Aucune exception n'est tolérée sans décision explicite de l'équipe.

## 7.2. Dans le code

Les noms de classes, de méthodes, de variables, de tables et de colonnes utilisent les termes métier définis. Les conventions de nommage technique (camelCase, snake_case, etc.) s'appliquent aux termes métier sans les modifier.

Exemples :

| Terme métier | Classe | Méthode | Table |
|---|---|---|---|
| Billet | `Billet` | `emettreBillet()` | `billet` |
| Réservation | `Reservation` | `creerReservation()` | `reservation` |
| Paiement | `Paiement` | `confirmerPaiement()` | `paiement` |
| Remboursement | `Remboursement` | `executerRemboursement()` | `remboursement` |

## 7.3. Dans les conversations

Les membres de l'équipe utilisent les termes métier dans leur signification définie. En cas de doute sur la signification d'un terme, la référence est le glossaire métier et ce document.

## 7.4. Dans les interfaces utilisateur

Les libellés visibles par les utilisateurs utilisent les termes métier dans leur signification définie. Les termes techniques ou anglicismes sont proscrits sauf si le glossaire les reconnaît.

---

# 8. Évolution du langage

## 8.1. Ajout d'un nouveau terme

Tout nouveau terme métier doit être :

1. Proposé à l'équipe avec sa définition proposée.
2. Rattaché au bounded contexte qui en détient la définition autoritaire.
3. Ajouté au glossaire métier avant ou pendant son intégration dans le modèle.
4. Référencé dans ce document avec ses contextes emprunteurs.

## 8.2. Modification d'un terme existant

Toute modification de la signification d'un terme existant doit être :

1. Signalée à l'équipe avec la justification.
2. Validée avant toute modification du code ou de la documentation.
3. Propagée à tous les documents et au code.

## 8.3. Abandon d'un terme

Tout terme devenu obsolète est marqué comme abandonné dans ce document sans être supprimé immédiatement. Il reste référencé jusqu'à ce que toutes les occurrences dans le code et la documentation soient remplacées.

---

# 9. Résumé

Ce document définit le langage ubiquitaire d'Eventix en reliant le glossaire métier de la phase 03 aux bounded contexts de la phase 05. Il établit pour chaque contexte les termes qui lui appartiennent, leur signification dans ce contexte, les termes proscrits et les règles d'usage. Il garantit qu'un terme possède une signification unique et stable dans l'ensemble du projet et que chaque concept possède un terme unique.

---

# 10. Critères de qualité du document

Ce document doit respecter les propriétés suivantes :

- chaque terme possède une signification unique et stable ;
- chaque terme est rattaché à un bounded contexte autoritaire ;
- les termes proscrits sont explicitement listés avec leur remplacement ;
- les règles d'usage sont vérifiables ;
- l'évolution du langage est encadrée ;
- aucune décision technique n'est prise ou implicite.

---

# 11. Statut

| Champ | Valeur |
|---|---|
| **Document** | `langage-ubiquitaire.md` |
| **Version** | 1.0 |
| **Statut** | À valider par l'équipe |
| **Périmètre** | MVP Eventix |
| **Marché** | Cameroun |

| Principe | État |
|---|---|
| Un terme, une signification | ✅ APPLIQUÉ |
| Un concept, un terme | ✅ APPLIQUÉ |
| Terme rattaché à un contexte | ✅ APPLIQUÉ |
| Termes proscrits listés | ✅ DÉFINIS |
| Règles d'usage établies | ✅ DÉFINIES |
| Évolution encadrée | ✅ DÉFINIE |
| Décisions techniques | ⏳ NON PRÉJUGÉES |

---

## Notes de rédaction

### Cohérence avec les sources

Ce document ne répète pas les définitions du glossaire métier ni les frontières des bounded contexts. Il les utilise comme références et établit les relations entre les termes et les contextes.

### Granularité

La granularité retenue est celle du terme métier dans son contexte d'appartenance. Les termes techniques (noms de classes, de tables, etc.) ne sont pas définis ici mais régis par les règles d'usage.

### Questions ouvertes

Les questions métier encore ouvertes identifiées dans les phases précédentes ne sont pas tranchées ici et restent référencées dans `questions-metier-ouvertes.md`.