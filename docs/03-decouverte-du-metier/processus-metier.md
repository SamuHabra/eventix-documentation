# Processus métier — Eventix

> **Statut :** Référence métier
> **Périmètre :** MVP Eventix
> **Marché initial :** Cameroun
> **Dernière consolidation :** PM01 → PM72

---

## 1. Objectif du document

Ce document décrit les principaux processus métier d'Eventix, leurs enchaînements, leurs règles, leurs états et leurs invariants.

Il constitue la référence permettant de comprendre **ce que doit faire Eventix**, indépendamment de la technologie utilisée pour l'implémenter.

Le document distingue explicitement :

* les fonctionnalités du **MVP** ;
* les fonctionnalités **futures** ;
* les règles métier ;
* les dépendances entre processus ;
* les invariants qui doivent toujours être respectés.

---

# 2. Périmètre MVP

Le MVP couvre principalement :

* création et gestion d'événements ;
* vérification de la crédibilité des organisateurs et événements ;
* publication d'événements ;
* configuration des billets ;
* recherche et consultation d'événements ;
* réservation ;
* paiement ;
* émission du billet ;
* téléchargement du billet ;
* envoi du billet par email ;
* contrôle du billet par QR Code ;
* gestion des événements annulés ou reportés ;
* remboursements ;
* signalements ;
* bannissement des organisations frauduleuses ;
* clôture financière ;
* retrait des fonds par l'organisateur ;
* historique métier participant et organisateur.

### Hors MVP

Les fonctionnalités suivantes ne font **pas partie du MVP** :

* vente physique ;
* points de vente physiques ;
* agents de vente physique ;
* envoi du billet par WhatsApp ;
* marketplace de revente ;
* quiz live ;
* reels ;
* cloud photos/vidéos ;
* fonctionnalités sociales avancées ;
* autres extensions non explicitement validées.

---

# 3. Acteurs métier

## 3.1 Participant

Utilisateur qui peut :

* consulter les événements ;
* réserver ;
* payer ;
* obtenir des billets ;
* consulter son historique ;
* participer aux événements ;
* signaler un événement ou un organisateur.

## 3.2 Organisateur

Utilisateur disposant de capacités d'organisation permettant de :

* créer des événements ;
* configurer les billets ;
* soumettre un événement à vérification ;
* gérer ses événements ;
* consulter ses ventes et statistiques ;
* consulter son historique ;
* récupérer les fonds disponibles.

## 3.3 Agent de contrôle

Acteur chargé de contrôler les billets à l'entrée d'un événement.

Il utilise Eventix pour :

* scanner un QR Code ;
* vérifier un billet ;
* autoriser ou refuser l'accès.

> L'agent de contrôle n'est pas un vendeur physique dans le MVP.

## 3.4 Eventix

La plateforme assure notamment :

* la gestion des événements ;
* la vérification ;
* la réservation ;
* le paiement ;
* l'émission des billets ;
* le contrôle ;
* les remboursements ;
* la sécurité ;
* la gestion financière ;
* la conservation de l'historique.

---

# 4. Cycle de vie d'un événement

Le cycle général est :

```text
BROUILLON
    ↓
SOUMIS
    ↓
EN VÉRIFICATION
    ↓
VALIDÉ
    ↓
PUBLIÉ
    ↓
VENTES
    ↓
EN COURS
    ↓
TERMINÉ
    ↓
CLÔTURÉ
    ↓
ARCHIVÉ
```

Branches possibles :

```text
PUBLIÉ ─────→ ANNULÉ
   │
   └────────→ REPORTÉ
```

---

# 5. PM01 → PM33 — Cycle de vie de l'événement

## PM01 — Création

L'organisateur peut créer un événement.

L'événement commence à l'état :

```text
DRAFT
```

Aucune publication publique n'est encore possible.

---

## PM02 — Configuration

L'organisateur configure notamment :

* nom ;
* description ;
* date ;
* heure ;
* lieu ;
* capacité ;
* catégories de billets ;
* prix ;
* informations nécessaires à l'événement.

---

## PM03 — Modification

Un événement en brouillon peut être modifié.

