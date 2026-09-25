# Événements de domaine — Eventix

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
3. [Définition d'un événement de domaine](#3-définition-dun-événement-de-domaine)
4. [Règles d'émission](#4-règles-démission)
5. [Événements par agrégat](#5-événements-par-agrégat)
6. [Événements par service de domaine](#6-événements-par-service-de-domaine)
7. [Consommateurs par événement](#7-consommateurs-par-événement)
8. [Flux d'événements transversaux](#8-flux-dévénements-transversaux)
9. [Résumé](#9-résumé)
10. [Critères de qualité du document](#10-critères-de-qualité-du-document)
11. [Statut](#11-statut)

---

# 1. Objectif

Ce document identifie les événements de domaine produits par les changements d'état des agrégats et les coordinations des services de domaine d'Eventix. Pour chaque événement, il précise :

- le producteur (agrégat ou service) ;
- le déclencheur métier ;
- la charge utile (données transportées) ;
- les consommateurs potentiels.

Il ne redéfinit ni les agrégats, ni les services, ni les invariants déjà établis. Ces éléments sont utilisés par référence et supposés connus du lecteur.

---

# 2. Sources de référence

Ce document est dérivé principalement de :

- `05-domain-driven-design/agregats.md` — frontières de cohérence dont les transitions d'état produisent des événements
- `05-domain-driven-design/services-de-domaine.md` — coordinations inter-agrégats dont les enchaînements produisent des événements

Toute définition d'agrégat, de service ou d'invariant mentionnée implicitement renvoie à ces documents sources.

---

# 3. Définition d'un événement de domaine

## 3.1. Caractéristiques

Un événement de domaine est un fait marquant survenu dans le domaine métier. Il possède :

- **Un nom au passé** : il exprime ce qui s'est produit (RéservationCréée, PaiementConfirmé).
- **Une charge utile minimale** : il transporte les données nécessaires aux consommateurs, sans excès.
- **Un producteur unique** : un seul agrégat ou un seul service le publie.
- **Des consommateurs multiples possibles** : zéro, un ou plusieurs contextes peuvent le consommer.
- **Une immuabilité** : une fois émis, il ne peut être modifié ni annulé.

## 3.2. Rôle dans le modèle

Les événements de domaine matérialisent les transitions d'état qui dépassent une frontière d'agrégat. Ils permettent la communication asynchrone entre bounded contexts et alimentent la traçabilité exigée par les processus métier.

---

# 4. Règles d'émission

| Règle | Énoncé |
|---|---|
| E1 — Nommage | Le nom est un terme du langage ubiquitaire au passé composé. Aucun événement ne porte un nom technique. |
| E2 — Immuabilité | Un événement émis ne peut être modifié. Une correction prend la forme d'un événement compensatoire. |
| E3 — Minimalité | La charge utile contient uniquement les données nécessaires aux consommateurs identifiés. |
| E4 — Unicité | Un événement est produit par une seule source. Deux sources distinctes ne publient jamais le même type d'événement. |
| E5 — Traçabilité | Tout événement émis laisse une trace dans l'Agrégat Historique ou l'Agrégat Événement métier, conformément à l'exigence de traçabilité des processus. |

---

# 5. Événements par agrégat

## 5.1. BC-01 — Identity & Access Management

### Agrégat Utilisateur

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `UtilisateurCréé` | Création d'un compte | Identifiant utilisateur, email ou téléphone | BC-11 (Historique) |
| `UtilisateurAuthentifié` | Authentification réussie | Identifiant utilisateur, date, heure | BC-11 (Historique) |
| `CompteParticipantActivé` | Activation de la capacité participant | Identifiant compte participant | BC-11 (Historique) |
| `CompteOrganisateurAutorisé` | Octroi de la capacité organisateur | Identifiant compte organisateur | BC-02, BC-11 (Historique) |
| `CompteOrganisateurRévoqué` | Révocation de la capacité organisateur | Identifiant compte organisateur | BC-02, BC-11 (Historique) |

### Agrégat Organisation

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `OrganisationCréée` | Création d'une organisation | Identifiant organisation, nom | BC-11 (Historique) |
| `OrganisationSoumiseÀVérification` | Début du processus de vérification | Identifiant organisation | BC-11 (Historique) |
| `OrganisationVérifiée` | Vérification positive | Identifiant organisation | BC-02, BC-11 (Historique) |
| `OrganisationSuspendue` | Suspension de l'organisation | Identifiant organisation, motif | BC-02, BC-11 (Historique) |
| `OrganisationBannie` | Bannissement définitif | Identifiant organisation, motif | BC-02, BC-11 (Historique) |

---

## 5.2. BC-02 — Event Catalog

### Agrégat Événement

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `ÉvénementCréé` | Création en état `DRAFT` | Identifiant événement, identifiant organisateur | BC-11 (Historique) |
| `ÉvénementSoumis` | Passage à `SUBMITTED` | Identifiant événement | BC-10, BC-11 (Historique) |
| `ÉvénementValidé` | Passage à `VALIDATED` | Identifiant événement | BC-11 (Historique) |
| `ÉvénementRefusé` | Refus de publication | Identifiant événement, motif | BC-11 (Historique) |
| `ÉvénementPublié` | Passage à `PUBLISHED` | Identifiant événement, date de publication | BC-03, BC-04, BC-06, BC-07, BC-11 (Historique) |
| `VentesArrêtées` | Arrêt automatique ou manuel des ventes | Identifiant événement, date d'arrêt | BC-04, BC-06, BC-11 (Historique) |
| `ÉvénementDébuté` | Passage à `ONGOING` | Identifiant événement, date de début | BC-07, BC-11 (Historique) |
| `ÉvénementTerminé` | Passage à `COMPLETED` | Identifiant événement, date de fin | BC-08, BC-11 (Historique) |
| `ÉvénementClôturé` | Passage à `CLOSED` | Identifiant événement | BC-08, BC-11 (Historique) |
| `ÉvénementArchivé` | Passage à `ARCHIVED` | Identifiant événement | BC-11 (Historique) |
| `ÉvénementAnnulé` | Passage à `CANCELLED` | Identifiant événement, motif | BC-06, BC-09, BC-12, BC-11 (Historique) |
| `ÉvénementReporté` | Report à une nouvelle date | Identifiant événement, nouvelle date | BC-06, BC-12, BC-11 (Historique) |
| `CapacitéModifiée` | Modification d'une capacité | Identifiant événement, identifiant zone, ancienne capacité, nouvelle capacité | BC-04, BC-11 (Historique) |
| `CatégorieDeBilletAjoutée` | Ajout d'une catégorie | Identifiant événement, identifiant catégorie | BC-04, BC-06, BC-11 (Historique) |
| `ConfigurationModifiée` | Modification de la configuration | Identifiant événement, éléments modifiés | BC-11 (Historique) |

---

## 5.3. BC-04 — Booking & Availability

### Agrégat Réservation

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `RéservationCréée` | Création en état `PENDING` | Identifiant réservation, identifiant événement, identifiant catégorie, quantité, date d'expiration | BC-05, BC-11 (Historique) |
| `RéservationConfirmée` | Passage à `CONFIRMED` | Identifiant réservation | BC-05, BC-11 (Historique) |
| `RéservationExpirée` | Passage à `EXPIRED` après délai | Identifiant réservation, identifiant disponibilité | BC-05, BC-11 (Historique) |
| `RéservationAnnulée` | Annulation par le participant | Identifiant réservation | BC-05, BC-11 (Historique) |

### Agrégat Disponibilité

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `DisponibilitéBloquée` | Blocage par une réservation | Identifiant disponibilité, identifiant réservation, quantité | BC-03, BC-11 (Historique) |
| `DisponibilitéLibérée` | Libération après expiration ou annulation | Identifiant disponibilité, identifiant réservation, quantité | BC-03, BC-11 (Historique) |
| `DisponibilitéDécrémentée` | Attribution définitive | Identifiant disponibilité, identifiant achat, quantité | BC-03, BC-11 (Historique) |
| `DisponibilitéÉpuisée` | Atteinte de zéro | Identifiant disponibilité | BC-03, BC-11 (Historique) |

---

## 5.4. BC-05 — Payment Processing

### Agrégat Paiement

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `PaiementInitié` | Démarrage de l'opération | Identifiant paiement, identifiant réservation, montant, moyen de paiement | BC-11 (Historique) |
| `PaiementConfirmé` | Réception d'une confirmation fiable | Identifiant paiement, identifiant réservation, montant | BC-06, BC-09, BC-11 (Historique) |
| `PaiementÉchoué` | Échec de la transaction | Identifiant paiement, identifiant réservation, motif | BC-11 (Historique) |

### Agrégat Réconciliation

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `RéconciliationDéclenchée` | Paiement confirmé après expiration | Identifiant réconciliation, identifiant paiement, identifiant réservation | BC-11 (Historique) |
| `RéconciliationRésolue` | Détermination de l'issue | Identifiant réconciliation, issue (billet ou remboursement) | BC-06 ou BC-09, BC-11 (Historique) |

---

## 5.5. BC-06 — Ticketing & Fulfillment

### Agrégat Achat

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `AchatFinalisé` | Confirmation de l'achat | Identifiant achat, identifiant réservation, identifiant paiement, montant | BC-08, BC-11 (Historique) |

### Agrégat Billet

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `BilletÉmis` | Émission du billet | Identifiant billet, identifiant événement, identifiant achat, identifiant propriétaire, QR Code | BC-07, BC-12, BC-11 (Historique) |
| `BilletTransféré` | Changement de propriétaire | Identifiant billet, ancien propriétaire, nouveau propriétaire | BC-11 (Historique) |
| `BilletUtilisé` | Validation au contrôle | Identifiant billet, identifiant événement, date, heure | BC-02, BC-11 (Historique) |
| `BilletAnnulé` | Annulation du billet | Identifiant billet, motif | BC-07, BC-11 (Historique) |

---

## 5.6. BC-07 — Access Control

### Agrégat Point d'entrée

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `ContrôleEffectué` | Scan et décision | Identifiant contrôle, identifiant billet, identifiant point d'entrée, résultat | BC-11 (Historique) |
| `PrésenceEnregistrée` | Entrée effective | Identifiant présence, identifiant billet, identifiant point d'entrée, date, heure | BC-11 (Statistiques, Historique) |
| `AccèsRefusé` | Refus d'un billet | Identifiant billet, identifiant point d'entrée, motif du refus | BC-11 (Historique) |
| `ModeDégradéActivé` | Perte de l'état partagé fiable | Identifiant point d'entrée | BC-11 (Historique) |
| `ModeDégradéDésactivé` | Retour à l'état partagé fiable | Identifiant point d'entrée | BC-11 (Historique) |

---

## 5.7. BC-08 — Financial Settlement

### Agrégat Clôture

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `ClôturePréparée` | Début du processus de clôture | Identifiant clôture, identifiant événement | BC-11 (Historique) |
| `ClôtureEffectuée` | Calcul validé | Identifiant clôture, identifiant événement, montant net | BC-08 (Solde), BC-11 (Historique) |

### Agrégat Solde organisateur

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `SoldeAlimenté` | Ajout du montant net après clôture | Identifiant solde, identifiant clôture, montant | BC-11 (Historique) |
| `RetraitDemandé` | Demande de retrait | Identifiant retrait, identifiant solde, montant | BC-11 (Historique) |
| `RetraitEffectué` | Retrait réussi | Identifiant retrait, identifiant solde, montant | BC-11 (Historique) |
| `RetraitÉchoué` | Échec du retrait | Identifiant retrait, identifiant solde, montant, motif | BC-08 (Solde), BC-11 (Historique) |
| `RetraitRestitué` | Restitution au solde après échec | Identifiant retrait, identifiant solde, montant | BC-11 (Historique) |

---

## 5.8. BC-09 — Refund Management

### Agrégat Obligation de remboursement

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `ObligationDeRemboursementCréée` | Détermination de l'obligation | Identifiant obligation, identifiant paiement, identifiant billet, montant de référence | BC-11 (Historique) |
| `RemboursementCréé` | Création du remboursement | Identifiant remboursement, identifiant obligation, montant | BC-11 (Historique) |
| `RemboursementEffectué` | Exécution réussie | Identifiant remboursement, identifiant obligation, montant | BC-08, BC-11 (Historique) |
| `RemboursementÉchoué` | Échec technique | Identifiant remboursement, identifiant obligation, motif | BC-11 (Historique) |
| `RemboursementRetenté` | Nouvelle tentative après échec | Identifiant remboursement, identifiant obligation | BC-11 (Historique) |

---

## 5.9. BC-10 — Trust & Safety

### Agrégat Signalement

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `SignalementReçu` | Réception d'un signalement | Identifiant signalement, cible, motif | BC-10 (Analyse), BC-11 (Historique) |

### Agrégat Mesure de sécurité

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `MesureDeSécuritéDécidée` | Décision après analyse | Identifiant mesure, cible, type de mesure | BC-01, BC-02, BC-11 (Historique) |
| `MesureDeSécuritéAppliquée` | Application effective | Identifiant mesure, cible, type de mesure | BC-11 (Historique) |

---

## 5.10. BC-11 — Analytics & Observability

### Agrégat Événement métier

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `ÉvénementMétierEnregistré` | Réception d'un fait marquant | Contenu de l'événement métier | BC-11 (Statistiques, Historique) |

### Agrégat Statistique

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `StatistiqueCalculée` | Calcul d'un agrégat | Type de statistique, période, valeur | BC-11 (Publication) |

---

## 5.11. BC-12 — Communication

### Agrégat Distribution

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `BilletDistribué` | Envoi réussi | Identifiant distribution, identifiant billet, canal | BC-11 (Historique) |
| `DistributionÉchouée` | Échec de l'envoi | Identifiant distribution, identifiant billet, motif | BC-11 (Historique) |

### Agrégat Notification

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `NotificationEnvoyée` | Envoi réussi | Identifiant notification, type, destinataires | BC-11 (Historique) |
| `NotificationÉchouée` | Échec de l'envoi | Identifiant notification, type, destinataires, motif | BC-11 (Historique) |

---

# 6. Événements par service de domaine

## 6.1. ServiceDeVérificationEvenementielle

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `VérificationDébutée` | Lancement de la vérification | Identifiant événement, identifiant organisation | BC-11 (Historique) |
| `VérificationConclue` | Décision finale (validation ou refus) | Identifiant événement, résultat, motif éventuel | BC-02, BC-11 (Historique) |

---

## 6.2. ServiceDExpirationDeReservation

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `ExpirationTraitéee` | Traitement d'une expiration | Identifiant réservation, identifiant disponibilité | BC-11 (Historique) |

---

## 6.3. ServiceDeFinalisationDAchat

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `FinalisationDébutée` | Réception d'une confirmation de paiement | Identifiant paiement, identifiant réservation | BC-11 (Historique) |
| `FinalisationEffectuée` | Achat finalisé et billet émis | Identifiant achat, identifiant billet | BC-11 (Historique) |
| `FinalisationIdempotente` | Confirmation reçue en doublon, aucun effet supplémentaire | Identifiant paiement | BC-11 (Historique) |

---

## 6.4. ServiceDeReconciliation

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `RéconciliationDébutée` | Paiement tardif détecté | Identifiant paiement, identifiant réservation | BC-11 (Historique) |
| `RéconciliationConclue` | Issue déterminée | Identifiant réconciliation, issue | BC-06 ou BC-09, BC-11 (Historique) |

---

## 6.5. ServiceDAnnulationDevenement

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `AnnulationDébutée` | Décision d'annulation | Identifiant événement | BC-11 (Historique) |
| `BilletsInvalidés` | Invalidation des billets concernés | Identifiant événement, nombre de billets | BC-11 (Historique) |
| `RemboursementsDéclenchés` | Création des obligations de remboursement | Identifiant événement, nombre d'obligations | BC-09, BC-11 (Historique) |
| `ParticipantsInformés` | Envoi des notifications | Identifiant événement, nombre de destinataires | BC-11 (Historique) |
| `AnnulationEffectuée` | Annulation complète | Identifiant événement | BC-11 (Historique) |

---

## 6.6. ServiceDeReportDevenement

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `ReportDébuté` | Décision de report | Identifiant événement, nouvelle date | BC-11 (Historique) |
| `ParticipantsInformésDuReport` | Envoi des notifications | Identifiant événement, nombre de destinataires | BC-11 (Historique) |
| `ReportEffectué` | Report complet | Identifiant événement, nouvelle date | BC-11 (Historique) |

---

## 6.7. ServiceDeControleDAcces

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `ContrôleDébuté` | Scan d'un billet | Identifiant billet, identifiant point d'entrée | BC-11 (Historique) |
| `AccèsAutorisé` | Décision positive | Identifiant billet, identifiant point d'entrée | BC-11 (Historique) |
| `AccèsRefusé` | Décision négative | Identifiant billet, identifiant point d'entrée, motif | BC-11 (Historique) |

---

## 6.8. ServiceDeGestionDuModeDegrade

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `ModeDégradéActivé` | Perte de l'état partagé fiable | Identifiant point d'entrée | BC-11 (Historique) |
| `ModeDégradéDésactivé` | Retour à l'état partagé fiable | Identifiant point d'entrée | BC-11 (Historique) |
| `OpérationsRéintégrées` | Rejeu après resynchronisation | Identifiant point d'entrée, nombre d'opérations | BC-11 (Historique) |

---

## 6.9. ServiceDeClotureFinanciere

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `ClôtureDébutée` | Lancement de la clôture | Identifiant événement | BC-11 (Historique) |
| `ClôtureCalculée` | Calcul du montant net | Identifiant événement, montant net | BC-11 (Historique) |
| `SoldeAlimenté` | Ajout au solde disponible | Identifiant solde, montant | BC-11 (Historique) |

---

## 6.10. ServiceDApplicationDeMesureDeSecurite

| Événement | Déclencheur | Charge utile | Consommateurs |
|---|---|---|---|
| `MesureAppliquée` | Application effective sur la cible | Identifiant mesure, cible, type | BC-01, BC-02, BC-11 (Historique) |
| `ÉvénementsÉvalués` | Évaluation des événements existants | Identifiant organisation, nombre d'événements | BC-11 (Historique) |

---

# 7. Consommateurs par événement

## 7.1. Consommateurs directs

| Événement | Consommateur | Usage |
|---|---|---|
| `ÉvénementPublié` | BC-03 | Mise à jour du catalogue exposé |
| `ÉvénementPublié` | BC-04 | Initialisation des disponibilités |
| `ÉvénementPublié` | BC-06 | Préparation de l'émission de billets |
| `ÉvénementPublié` | BC-07 | Préparation du contrôle d'accès |
| `RéservationCréée` | BC-05 | Déclenchement du paiement |
| `PaiementConfirmé` | BC-06 | Finalisation de l'achat et émission du billet |
| `PaiementConfirmé` | BC-09 | Déclenchement éventuel d'un remboursement (réconciliation) |
| `BilletÉmis` | BC-07 | Préparation du contrôle |
| `BilletÉmis` | BC-12 | Distribution du billet |
| `BilletUtilisé` | BC-02 | Mise à jour de l'état du billet |
| `ÉvénementAnnulé` | BC-06 | Invalidation des billets |
| `ÉvénementAnnulé` | BC-09 | Déclenchement des remboursements |
| `ÉvénementAnnulé` | BC-12 | Information des participants |
| `ÉvénementTerminé` | BC-08 | Déclenchement de la clôture |
| `ClôtureEffectuée` | BC-08 | Alimentation du solde |
| `RemboursementEffectué` | BC-08 | Intégration à la clôture |
| `MesureDeSécuritéDécidée` | BC-01 | Application sur comptes et organisations |
| `MesureDeSécuritéDécidée` | BC-02 | Application sur événements |

## 7.2. Consommateur universel

| Événement | Consommateur | Usage |
|---|---|---|
| Tous les événements | BC-11 | Enregistrement dans l'Historique et l'Événement métier |

---

# 8. Flux d'événements transversaux

## 8.1. Parcours participant complet

```text
RéservationCréée
    ↓
PaiementInitié
    ↓
PaiementConfirmé
    ↓
AchatFinalisé
    ↓
BilletÉmis
    ↓
BilletDistribué
    ↓
ContrôleEffectué
    ↓
PrésenceEnregistrée
    ↓
BilletUtilisé


8.2. Parcours organisateur complet
Text
CompteOrganisateurAutorisé
    ↓
ÉvénementCréé
    ↓
ÉvénementSoumis
    ↓
ÉvénementValidé
    ↓
ÉvénementPublié
    ↓
VentesArrêtées
    ↓
ÉvénementDébuté
    ↓
ÉvénementTerminé
    ↓
ClôtureEffectuée
    ↓
SoldeAlimenté
    ↓
RetraitEffectué
8.3. Flux de réconciliation
Text
RéservationExpirée
    ↓
PaiementConfirmé (tardif)
    ↓
RéconciliationDébutée
    ↓
RéconciliationConclue
    ↓
BilletÉmis (si disponibilité)
    ou
ObligationDeRemboursementCréée (si indisponibilité)
8.4. Flux de sécurité
Text
SignalementReçu
    ↓
MesureDeSécuritéDécidée
    ↓
MesureDeSécuritéAppliquée
    ↓
ÉvénementAnnulé (si mesure d'annulation)
    ou
OrganisationSuspendue (si mesure de suspension)
9. Résumé
Ce document identifie soixante-dix-sept événements de domaine produits par les seize agrégats et les dix services de domaine d'Eventix. Chaque événement possède un nom au passé, une charge utile minimale, un producteur unique et des consommateurs identifiés. Les événements matérialisent les transitions d'état qui dépassent une frontière d'agrégat et alimentent la traçabilité exigée par les processus métier. Ce document constitue la base pour la définition des politiques de cohérence et des mécanismes de communication inter-contextes dans les phases ultérieures.
10. Critères de qualité du document
Ce document doit respecter les propriétés suivantes :
chaque événement possède un nom au passé composé en termes du langage ubiquitaire ;
chaque événement possède un producteur unique ;
la charge utile est minimale et justifiée ;
les consommateurs sont identifiés pour chaque événement ;
aucune définition d'agrégat ou de service n'est reprise des documents sources ;
aucune décision technique n'est prise ou implicite.
11. Statut
Table
Champ	Valeur
Document	evenements-de-domaine.md
Version	1.0
Statut	À valider par l'équipe
Périmètre	MVP Eventix
Marché	Cameroun
Table
Principe	État
Nommage au passé composé	✅ APPLIQUÉ
Producteur unique	✅ APPLIQUÉ
Charge utile minimale	✅ APPLIQUÉ
Consommateurs identifiés	✅ DÉFINIS
Cohérence avec les sources	✅ RESPECTÉE
Décisions techniques	⏳ NON PRÉJUGÉES