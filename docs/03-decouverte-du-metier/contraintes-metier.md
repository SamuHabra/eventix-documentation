# Contraintes métier — Eventix

**Version :** 1.0
**Statut :** Validé — MVP
**Marché initial :** Cameroun
**Dernière mise à jour :** 2026-08-10

---

## 1. Objectif

Ce document définit les contraintes qui encadrent le fonctionnement métier d'Eventix pour le MVP.

Une contrainte métier définit une **limite, une condition ou un périmètre** dans lequel Eventix doit fonctionner.

Elle ne décrit pas l'implémentation technique.

---

# 2. Périmètre géographique et monétaire

## CM01 — Marché initial

Le marché initial d'Eventix est le **Cameroun**.

Aucune restriction de nationalité n'est imposée aux participants.

Les éventuelles capacités internationales seront limitées par les moyens de paiement, la devise et les contraintes réglementaires supportés par le MVP.

---

## CM02 — Devise du MVP

Tous les prix des événements et des billets du MVP sont exprimés en **FCFA (XAF)**.

```text
10 000 FCFA → autorisé
10 USD      → non supporté dans le MVP
```

La gestion multi-devises est une évolution future.

---

# 3. Paiements

## CM03 — Paiement en ligne

Pour le MVP, les paiements en ligne sont effectués exclusivement via **Mobile Money**.

```text
Achat en ligne
      ↓
Mobile Money
      ↓
Confirmation
      ↓
Billet
```

Les autres moyens de paiement en ligne pourront être ajoutés ultérieurement.

---

## CM04 — Connexion nécessaire à l'achat

Une connexion Internet est nécessaire pour initier un achat en ligne.

Cependant, une interruption de connexion après le lancement du paiement ne doit pas empêcher Eventix de traiter ultérieurement le résultat de la transaction.

```text
Achat
 ↓
Mobile Money
 ↓
Interruption réseau
 ↓
Confirmation ultérieure
 ↓
Réconciliation
```

L'état de la connexion ne constitue donc pas à lui seul une preuve d'échec du paiement.

---

# 4. Configuration des événements

## CM05 — Le type de tarification est figé

Un événement peut être :

* `GRATUIT` ;
* `PAYANT`.

Une fois que l'événement a commencé à attribuer ou vendre des billets, ce statut ne peut plus être modifié.

```text
GRATUIT → PAYANT ❌
PAYANT  → GRATUIT ❌
```

---

## CM06 — Modifications contrôlées après publication

Après publication, les modifications apportées à un événement sont limitées aux informations que les règles Eventix autorisent à modifier dans son état courant.

Les informations susceptibles d'affecter les engagements déjà pris envers les participants doivent être soumises à des règles spécifiques.

---

## CM07 — Pas de suppression physique des événements

Un événement créé ne peut pas être supprimé physiquement du système.

Il peut être :

* masqué ;
* archivé ;
* annulé ;

selon son état et les opérations métier déjà réalisées.

L'objectif est de préserver l'historique et la traçabilité.

```text
Supprimer
   ≠
Masquer
   ≠
Archiver
   ≠
Annuler
```

---

# 5. Vérification et confiance

## CM08 — Suspension d'un organisateur

Lorsqu'un organisateur est suspendu :

* ses événements sont masqués ;
* les nouvelles ventes sont bloquées.

La suspension d'un organisateur ne signifie pas automatiquement que les billets déjà achetés sont annulés.

```text
Suspension
    ↓
Événements masqués
    +
Nouvelles ventes bloquées
```

Les mesures supplémentaires sont déterminées selon la situation.

---

## CM09 — Vérification préalable des événements

Un événement doit être vérifié par Eventix avant d'être rendu publiquement visible.

```text
Création
   ↓
Soumission
   ↓
Vérification
   ↓
Validation
   ↓
Publication
```

Un événement non vérifié ne peut donc pas être présenté comme un événement officiel visible sur la plateforme.

---

## CM10 — Nouvelle vérification après modification sensible

Une modification importante apportée à un événement déjà vérifié peut nécessiter une nouvelle vérification.

Dans ce cas, la modification doit être soumise au processus de vérification avant d'être considérée comme définitivement validée.

```text
Événement vérifié
       ↓
Modification sensible
       ↓
Nouvelle vérification
       ↓
Validation / Refus
```

La liste exacte des modifications considérées comme sensibles sera définie dans les processus métier.

---

# 6. Données et traçabilité

## CM11 — Conservation des données nécessaires

Eventix conserve les données nécessaires :

* à la traçabilité ;
* à la sécurité ;
* aux opérations financières ;
* aux statistiques ;
* à la résolution des litiges ;
* aux obligations applicables.

Les données qui ne sont plus nécessaires peuvent être supprimées ou anonymisées selon les règles applicables.

```text
Donnée
  ↓
Nécessaire ?
 ├── Oui → Conservation
 └── Non → Suppression / Anonymisation
```

Les durées précises de conservation seront définies ultérieurement.