Une modification importante après publication peut nécessiter une nouvelle vérification selon les règles définies par Eventix.

---

## PM04 — Soumission

Lorsque l'organisateur considère son événement prêt :

```text
DRAFT
  ↓
SUBMITTED
```

L'événement entre dans le processus de vérification.

---

## PM05 — Vérification de l'organisateur et de l'événement

La vérification a pour objectif principal de déterminer si l'organisateur et son événement sont **suffisamment crédibles pour être proposés aux utilisateurs**.

Cette vérification vise notamment à lutter contre :

* les événements fictifs ;
* les fausses organisations ;
* les arnaques ;
* les événements frauduleux.

> La vérification n'est pas simplement une vérification technique de formulaire.

---

## PM06 — Décision de vérification

La vérification peut aboutir à :

```text
VALIDÉ
REFUSÉ
VÉRIFICATION COMPLÉMENTAIRE
```

Un événement refusé ne peut pas être publié.

---

## PM07 — Publication

Après validation :

```text
VALIDÉ
   ↓
PUBLIÉ
```

L'événement devient visible publiquement et peut entrer dans son cycle de vente.

---

## PM08 — Recherche et découverte

Le participant peut :

* rechercher des événements ;
* consulter leur détail ;
* filtrer les résultats ;
* consulter les informations nécessaires avant achat.

---

## PM09 — Disponibilité des billets

Eventix doit présenter les disponibilités correspondant à la configuration de l'événement.

La disponibilité doit tenir compte notamment :

* des billets vendus ;
* des billets temporairement réservés ;
* des billets encore disponibles.

---

## PM10 — Début des ventes

Lorsque les conditions de publication et de disponibilité sont satisfaites, les billets peuvent être achetés.

---

## PM11 — Achat d'un événement gratuit

Pour un événement gratuit, le parcours financier peut être absent.

Le participant obtient néanmoins un billet ou une preuve d'accès conforme au modèle métier.

---

## PM12 — Fermeture des ventes

Les ventes peuvent être arrêtées selon les règles définies pour l'événement.

---

## PM13 — Début de l'événement

À l'heure de début :

```text
VENTES → FERMÉES
```

Aucune nouvelle réservation normale ne peut être créée pour l'événement.

---

## PM14 — Contrôle des billets

Les participants présentent leur billet au contrôle.

Le système vérifie notamment :

* authenticité ;
* événement ;
* statut ;
* utilisation antérieure.

---

## PM15 — Billet valide

Si toutes les vérifications sont positives :

```text
VALID
 ↓
ACCÈS AUTORISÉ
 ↓
USED
```

---

## PM16 — Billet déjà utilisé

Un billet déjà utilisé est refusé.

Motif :

```text
Billet déjà utilisé
```

---

## PM17 — Billet annulé

Un billet annulé est refusé.

Motif :

```text
Billet annulé
```

---

## PM18 — Billet d'un autre événement

Un billet authentique appartenant à un autre événement est refusé.

Motif :

```text
Billet non valable pour cet événement
```

---

## PM19 — Fin de l'événement

À la fin de l'événement :

* les ventes restent fermées ;
* les contrôles cessent selon les règles opérationnelles ;
* l'événement entre dans son processus de clôture.

---

## PM20 — Billets non vendus

Les billets non vendus ne sont pas supprimés.

Ils restent dans l'historique comme disponibilités non consommées.

---

## PM21 — Fermeture automatique des ventes

À l'heure de début de l'événement :

```text
Début événement
      ↓
Ventes automatiquement arrêtées
```

---

## PM22 — Vérification complète du billet

Le contrôle vérifie :

1. l'authenticité ;
2. l'association à l'événement ;
3. le statut ;
4. l'utilisation antérieure.

---

## PM23 — Consommation du billet

Le billet devient `USED` après :

```text
Vérification réussie
       ↓
Accès autorisé
       ↓
USED
```

---

## PM24 — Réutilisation

Un billet `USED` présenté une seconde fois est refusé.

```text
INVALIDE — Billet déjà utilisé
```

---

## PM25 — Billet annulé

