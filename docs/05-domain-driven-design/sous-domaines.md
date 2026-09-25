# Sous-domaines — Eventix

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
2. [Source de référence](#2-source-de-référence)
3. [Convention d'identification](#3-convention-didentification)
4. [Sous-domaines de IDENTITY](#4-sous-domaines-de-identity)
5. [Sous-domaines de CATALOG](#5-sous-domaines-de-catalog)
6. [Sous-domaines de DISCOVERY](#6-sous-domaines-de-discovery)
7. [Sous-domaines de BOOKING](#7-sous-domaines-de-booking)
8. [Sous-domaines de PAYMENT](#8-sous-domaines-de-payment)
9. [Sous-domaines de TICKETING](#9-sous-domaines-de-ticketing)
10. [Sous-domaines de ACCESS](#10-sous-domaines-de-access)
11. [Sous-domaines de FINANCE](#11-sous-domaines-de-finance)
12. [Sous-domaines de REFUND](#12-sous-domaines-de-refund)
13. [Sous-domaines de TRUST & SAFETY](#13-sous-domaines-de-trust--safety)
14. [Sous-domaines de OBSERVATION](#14-sous-domaines-de-observation)
15. [Sous-domaines de COMMUNICATION](#15-sous-domaines-de-communication)
16. [Résumé du découpage](#16-résumé-du-découpage)
17. [Principes de découpage](#17-principes-de-découpage)
18. [Statut](#18-statut)

---

# 1. Objectif

Ce document décompose chacun des douze domaines définis dans le document source en sous-domaines. Pour chaque sous-domaine, il précise :

- la responsabilité propre à l'échelle du sous-domaine ;
- la limite avec les sous-domaines voisins du même domaine ;
- le lien avec les sous-domaines des domaines amont et aval.

Il ne redéfinit pas les concepts, les états métier, les règles métier ni les exigences déjà établis dans la source. Ces éléments sont utilisés par référence et supposés connus du lecteur.

---

# 2. Source de référence

Ce document est dérivé exclusivement de :

- `05-domain-driven-design/domaines.md`

Toute définition, règle métier ou exigence mentionnée implicitement dans la suite renvoie à ce document source.

---

# 3. Convention d'identification

Chaque sous-domaine possède un identifiant dérivé de celui de son domaine parent :

| Identifiant | Sous-domaine | Domaine parent |
|---|---|---|
| `SD-01-1` | Gestion des comptes | IDENTITY |
| `SD-01-2` | Autorisation organisateur | IDENTITY |
| `SD-01-3` | Gestion des organisations | IDENTITY |
| `SD-01-4` | Sécurité interne | IDENTITY |
| `SD-02-1` | Cycle de vie de l'événement | CATALOG |
| `SD-02-2` | Configuration des espaces | CATALOG |
| `SD-02-3` | Catégories et disponibilités | CATALOG |
| `SD-02-4` | Historique de configuration | CATALOG |
| `SD-03-1` | Exposition du catalogue | DISCOVERY |
| `SD-03-2` | Recherche et filtrage | DISCOVERY |
| `SD-03-3` | Consultation détaillée | DISCOVERY |
| `SD-04-1` | Création de réservation | BOOKING |
| `SD-04-2` | Expiration automatique | BOOKING |
| `SD-04-3` | Annulation explicite | BOOKING |
| `SD-04-4` | Arbitrage des disponibilités | BOOKING |
| `SD-05-1` | Initiation du paiement | PAYMENT |
| `SD-05-2` | Suivi et confirmation | PAYMENT |
| `SD-05-3` | Traitement des échecs | PAYMENT |
| `SD-05-4` | Réconciliation des paiements tardifs | PAYMENT |
| `SD-06-1` | Finalisation de l'achat | TICKETING |
| `SD-06-2` | Émission des billets | TICKETING |
| `SD-06-3` | Propriété et transfert | TICKETING |
| `SD-06-4` | Consultation des billets | TICKETING |
| `SD-07-1` | Scan | ACCESS |
| `SD-07-2` | Validation | ACCESS |
| `SD-07-3` | Mode dégradé | ACCESS |
| `SD-07-4` | Réintégration après resynchronisation | ACCESS |
| `SD-08-1` | Clôture financière | FINANCE |
| `SD-08-2` | Tenue du solde | FINANCE |
| `SD-08-3` | Retraits | FINANCE |
| `SD-09-1` | Détermination des obligations | REFUND |
| `SD-09-2` | Calcul et création des remboursements | REFUND |
| `SD-09-3` | Exécution des remboursements | REFUND |
| `SD-10-1` | Réception des signalements | TRUST & SAFETY |
| `SD-10-2` | Analyse de risque | TRUST & SAFETY |
| `SD-10-3` | Décisions et mesures | TRUST & SAFETY |
| `SD-11-1` | Collecte des événements métier | OBSERVATION |
| `SD-11-2` | Statistiques | OBSERVATION |
| `SD-11-3` | Historique et traçabilité | OBSERVATION |
| `SD-12-1` | Mise à disposition du billet | COMMUNICATION |
| `SD-12-2` | Distribution par email | COMMUNICATION |
| `SD-12-3` | Notifications d'événement | COMMUNICATION |

---

# 4. Sous-domaines de IDENTITY

| Champ | Valeur |
|---|---|
| **Domaine parent** | `DOM-01` — IDENTITY |
| **Dépendances du domaine** | Aucune entrante ; fournit l'identité des acteurs à tous les domaines |

## 4.1. SD-01-1 — Gestion des comptes

| Champ | Valeur |
|---|---|
| **Responsabilité** | Créer et maintenir le compte utilisateur minimal, puis accueillir les informations complémentaires ajoutées après la création |
| **Ne couvre pas** | L'octroi de la capacité organisateur (SD-01-2) ; la vérification des organisations (SD-01-3) |

Sous-domaine d'entrée de tous les parcours. Il matérialise le compte participant et conserve le reste du profil au fil du temps, sans juger de la valeur de ces informations pour la sécurité.

## 4.2. SD-01-2 — Autorisation organisateur

| Champ | Valeur |
|---|---|
| **Responsabilité** | Gérer la demande, l'octroi et la révocation de la capacité organisateur |
| **Ne couvre pas** | La gestion du compte lui-même (SD-01-1) ; les mesures de sécurité (SD-01-4) |

Isole la décision d'autorisation : un même compte peut exercer les deux capacités, mais le passage de participant à organisateur passe nécessairement par ce sous-domaine.

## 4.3. SD-01-3 — Gestion des organisations

| Champ | Valeur |
|---|---|
| **Responsabilité** | Créer les organisations, les associer à leurs membres autorisés, et conduire leur cycle de vérification |
| **Ne couvre pas** | Les mesures de sécurité appliquées aux organisations (SD-01-4) |

Reçoit les demandes de vérification émanant d'organisateurs autorisés et restitue le résultat au domaine CATALOG, dont la publication d'événement en dépend.

## 4.4. SD-01-4 — Sécurité interne

| Champ | Valeur |
|---|---|
| **Responsabilité** | Regrouper les données et décisions de sécurité et de fraude, en cloisonnement vis-à-vis des utilisateurs |
| **Ne couvre pas** | L'analyse des risques (domaine TRUST & SAFETY) |

Centralise ce qui ne doit jamais être exposé aux utilisateurs. Les mesures de restriction (suspension, bannissement) sont reçues de TRUST & SAFETY et appliquées ici sur les comptes et organisations.

---

# 5. Sous-domaines de CATALOG

| Champ | Valeur |
|---|---|
| **Domaine parent** | `DOM-02` — CATALOG |
| **Dépendances du domaine** | Dépend de IDENTITY ; fournit les événements publiés à DISCOVERY, BOOKING, TICKETING, ACCESS |

## 5.1. SD-02-1 — Cycle de vie de l'événement

| Champ | Valeur |
|---|---|
| **Responsabilité** | Piloter les transitions d'état de l'événement, de sa création jusqu'à son archivage ou sa suppression conditionnée |
| **Ne couvre pas** | Le contenu de la configuration (SD-02-2, SD-02-3) ; l'historique des modifications (SD-02-4) |

Porte exclusivement les transitions : soumission, vérification, publication, arrêt automatique des ventes à l'heure de début, fermeture, archivage. Les conditions de chaque transition sont celles du domaine parent ; ce sous-domaine les orchestre sans les redéfinir.

## 5.2. SD-02-2 — Configuration des espaces

| Champ | Valeur |
|---|---|
| **Responsabilité** | Décrire les lieux et les zones d'accueil avec leurs capacités propres |
| **Ne couvre pas** | L'association des catégories de billets aux disponibilités (SD-02-3) |

Fournit la structure d'accueil sur laquelle reposent les capacités. Les refus de réduction incompatible avec des billets déjà attribués émanent de la confrontation entre cette structure et l'état des ventes.

## 5.3. SD-02-3 — Catégories et disponibilités

| Champ | Valeur |
|---|---|
| **Responsabilité** | Définir les catégories de billets et l'état courant de la disponibilité associée |
| **Ne couvre pas** | La structure spatiale (SD-02-2) ; le blocage temporaire opéré par BOOKING (domaine aval) |

Tient le compte des disponibilités encore attribuables, que ce sous-domaine restitue à la consultation (DISCOVERY) et à la réservation (BOOKING).

## 5.4. SD-02-4 — Historique de configuration

| Champ | Valeur |
|---|---|
| **Responsabilité** | Conserver la trace des opérations de configuration sans jamais les réécrire |
| **Ne couvre pas** | L'historique global des opérations métier (domaine OBSERVATION) |

Limite son périmètre aux modifications de configuration de l'événement. Les opérations irréversibles qu'il conserve conditionnent l'impossibilité de suppression définitive.

---

# 6. Sous-domaines de DISCOVERY

| Champ | Valeur |
|---|---|
| **Domaine parent** | `DOM-03` — DISCOVERY |
| **Dépendances du domaine** | Dépend de CATALOG ; fournit le parcours de découverte au participant |

## 6.1. SD-03-1 — Exposition du catalogue

| Champ | Valeur |
|---|---|
| **Responsabilité** | Restituer en lecture seule l'ensemble des événements publiés et accessibles |
| **Ne couvre pas** | La formulation des requêtes (SD-03-2) ; le détail d'un événement (SD-03-3) |

Point d'entrée sans compte requis. Filtre à la source tout ce qui n'est ni publié ni accessible.

## 6.2. SD-03-2 — Recherche et filtrage

| Champ | Valeur |
|---|---|
| **Responsabilité** | Transformer les critères du participant en sélection sur le catalogue exposé |
| **Ne couvre pas** | L'exposition elle-même (SD-03-1) |

Applique les filtres au résultat de SD-03-1 sans modifier le catalogue.

## 6.3. SD-03-3 — Consultation détaillée

| Champ | Valeur |
|---|---|
| **Responsabilité** | Présenter le détail d'un événement et la disponibilité visible calculée |
| **Ne couvre pas** | La disponibilité effective gérée par CATALOG (SD-02-3) et BOOKING |

Calcule la disponibilité affichée en tenant compte des billets vendus et des réservations en cours, sans détenir l'autorité sur ces données.

---

# 7. Sous-domaines de BOOKING

| Champ | Valeur |
|---|---|
| **Domaine parent** | `DOM-04` — BOOKING |
| **Dépendances du domaine** | Dépend de CATALOG ; fournit la réservation à PAYMENT |

## 7.1. SD-04-1 — Création de réservation

| Champ | Valeur |
|---|---|
| **Responsabilité** | Enregistrer la demande de réservation et opérer le blocage temporaire de la disponibilité |
| **Ne couvre pas** | La garantie d'unicité (SD-04-4) ; l'expiration (SD-04-2) |

Reçoit la demande du participant et sollicite SD-04-4 pour l'attribution effective.

## 7.2. SD-04-2 — Expiration automatique

| Champ | Valeur |
|---|---|
| **Responsabilité** | Libérer les disponibilités bloquées au terme du délai imparti |
| **Ne couvre pas** | La libération sur demande (SD-04-3) ; la signification de l'expiration pour le paiement (domaine aval) |

Fait tourner l'horloge du blocage et rend la disponibilité sans constituer un échec de paiement.

## 7.3. SD-04-3 — Annulation explicite

| Champ | Valeur |
|---|---|
| **Responsabilité** | Libérer une réservation sur initiative du participant avant son expiration |
| **Ne couvre pas** | L'expiration automatique (SD-04-2) |

## 7.4. SD-04-4 — Arbitrage des disponibilités

| Champ | Valeur |
|---|---|
| **Responsabilité** | Garantir qu'une même disponibilité n'est jamais attribuée simultanément à plusieurs achats valides |
| **Ne couvre pas** | La création de la demande (SD-04-1) |

Point de concurrence du domaine : toute attribution, quelle que soit son origine, transite par cet arbitre unique.

---

# 8. Sous-domaines de PAYMENT

| Champ | Valeur |
|---|---|
| **Domaine parent** | `DOM-05` — PAYMENT |
| **Dépendances du domaine** | Dépend de BOOKING ; fournit le résultat à TICKETING et REFUND |

## 8.1. SD-05-1 — Initiation du paiement

| Champ | Valeur |
|---|---|
| **Responsabilité** | Démarrer l'opération de paiement Mobile Money à partir d'une réservation valide |
| **Ne couvre pas** | Le suivi de l'opération engagée (SD-05-2) |

Unique point de contact avec le moyen de paiement du MVP pour le démarrage d'une opération.

## 8.2. SD-05-2 — Suivi et confirmation

| Champ | Valeur |
|---|---|
| **Responsabilité** | Recevoir les accusés du service de paiement et en garantir l'effet unique |
| **Ne couvre pas** | Le traitement des échecs (SD-05-3) ; les confirmations tardives (SD-05-4) |

Dédoublonne les confirmations reçues plusieurs fois : une seule confirmation ne produit qu'un seul effet métier en aval.

## 8.3. SD-05-3 — Traitement des échecs

| Champ | Valeur |
|---|---|
| **Responsabilité** | Enregistrer les opérations n'ayant pas abouti et conduire leur traitement |
| **Ne couvre pas** | La réconciliation des paiements aboutis tardivement (SD-05-4) |

## 8.4. SD-05-4 — Réconciliation des paiements tardifs

| Champ | Valeur |
|---|---|
| **Responsabilité** | Traiter les paiements confirmés après expiration de la réservation et déterminer l'issue |
| **Ne couvre pas** | La confirmation en temps utile (SD-05-2) |

Trois issues possibles : la disponibilité est encore attribuable, elle est déjà attribuée, ou un remboursement doit être déclenché vers REFUND. Un paiement tardif n'est jamais ignoré du seul fait de l'expiration.

---

# 13. Sous-domaines de TRUST & SAFETY

| Champ | Valeur |
|---|---|
| **Domaine parent** | `DOM-10` — TRUST & SAFETY |
| **Dépendances du domaine** | Dépend de IDENTITY et CATALOG ; fournit les décisions de sécurité à CATALOG et IDENTITY |

## 13.1. SD-10-1 — Réception des signalements

| Champ | Valeur |
|---|---|
| **Responsabilité** | Enregistrer les alertes des participants avec leur motif prédéfini obligatoire et leur description facultative |
| **Ne couvre pas** | L'évaluation du signalement (SD-10-2) |

Un signalement reçu n'est pas une preuve : il constitue uniquement l'entrée du circuit d'analyse.

## 13.2. SD-10-2 — Analyse de risque

| Champ | Valeur |
|---|---|
| **Responsabilité** | Évaluer le niveau de menace identifié à partir des signalements et des éléments disponibles |
| **Ne couvre pas** | La réception (SD-10-1) ; la décision (SD-10-3) |

## 13.3. SD-10-3 — Décisions et mesures

| Champ | Valeur |
|---|---|
| **Responsabilité** | Choisir et appliquer la mesure adaptée au niveau de risque, en traçant chaque décision |
| **Ne couvre pas** | L'analyse (SD-10-2) |

Le panel de mesures est : maintien, suspension, annulation ou nouvelle vérification, jusqu'au bannissement définitif. Le bannissement d'une organisation n'entraîne pas automatiquement la suppression de ses événements existants : la mesure porte sur l'acteur, non rétroactivement sur son catalogue.

---

# 14. Sous-domaines de OBSERVATION

| Champ | Valeur |
|---|---|
| **Domaine parent** | `DOM-11` — OBSERVATION |
| **Dépendances du domaine** | Dépend de tous les domaines ; fournit statistiques et historique aux organisateurs et à Eventix |

## 14.1. SD-11-1 — Collecte des événements métier

| Champ | Valeur |
|---|---|
| **Responsabilité** | Recueillir les faits marquants émis par chaque domaine, y compris ceux remontés par ACCESS en mode dégradé |
| **Ne couvre pas** | L'exploitation des données collectées (SD-11-2, SD-11-3) |

## 14.2. SD-11-2 — Statistiques

| Champ | Valeur |
|---|---|
| **Responsabilité** | Produire les agrégats d'activité pour le pilotage |
| **Ne couvre pas** | Le journal des opérations (SD-11-3) |

Lecture seule par construction : aucune statistique ne modifie une donnée métier. Elle distingue soigneusement les ventes, les entrées et les participants présents des seuls billets vendus.

## 14.3. SD-11-3 — Historique et traçabilité

| Champ | Valeur |
|---|---|
| **Responsabilité** | Maintenir le journal chronologique des opérations importantes avec leur contexte |
| **Ne couvre pas** | Les agrégats (SD-11-2) |

Le journal n'est jamais réécrit par les modifications ultérieures.

---

# 15. Sous-domaines de COMMUNICATION

| Champ | Valeur |
|---|---|
| **Domaine parent** | `DOM-12` — COMMUNICATION |
| **Dépendances du domaine** | Dépend de TICKETING et CATALOG ; fournit les communications aux participants |

## 15.1. SD-12-1 — Mise à disposition du billet

| Champ | Valeur |
|---|---|
| **Responsabilité** | Rendre le billet accessible depuis le compte participant dès son émission |
| **Ne couvre pas** | L'émission (domaine TICKETING) ; l'envoi par email (SD-12-2) |

## 15.2. SD-12-2 — Distribution par email

| Champ | Valeur |
|---|---|
| **Responsabilité** | Distribuer le billet par email lorsque ce canal est utilisé |
| **Ne couvre pas** | La mise à disposition dans le compte (SD-12-1) ; les notifications de changement (SD-12-3) |

## 15.3. SD-12-3 — Notifications d'événement

| Champ | Valeur |
|---|---|
| **Responsabilité** | Informer les participants concernés d'une annulation ou d'un report |
| **Ne couvre pas** | La distribution des billets (SD-12-1, SD-12-2) |

Les modalités détaillées de communication restent encadrées par les capacités du MVP.

---

# 16. Résumé du découpage

| Sous-domaine | Responsabilité condensée |
|---|---|
| SD-01-1 | Compte utilisateur minimal puis compléments |
| SD-01-2 | Demande, octroi et révocation de la capacité organisateur |
| SD-01-3 | Organisations, membres et vérification |
| SD-01-4 | Cloisonnement des données et décisions de sécurité |
| SD-02-1 | Transitions d'état de l'événement |
| SD-02-2 | Lieux, zones et capacités d'accueil |
| SD-02-3 | Catégories de billets et disponibilité courante |
| SD-02-4 | Trace non réécrite des opérations de configuration |
| SD-03-1 | Exposition en lecture seule du catalogue publié |
| SD-03-2 | Requêtes et filtres du participant |
| SD-03-3 | Détail événement et disponibilité visible |
| SD-04-1 | Enregistrement de la demande et blocage temporaire |
| SD-04-2 | Libération automatique au terme du délai |
| SD-04-3 | Libération sur demande du participant |
| SD-04-4 | Arbitre unique des attributions concurrentes |
| SD-05-1 | Démarrage de l'opération Mobile Money |
| SD-05-2 | Accusés et effet unique de la confirmation |
| SD-05-3 | Enregistrement et traitement des échecs |
| SD-05-4 | Paiements tardifs et choix de l'issue |
| SD-06-1 | Création de l'achat, y compris gratuit |
| SD-06-2 | Émission du billet et reprise après échec |
| SD-06-3 | Propriétaire actif unique et transferts tracés |
| SD-06-4 | Restitution des billets au compte |
| SD-07-1 | Lecture du QR Code |
| SD-07-2 | Décision d'acceptation et usage unique |
| SD-07-3 | Exploitation réduite à un scanner |
| SD-07-4 | Rejeu des opérations après resynchronisation |
| SD-08-1 | Montant net de la clôture |
| SD-08-2 | Solde retirable |
| SD-08-3 | Versements et restitutions d'échecs |
| SD-09-1 | Qualification des obligations de rembourser |
| SD-09-2 | Calcul et création unique |
| SD-09-3 | Versement, reprise et traitement progressif |
| SD-10-1 | Enregistrement des signalements |
| SD-10-2 | Évaluation du niveau de risque |
| SD-10-3 | Choix, application et traçabilité des mesures |
| SD-11-1 | Collecte des événements métier |
| SD-11-2 | Agrégats de pilotage en lecture seule |
| SD-11-3 | Journal chronologique non réécrit |
| SD-12-1 | Billet accessible depuis le compte |
| SD-12-2 | Envoi du billet par email |
| SD-12-3 | Information en cas d'annulation ou de report |

---

# 17. Principes de découpage

Les propriétés suivantes régissent le découpage :

- chaque sous-domaine possède une responsabilité unique et une frontière explicite avec ses voisins ;
- une même règle métier du domaine parent n'est jamais répartie entre deux sous-domaines : elle est portée entièrement par celui auquel elle appartient naturellement, les autres la consomment par référence ;
- les dépendances entre sous-domaines de domaines différents empruntent les frontières de domaines établies dans la source — aucune dépendance transversale nouvelle n'est introduite ;
- aucune décision technique n'est prise ou implicite.

---

# 18. Statut

| Champ | Valeur |
|---|---|
| **Document** | `sous-domaines.md` |
| **Version** | 1.0 |
| **Statut** | À valider par l'équipe |
| **Périmètre** | MVP Eventix |
| **Marché** | Cameroun |

| Principe | État |
|---|---|
| Responsabilité unique par sous-domaine | ✅ APPLIQUÉ |
| Frontières explicites avec les voisins | ✅ DÉFINIES |
| Cohérence avec les frontières de domaines | ✅ VÉRIFIÉE |
| Règles métier redéfinies | ❌ AUCUNE — référence à la source |
| Décisions techniques | ⏳ NON PRÉJUGÉES |

---

## Notes de rédaction

### Cohérence avec la source

Ce document ne répète pas les définitions, états métier, invariants ni exigences déjà établis dans `domaines.md`. Lorsqu'une règle du domaine parent conditionne un sous-domaine, elle est invoquée sans être restituée. La comparaison entre les deux documents ne doit jamais faire apparaître deux formulations concurrentes d'une même règle.

### Granularité

La granularité retenue est celle du sous-domaine : unité de responsabilité interne à un domaine, sans existence propre vis-à-vis des autres domaines. Un sous-domaine détaillé supplémentaire pourra faire l'objet d'un document ultérieur si la phase 05 l'exige.

### Questions ouvertes

Les questions métier encore ouvertes identifiées dans les phases précédentes ne sont pas tranchées ici et restent référencées dans `questions-metier-ouvertes.md`.