---

# 7. Règlement financier

## CM12 — Réconciliation avant settlement

Avant tout règlement à l'organisateur, Eventix doit réconcilier les opérations financières pertinentes.

La détermination du montant payable doit notamment tenir compte :

* des ventes ;
* des paiements confirmés ;
* des remboursements ;
* des frais ;
* des ajustements ;
* des montants éventuellement bloqués.

```text
Ventes
  +
Paiements confirmés
  -
Remboursements
  -
Frais
  -
Montants bloqués
        ↓
Réconciliation
        ↓
Montant payable
        ↓
Settlement
```

Le calcul détaillé des frais et des règles de blocage fera l'objet de décisions métier spécifiques.

---

# 8. Réseau de distribution physique

## CM13 — Gestion des agents physiques par Eventix

Les agents des points physiques sont gérés par **Eventix**.

Ils ne sont pas des employés ou des comptes créés directement par les organisateurs.

L'organisateur peut autoriser l'utilisation de son événement par un point physique ou par des agents habilités, mais ne gère pas leur identité dans Eventix.

```text
                    EVENTIX
                       │
          ┌────────────┴────────────┐
          ↓                         ↓
   Organisateurs             Points physiques
                                    │
                               Agents Eventix
                                    │
                                    ↓
                                  Vente
```

---

# 9. Contraintes spécifiques au MVP

Les décisions suivantes sont volontairement limitées au MVP :

| Domaine           | Contrainte MVP                |
| ----------------- | ----------------------------- |
| Marché            | Cameroun                      |
| Devise            | FCFA                          |
| Paiement en ligne | Mobile Money                  |
| Vente physique    | Connexion obligatoire         |
| Paiement physique | CASH autorisé                 |
| Type d'événement  | Gratuit ou payant             |
| Vérification      | Obligatoire avant publication |
| Mode offline      | Non supporté                  |
| Multi-devise      | Non supporté                  |
| Marketplace       | Non supportée                 |
| Revente           | Non supportée                 |

---

# 10. Évolutions futures

Les contraintes suivantes pourront évoluer dans les versions futures.

## CM-F01 — Internationalisation

Eventix pourra prendre en charge :

* plusieurs pays ;
* plusieurs devises ;
* plusieurs moyens de paiement ;
* des contraintes réglementaires propres à chaque marché.

---

## CM-F02 — Mode offline des points physiques

Une future version pourra permettre aux points physiques de vendre temporairement sans connexion.

Cette évolution devra traiter notamment :

* la synchronisation ;
* les conflits ;
* la cohérence du stock ;
* la double vente ;
* la réconciliation.

---

## CM-F03 — Marketplace de revente

Après le `SOLD OUT` d'un événement, une future version pourra permettre la revente sécurisée des billets via une Marketplace Eventix.

Cette fonctionnalité sera traitée comme un domaine métier spécifique.

---

## CM-F04 — Nouveaux moyens de paiement

Des moyens de paiement supplémentaires pourront être ajoutés après le MVP.

---

## CM-F05 — Multi-devises

La gestion de plusieurs devises pourra être introduite lors de l'expansion internationale.

---

# 11. Points nécessitant encore une décision

Certaines contraintes ont volontairement été identifiées sans être complètement figées.

Elles seront traitées dans `questions-metier-ouvertes.md`.

Notamment :

* durée exacte de conservation des données ;
* liste exacte des modifications nécessitant une nouvelle vérification ;
* conditions précises de suspension d'un organisateur ;
* règles détaillées de règlement financier ;
* frais Eventix ;
* conditions réglementaires applicables ;
* moyens de paiement futurs ;
* règles d'expansion internationale.

---

# 12. Principes fondamentaux

### P01 — Préserver l'historique

Une opération métier réalisée ne doit pas être effacée de manière à empêcher sa traçabilité.

### P02 — Séparer les états métier

```text
Réservation
≠ Paiement
≠ Billet
≠ Présence
≠ Settlement
```

### P03 — Protéger les engagements existants

Une modification future d'un événement ne doit pas invalider arbitrairement les billets déjà attribués.

### P04 — Centraliser les opérations critiques

Les opérations importantes doivent être enregistrées dans Eventix afin de garantir cohérence et traçabilité.

### P05 — Ne pas introduire inutilement de complexité dans le MVP

Les fonctionnalités distribuées ou complexes sont volontairement repoussées lorsqu'elles ne sont pas nécessaires au lancement.

Exemple :

```text
MVP
→ Vente physique connectée

Future
→ Vente offline
→ Synchronisation
→ Gestion des conflits
```

---

# 13. Statut du document

**Version :** 1.0
**Statut :** Validé pour le MVP
**Périmètre :** Eventix — Découverte métier
**Marché initial :** Cameroun

Ce document constitue la référence des contraintes métier actuellement validées.

Toute nouvelle contrainte ou modification d'une contrainte existante doit être discutée et validée par l'équipe avant d'être intégrée à cette version de référence.