Un billet annulé ne donne plus accès à l'événement, même si son paiement historique a été confirmé.

---

## PM26 — Mauvais événement

Un billet valide mais associé à un autre événement est refusé.

---

## PM27 — Fin des disponibilités

Les billets non vendus restent conservés dans l'historique.

Ils ne deviennent pas automatiquement gratuits et ne peuvent plus être vendus après fermeture des ventes.

---

## PM28 — Clôture financière

Après la fin de l'événement, Eventix effectue les opérations financières nécessaires avant de déterminer le montant retirable.

---

## PM29 — Annulation d'un événement

Lorsqu'un événement est annulé pour une raison relevant de l'organisateur :

```text
ANNULATION
    ↓
Nouvelles ventes bloquées
    ↓
Participants informés
    ↓
Billets concernés invalidés
    ↓
Remboursements applicables
```

---

## PM30 — Report

Un report ne constitue pas une annulation.

Les billets existants sont conservés et rattachés à la nouvelle date.

```text
20 août
  ↓
REPORT
  ↓
10 septembre
  ↓
Billets conservés
```

---

## PM31 — Archivage

Après la fin de l'événement et la clôture des opérations nécessaires :

```text
TERMINÉ
   ↓
CLÔTURÉ
   ↓
ARCHIVÉ
```

L'événement n'est pas supprimé.

---

## PM32 — Suppression

Un événement peut être supprimé uniquement lorsqu'aucune opération métier irréversible n'a eu lieu.

Après une opération irréversible :

```text
❌ Suppression définitive
```

L'événement doit être annulé, masqué ou archivé selon son état.

---

## PM33 — Historique métier

Eventix conserve les principales opérations métier.

Exemple :

```text
Événement créé
Configuration modifiée
Soumis
Vérifié
Validé
Publié
Billet vendu
Billet contrôlé
Événement terminé
Clôturé
Archivé
```

Le MVP ne nécessite pas un journal exhaustif de chaque clic utilisateur.

---

# 6. PM34 → PM40 — Gestion des comptes

## PM34 — Création du compte participant

Le participant n'est pas obligé de créer un compte avant de commencer son parcours.

Le compte intervient lorsque l'identification devient nécessaire.

```text
Découverte
   ↓
Choix événement
   ↓
Choix billet
   ↓
Réservation
   ↓
Identification
   ↓
Compte créé ou compte existant
   ↓
Paiement
```

---

## PM35 — Authentification

Le participant peut s'authentifier avec :

```text
Email + mot de passe
OU
Téléphone + mot de passe
```

---

## PM36 — Création minimale

Le MVP applique le principe de minimisation des données.

Le compte peut être créé avec :

```text
Email OU téléphone
+
Mot de passe
```

Les informations complémentaires peuvent être ajoutées ultérieurement.

---

## PM37 — Historique participant

Le participant peut consulter :

* billets ;
* achats ;
* réservations ;
* remboursements ;
* événements passés ;
* informations pertinentes relatives à son activité.

Les informations internes de sécurité et de fraude ne sont pas exposées.

---

## PM38 — Identité utilisateur unique

Un même compte utilisateur peut être :

```text
PARTICIPANT
+
ORGANISATEUR
```

Le rôle organisateur correspond à une capacité accordée au compte.

---

## PM39 — Obtention des capacités d'organisateur

Un utilisateur autorisé peut créer un événement sans attendre sa vérification complète.

Cependant :

```text
Création
   ↓
BROUILLON
   ↓
Soumission
   ↓
Vérification
   ↓
Validation
   ↓
Publication
```

Un événement ne peut donc pas être publié librement.

---

## PM40 — Historique organisateur

L'organisateur peut consulter son historique métier :

* événements ;
* ventes ;
* billets ;
* remboursements ;
* annulations ;
* revenus ;
* opérations financières pertinentes.

Les informations internes de sécurité et d'administration restent privées à Eventix.

---

# 7. PM41 → PM47 — Réservation, paiement et billet

## PM41 — Création d'une réservation

Lorsqu'un participant sélectionne un billet disponible :

```text
Billet disponible
      ↓
Réservation temporaire
      ↓
Disponibilité bloquée
```

La réservation dure **5 minutes**.

> Une réservation n'est pas un achat.

---

## PM42 — Expiration

Après 5 minutes sans confirmation appropriée :

```text
PENDING
   ↓
EXPIRED
   ↓
Disponibilité libérée
```

L'expiration de la réservation ne signifie pas automatiquement que le paiement a échoué.

---

## PM43 — Paiement tardif

Si un paiement arrive après expiration :

```text
Paiement confirmé
      ↓
Réconciliation
      ↓
┌───────────────┴───────────────┐
↓                               ↓
Billet disponible            Billet vendu
↓                               ↓
Attribution                  Remboursement
```

---

## PM44 — Idempotence du paiement

Une même confirmation de paiement reçue plusieurs fois ne doit produire qu'un seul effet métier.

Elle ne doit jamais provoquer :

* deux achats ;
* deux billets ;
* deux remboursements ;
* deux débits métier.

---

## PM45 — Émission du billet

Après confirmation définitive du paiement :

```text
Paiement confirmé
      ↓
Achat finalisé
      ↓
Billet émis
      ↓
QR Code
```

Le billet est distinct de :

* la réservation ;
* le paiement ;
* la transaction.

---

## PM46 — Obtention du billet

Dans le MVP, le participant obtient son billet :

* dans son compte Eventix ;
* par téléchargement direct ;
* par email.

```text
Billet émis
    ↓
┌──────┼────────────┐
↓      ↓            ↓
Compte Téléchargement Email
```

**WhatsApp est hors MVP.**

---

## PM47 — Échec d'émission

Si le paiement est confirmé mais que l'émission du billet échoue techniquement :

```text
Paiement confirmé
      ↓
Émission échouée
      ↓
Achat conservé
      ↓
Nouvelle tentative
      ↓
Billet émis
```

Le système ne doit pas :

* débiter à nouveau ;
* créer un deuxième achat ;
* générer plusieurs billets pour la même transaction.

---

# 8. PM48 — Contrôle et mode dégradé

Le contrôle multi-scanners nécessite un **état partagé fiable**.

## Mode normal

```text
             État partagé
                  │
          ┌───────┴───────┐
          ↓               ↓
      Scanner A       Scanner B
          │               │
          └───────┬───────┘
                  ↓
        Contrôles parallèles
```

## Mode dégradé

Si l'état partagé fiable n'est plus disponible :

```text
Synchronisation indisponible
           ↓
      Mode dégradé
           ↓
    Un seul scanner actif
           ↓
    Contrôles séquentiels
```

Les autres scanners sont temporairement empêchés de valider des billets.

### Trade-off

| Mode                                       |       Débit |    Fiabilité |
| ------------------------------------------ | ----------: | -----------: |
| Plusieurs scanners synchronisés            |       Élevé |       Élevée |
| Un seul scanner                            | Plus faible |  Très élevée |
| Plusieurs scanners indépendants hors ligne |       Élevé | Insuffisante |

Eventix privilégie la **fiabilité du contrôle** sur le débit lorsque l'état partagé ne peut plus être garanti.

---

# 9. PM49 → PM51 — Remboursements

## PM49 — Déclenchement

Un remboursement peut être déclenché :

* automatiquement par une règle métier ;
* par Eventix lorsqu'une décision de remboursement est prise.

Exemples :

* événement annulé ;
* paiement tardif alors que le billet n'est plus disponible.

---

## PM50 — Montant

Le montant de référence est le montant effectivement payé pour la transaction concernée.

```text
Montant payé
     ↓
Montant de référence du remboursement
```

Le prix actuel du billet ne modifie pas rétroactivement le montant payé.

---

## PM51 — Échec

Un remboursement qui échoue techniquement reste traçable et peut être retenté.

```text
Remboursement requis
       ↓
Tentative
       ↓
FAILED
       ↓
Nouvelle tentative
       ↓
COMPLETED
```

Un échec technique ne signifie pas que l'obligation de remboursement disparaît.

---

# 10. PM52 → PM56 — Signalement et bannissement

## PM52 — Signalement

Dans le MVP, un participant peut signaler :

* un événement ;
* un organisateur.

---

## PM53 — Contenu

Un signalement contient :

```text
Motif obligatoire
+
Description facultative
```

Les motifs sont prédéfinis afin de permettre la catégorisation.

---

## PM54 — Traitement

Un signalement ne constitue pas automatiquement une preuve de fraude.

```text
Signalement
    ↓
Enregistrement
    ↓
Analyse
    ↓
Évaluation du risque
```

Selon le résultat :

* aucune action ;
* vérification renforcée ;
* suspension ;
* annulation ;
* remboursement ;
* bannissement.

---

## PM55 — Bannissement

Lorsqu'une organisation est reconnue frauduleuse :

```text
Organisation bannie
        ↓
Nouvelles activités d'organisation bloquées
        ↓
Événements existants évalués
```

Les événements existants ne sont pas automatiquement supprimés sans analyse.

Ils peuvent être :

* maintenus ;
* suspendus ;
* annulés ;
* soumis à une nouvelle vérification.

---

## PM56 — Contournement

Une organisation bannie peut tenter de revenir via une nouvelle identité.

Eventix analyse les signaux disponibles.

```text
Nouvelle organisation
        ↓
Analyse
        ↓
┌──────────────┬──────────────────┐
↓              ↓                  ↓
Aucun lien   Lien suspect      Lien confirmé
↓              ↓                  ↓
Activité     Vérification      Mesure de
normale      renforcée         blocage
```

Une suspicion de contournement déclenche une vérification renforcée mais ne constitue pas automatiquement une preuve.

---

# 11. PM57 → PM61 — Clôture financière et retraits

## PM57 — Clôture

La clôture financière est terminée lorsque les opérations nécessaires ont été traitées et que le montant net dû à l'organisateur peut être déterminé.

```text
Événement terminé
       ↓
Paiements
       ↓
Remboursements
       ↓
Ajustements
       ↓
Clôture
```

---

## PM58 — Montant net

```text
Ventes confirmées
       ↓
− remboursements
       ↓
− frais / commissions applicables
       ↓
= montant net organisateur
```

Les règles précises concernant les frais, commissions et taxes sont définies séparément dans les règles financières et commerciales.

---

## PM59 — Retrait

Après clôture, le montant net devient disponible dans le solde de l'organisateur.

L'organisateur peut demander :

* la totalité ;
* ou une partie du solde disponible.

```text
Solde disponible
      ↓
Demande de retrait
      ↓
Montant ≤ solde
      ↓
Traitement
```

---

## PM60 — Échec du retrait

En cas d'échec :

```text
Retrait
  ↓
FAILED
  ↓
Montant restitué
  ↓
Solde disponible
  ↓
Nouvelle tentative
```

L'opération initiale reste enregistrée.

---

## PM61 — Retraits multiples

Un organisateur peut effectuer plusieurs retraits successifs tant qu'un solde est disponible.

```text
500 000 FCFA
     ↓
300 000 retirés
     ↓
200 000 disponibles
     ↓
200 000 retirables ultérieurement
```

---

# 12. PM62 → PM70 — Enchaînement des processus

## PM62 — Réservation → paiement → billet

```text
Réservation
     ↓
Paiement
     ↓
Confirmation
     ↓
Achat finalisé
     ↓
Billet
```

Ces trois domaines restent distincts :

```text
Réservation ≠ Paiement ≠ Billet
```

---

## PM63 — Expiration → paiement tardif → réconciliation

```text
Réservation PENDING
       ↓
5 minutes
       ↓
EXPIRED
       ↓
Paiement tardif
       ↓
Réconciliation
       ↓
Disponible → Billet
Indisponible → Remboursement
```

---

## PM64 — Annulation → remboursement

```text
Événement annulé
       ↓
Ventes bloquées
       ↓
Billets invalidés
       ↓
Participants informés
       ↓
Remboursements
```

---

## PM65 — Report → conservation du billet

```text
Ancienne date
     ↓
REPORT
     ↓
Nouvelle date
     ↓
Billet conservé
```

Un report n'entraîne pas automatiquement un remboursement.

---

## PM66 — Contrôle → billet

```text
Scan
 ↓
Authenticité
 ↓
Événement
 ↓
Statut
 ↓
Utilisation
 ↓
Décision
```

En cas de succès :

```text
VALID
 ↓
Accès
 ↓
USED
```

---

## PM67 — Contrôle → mode dégradé

```text
État partagé fiable
        ↓
Plusieurs scanners
```

Sinon :

```text
État partagé indisponible
        ↓
Un seul scanner actif
```

---

## PM68 — Signalement → analyse → mesure

```text
Signalement
     ↓
Analyse
     ↓
Évaluation
     ↓
Mesure adaptée
```

La mesure peut être inexistante, préventive ou coercitive selon les résultats.

---

## PM69 — Fraude → bannissement

```text
Fraude confirmée
       ↓
Bannissement
       ↓
Blocage activité
       ↓
Évaluation événements existants
       ↓
Mesure adaptée
```

Une annulation peut entraîner un remboursement.

---

## PM70 — Fin → clôture → retrait

```text
Événement terminé
       ↓
Clôture financière
       ↓
Montant net
       ↓
Solde disponible
       ↓
Retrait
```

---

# 13. PM71 — Invariants métier

Les invariants sont des règles qui doivent rester vraies indépendamment de l'implémentation technique.

## Billet

> Un billet ne peut être utilisé avec succès qu'une seule fois.

## Paiement

> Une même confirmation de paiement ne peut produire qu'un seul effet métier.

## Remboursement

> Une même obligation de remboursement ne doit pas produire plusieurs remboursements effectifs.

## Réservation

> Une réservation expirée libère la disponibilité correspondante.

## Disponibilité

> Une même disponibilité ne peut pas être attribuée simultanément à plusieurs achats valides.

## Finances

> Un organisateur ne peut jamais retirer plus que son solde disponible.

## Historique

> Une opération métier réalisée doit rester traçable.

## Événement

> Un événement terminé ne peut plus recevoir de nouvelles ventes.

## Sécurité

> Un signalement n'est pas automatiquement une preuve de fraude.

## Bannissement

> Une organisation bannie ne doit pas pouvoir poursuivre librement son activité d'organisation.

## Contrôle

> Eventix ne doit pas autoriser plusieurs contrôles concurrents lorsqu'il ne peut pas garantir un état partagé fiable.

---

# 14. PM72 — Vue globale

Le parcours principal est :

```text
                         COMPTE UTILISATEUR
                                │
                    ┌───────────┴───────────┐
                    ↓                       ↓
               PARTICIPANT             ORGANISATEUR
                    │                       │
                    │                       ↓
                    │                   ÉVÉNEMENT
                    │                       │
                    │                 Vérification
                    │                       │
                    │                   Publication
                    │                       │
                    ↓                       ↓
                RÉSERVATION ←──────────── BILLETS
                    │
                    ↓
                 PAIEMENT
                    │
              ┌─────┴─────┐
              ↓           ↓
          Confirmé       Tardif
              ↓           ↓
           ACHAT      RÉCONCILIATION
              ↓        ↙       ↘
           BILLET   Disponible  Vendu
              ↓        ↓          ↓
           CONTRÔLE  BILLET   REMBOURSEMENT
              ↓
            USED
              │
              ↓
       FIN ÉVÉNEMENT
              │
              ↓
     CLÔTURE FINANCIÈRE
              │
              ↓
      SOLDE ORGANISATEUR
              │
              ↓
           RETRAIT
```

Processus de confiance et sécurité :

```text
ÉVÉNEMENT / ORGANISATEUR
          ↓
      SIGNALEMENT
          ↓
        ANALYSE
          ↓
   ┌──────┴────────┐
   ↓               ↓
Pas de mesure    Risque confirmé
                   ↓
               BANNISSEMENT
                   ↓
           Évaluation événements
                   ↓
              Annulation
                   ↓
              Remboursement
```

---

# 15. États métier principaux

## 15.1 Événement

```text
DRAFT
SUBMITTED
UNDER_REVIEW
VALIDATED
PUBLISHED
ONGOING
COMPLETED
CLOSED
ARCHIVED
CANCELLED
```

---

## 15.2 Réservation

```text
PENDING
CONFIRMED
EXPIRED
CANCELLED
```

---

## 15.3 Paiement

```text
PENDING
CONFIRMED
FAILED
```

Une confirmation tardive peut déclencher une réconciliation même si la réservation est déjà `EXPIRED`.

---

## 15.4 Billet

```text
ISSUED
USED
CANCELLED
```

---

## 15.5 Remboursement

```text
PENDING
PROCESSING
FAILED
COMPLETED
```

---

## 15.6 Retrait

```text
PENDING
PROCESSING
FAILED
COMPLETED
```

---

## 15.7 Organisation

```text
ACTIVE
UNDER_REVIEW
SUSPENDED
BANNED
```

---

# 16. Règles métier critiques

## Règle 1 — Séparation réservation / paiement / billet

```text
Réservation ≠ Paiement ≠ Billet
```

---

## Règle 2 — Expiration

Une réservation expire après **5 minutes** lorsqu'elle reste `PENDING`.

---

## Règle 3 — Paiement tardif

Un paiement arrivé après expiration doit être **réconcilié**, pas ignoré automatiquement.

---

## Règle 4 — Idempotence

Les paiements et remboursements doivent être traités de manière idempotente.

---

## Règle 5 — Unicité du billet

Une disponibilité ne peut pas être attribuée à plusieurs achats valides.

---

## Règle 6 — Contrôle

Un billet validé devient `USED` lorsque l'accès est effectivement autorisé.

---

## Règle 7 — Multi-scanners

Plusieurs scanners nécessitent un état partagé fiable.

Sinon :

```text
UN SEUL SCANNER ACTIF
```

---

## Règle 8 — Annulation

Un événement annulé ne peut plus recevoir de nouvelles ventes et les billets concernés deviennent invalides.

---

## Règle 9 — Report

Un report conserve les billets existants et les rattache à la nouvelle date.

---

## Règle 10 — Finances

L'organisateur ne peut retirer les fonds qu'après la clôture financière nécessaire.

---

## Règle 11 — Solde

Un organisateur ne peut jamais retirer plus que son solde disponible.

---

## Règle 12 — Fraude

Un signalement déclenche une analyse mais ne constitue pas automatiquement une preuve.

---

## Règle 13 — Bannissement

Une fraude confirmée peut entraîner le bannissement de l'organisation.

---

## Règle 14 — Historique

Les principales opérations métier sont conservées afin de garantir la traçabilité.

---

# 17. MVP vs fonctionnalités futures

## 17.1 MVP

### Événements

* [x] Création
* [x] Configuration
* [x] Vérification
* [x] Publication
* [x] Vente en ligne
* [x] Annulation
* [x] Report
* [x] Archivage

### Participant

* [x] Compte utilisateur
* [x] Email ou téléphone
* [x] Mot de passe
* [x] Historique
* [x] Achat

### Réservation

* [x] Réservation temporaire
* [x] Expiration à 5 minutes
* [x] Libération de disponibilité
* [x] Réconciliation des paiements tardifs

### Paiement

* [x] Confirmation
* [x] Échec
* [x] Idempotence
* [x] Réconciliation

### Billet

* [x] Émission automatique
* [x] QR Code
* [x] Téléchargement
* [x] Email
* [x] Historique

### Contrôle

* [x] Scan
* [x] Vérification du billet
* [x] Détection du billet déjà utilisé
* [x] Détection du mauvais événement
* [x] Détection du billet annulé
* [x] Mode multi-scanners avec état partagé
* [x] Mode dégradé mono-scanner

### Sécurité

* [x] Vérification de l'organisateur
* [x] Vérification de l'événement
* [x] Signalement
* [x] Analyse
* [x] Suspension
* [x] Bannissement

### Finance

* [x] Remboursement
* [x] Clôture financière
* [x] Solde organisateur
* [x] Retrait partiel
* [x] Retraits multiples

---

# 18. Fonctionnalités futures

Les fonctionnalités suivantes pourront être ajoutées après le MVP.

## Distribution

* [ ] WhatsApp
* [ ] autres canaux de distribution

## Vente physique

* [ ] points de vente physiques
* [ ] agents de vente
* [ ] kiosques
* [ ] synchronisation des ventes physiques

> **La vente physique n'appartient pas au MVP.**

## Marketplace

* [ ] revente de billets
* [ ] transfert de billets
* [ ] marketplace après sold-out
* [ ] mécanismes anti-fraude spécifiques à la revente

## Expérience événementielle

* [ ] quiz live
* [ ] fonctionnalités sociales
* [ ] reels
* [ ] photos/vidéos cloud
* [ ] interactions temps réel avancées

## Contrôle avancé

* [ ] architectures offline plus avancées
* [ ] synchronisation distribuée avancée
* [ ] mécanismes de reprise sophistiqués
* [ ] optimisation du contrôle à très grande échelle

---

# 19. Principes métier fondamentaux d'Eventix

## Principe 1 — La confiance avant la vente

Eventix doit éviter de devenir un outil permettant de diffuser facilement des événements frauduleux.

```text
Organisateur
     ↓
Vérification
     ↓
Crédibilité suffisante
     ↓
Publication
```

---

## Principe 2 — La réservation n'est pas le paiement

Une réservation représente une **intention temporaire**.

Un paiement représente une **opération financière**.

---

## Principe 3 — Le paiement n'est pas le billet

Le paiement confirmé entraîne l'achat finalisé puis l'émission du billet.

---

## Principe 4 — L'historique est conservé

Une opération métier importante ne doit pas disparaître simplement parce que l'état courant a changé.

---

## Principe 5 — La fiabilité prime sur la rapidité lorsqu'il existe un risque d'incohérence

C'est notamment le cas du contrôle des billets :

```text
Plusieurs scanners + synchronisation fiable
                ↓
           ACCEPTÉ

Synchronisation non fiable
                ↓
        Un seul scanner
```

---

## Principe 6 — Les opérations financières sont traçables

Paiements, remboursements et retraits doivent conserver leur historique.

---

## Principe 7 — Les décisions de sécurité sont proportionnées

```text
Signalement
    ≠
Fraude confirmée
```

Une suspicion entraîne une analyse avant une sanction définitive.

---

## Principe 8 — L'historique métier ne doit pas être détruit par commodité technique

```text
Suppression technique
        ≠
Disparition de l'historique métier
```

---

# 20. Résumé du modèle métier

Eventix peut être résumé par cinq grands domaines :

```text
┌─────────────────────────────────────────────────┐
│                  EVENTIX                        │
├─────────────────────────────────────────────────┤
│ 1. CONFIANCE                                    │
│    Organisateur → Vérification → Publication    │
│                                                 │
│ 2. COMMERCE                                     │
│    Réservation → Paiement → Billet              │
│                                                 │
│ 3. ACCÈS                                        │
│    Billet → Scan → Validation → USED            │
│                                                 │
│ 4. SÉCURITÉ                                     │
│    Signalement → Analyse → Sanction              │
│                                                 │
│ 5. FINANCE                                      │
│    Ventes → Clôture → Solde → Retrait            │
└─────────────────────────────────────────────────┘
```

Le modèle central est donc :

```text
                    CONFIANCE
                       ↓
                  ÉVÉNEMENT
                       ↓
                    VENTES
                       ↓
                 RÉSERVATION
                       ↓
                    PAIEMENT
                       ↓
                     BILLET
                       ↓
                    CONTRÔLE
                       ↓
                  PARTICIPATION
                       ↓
              CLÔTURE FINANCIÈRE
                       ↓
                    RETRAIT
```

Avec deux mécanismes transversaux :

```text
                    SÉCURITÉ
                       ↑
              Signalement / Fraude
                       ↓
                  Bannissement

                    HISTORIQUE
                       ↑
          Toutes les opérations importantes
```

> **Ce document définit le comportement métier attendu d'Eventix. L'architecture logicielle, l'architecture des données, l'infrastructure, la scalabilité, la performance et les choix technologiques devront ensuite être conçus pour respecter ces règles métier.**
