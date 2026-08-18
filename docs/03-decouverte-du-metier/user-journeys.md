# User Journeys — Eventix

> **Statut :** Version finale — Business Discovery
> **Périmètre :** MVP Eventix
> **Objectif :** Décrire l'expérience des acteurs principaux, du déclencheur initial jusqu'à la fin de leur parcours, y compris les cas alternatifs et les situations d'échec.

---

## 1. Objectif du document

Les processus métier décrivent **ce que fait Eventix**.

Les User Journeys décrivent **ce que vit l'utilisateur**.

Ce document permet de comprendre :

* pourquoi l'utilisateur arrive sur Eventix ;
* ce qu'il cherche à accomplir ;
* les étapes qu'il traverse ;
* les décisions qu'il doit prendre ;
* les problèmes qu'il peut rencontrer ;
* le résultat attendu de son parcours.

Les journeys sont décrits du point de vue de l'acteur et indépendamment de l'implémentation technique.

---

## 2. Acteurs concernés

| Acteur                   | Objectif principal                                          |
| ------------------------ | ----------------------------------------------------------- |
| Participant              | Trouver un événement et obtenir/utiliser un billet          |
| Organisateur             | Créer, publier et gérer un événement                        |
| Agent de contrôle        | Vérifier les billets et autoriser l'accès                   |
| Eventix / Administration | Garantir la confiance, la sécurité et le bon fonctionnement |

---

## 3. Journey J01 — Participant : découvrir un événement

### Objectif

Trouver un événement auquel le participant souhaite assister.

### Parcours principal

```text
Arrivée sur Eventix
       ↓
Découverte des événements
       ↓
Recherche / navigation
       ↓
Sélection d'un événement
       ↓
Consultation des détails
```

Le participant doit pouvoir comprendre notamment :

* ce qu'est l'événement ;
* où il se déroule ;
* quand il se déroule ;
* quelles catégories de billets existent ;
* combien coûte chaque billet ;
* la disponibilité ;
* les informations importantes communiquées par l'organisateur.

### Résultat

```text
Événement consulté
       ↓
┌──────┴──────┐
↓             ↓
Participer    Ne pas participer
↓
Choix du billet
```

---

## 4. Journey J02 — Participant : acheter un billet

### Objectif

Obtenir un billet valide pour un événement payant.

### Parcours principal

```text
Événement sélectionné
       ↓
Choix du billet
       ↓
Réservation
       ↓
Identification / compte
       ↓
Paiement
       ↓
Confirmation
       ↓
Billet émis
       ↓
Billet disponible
```

Le participant doit comprendre :

1. quel billet il réserve ;
2. combien il doit payer ;
3. combien de temps il dispose pour finaliser ;
4. si le paiement a réussi ;
5. si son achat est finalisé ;
6. où récupérer son billet.

---

## 5. Journey J03 — Participant : réservation

### Objectif

Bloquer temporairement une disponibilité pendant le paiement.

```text
Choix billet
    ↓
Réservation
    ↓
Disponibilité temporairement bloquée
    ↓
5 minutes
```

La réservation est explicitement temporaire.

### Possibilités

```text
                  Réservation
                       ↓
              ┌────────┴────────┐
              ↓                 ↓
          Paiement           Pas de paiement
          confirmé                ↓
              ↓                Expiration
          Achat finalisé          ↓
              ↓               Disponibilité
            Billet               libérée
```

> Une réservation n'est pas une confirmation définitive d'achat.

---

## 6. Journey J04 — Participant : paiement réussi

```text
Réservation
    ↓
Paiement
    ↓
Confirmation
    ↓
Achat finalisé
    ↓
Billet généré
    ↓
Billet accessible
```

Le participant reçoit une confirmation claire.

Le billet contient notamment les informations nécessaires au contrôle, dont le QR Code.

---

## 7. Journey J05 — Participant : paiement échoué

```text
Réservation
    ↓
Paiement
    ↓
ÉCHEC
    ↓
Participant informé
```

Le paiement échoué ne constitue pas un achat finalisé.

Le participant peut éventuellement recommencer le paiement tant que la réservation est encore valide.

```text
Paiement échoué
      ↓
Réservation encore valide ?
      ↓
   ┌──┴──┐
   ↓     ↓
  Oui    Non
   ↓      ↓
 Retry   Expiration
```

---

## 8. Journey J06 — Participant : réservation expirée

Le participant ne termine pas le paiement dans le délai de 5 minutes.

```text
Réservation PENDING
       ↓
5 minutes
       ↓
EXPIRED
       ↓
Billet libéré
```

Le participant doit alors recommencer le processus si le billet est toujours disponible.

```text
Expiration
    ↓
Billet disponible ?
   ┌─┴─┐
   ↓   ↓
 Oui  Non
   ↓   ↓
Nouvelle   Plus disponible
réservation
```

---

## 9. Journey J07 — Participant : paiement tardif

Cas particulier :

```text
Réservation
    ↓
Expiration
    ↓
Paiement reçu
```

Eventix effectue une réconciliation.

```text
Paiement tardif
      ↓
Réconciliation
      ↓
Billet encore disponible ?
      ↓
 ┌────┴────┐
 ↓         ↓
Oui       Non
 ↓         ↓
Billet   Remboursement
```

---

## 10. Journey J08 — Participant : obtention du billet

Après finalisation de l'achat :

```text
Achat finalisé
      ↓
Billet émis
      ↓
┌─────┴───────────┐
↓                 ↓
Compte Eventix    Email
↓                 ↓
Consultation      Réception
↓
Téléchargement
```

### MVP

* billet dans le compte ;
* téléchargement ;
* envoi par email.

### Hors MVP

* WhatsApp.

---

## 11. Journey J09 — Participant : préparation à l'événement

Avant l'événement :

```text
Participant
    ↓
Consulte son billet
    ↓
Télécharge / conserve son billet
    ↓
Se rend à l'événement
```

Le participant doit pouvoir retrouver facilement :

* événement ;
* date ;
* heure ;
* lieu ;
* catégorie de billet ;
* numéro de siège lorsque applicable ;
* QR Code.

---

## 12. Journey J10 — Participant : contrôle à l'entrée

```text
Arrivée
   ↓
Présentation du billet
   ↓
Scan QR Code
   ↓
Vérification
   ↓
Décision
```

### Billet valide

```text
Billet valide
     ↓
Accès autorisé
     ↓
Billet marqué USED
```

### Billet invalide

```text
Billet invalide
      ↓
Accès refusé
      ↓
Motif présenté à l'agent
```

Motifs possibles :

* billet inexistant ;
* billet frauduleux ;
* mauvais événement ;
* billet annulé ;
* billet déjà utilisé.

---

## 13. Journey J11 — Participant : billet déjà utilisé

```text
Scan
 ↓
Billet authentique
 ↓
Statut = USED
 ↓
ACCÈS REFUSÉ
```

> Un billet ne peut pas permettre deux accès réussis.

---

## 14. Journey J12 — Participant : événement annulé

```text
Événement publié
       ↓
ANNULATION
       ↓
Ventes bloquées
       ↓
Participant informé
       ↓
Billet invalidé
       ↓
Remboursement applicable
```

Le participant ne doit pas découvrir l'annulation uniquement à l'entrée de l'événement.

---

## 15. Journey J13 — Participant : événement reporté

```text
Événement initial
       ↓
REPORT
       ↓
Nouvelle date
       ↓
Participant informé
       ↓
Billet conservé
```

> Un report n'est pas traité comme une annulation automatique.

---

## 16. Journey J14 — Participant : remboursement

```text
Remboursement requis
       ↓
Traitement
       ↓
┌──────┴──────┐
↓             ↓
Succès       Échec
↓             ↓
COMPLETED    FAILED
               ↓
         Nouvelle tentative
```

Le participant doit pouvoir connaître l'état de son remboursement.

---

## 17. Journey J15 — Participant : signaler un événement

```text
Événement
    ↓
Signaler
    ↓
Choisir un motif
    ↓
Ajouter description facultative
    ↓
Envoyer
    ↓
Signalement enregistré
```

Le motif est obligatoire.

La description est facultative.

> Signalement ≠ fraude confirmée.

---

## 18. Journey J16 — Participant : signaler un organisateur

```text
Organisateur
     ↓
Signaler
     ↓
Motif
     ↓
Description facultative
     ↓
Envoi
     ↓
Analyse Eventix
```

---

## 19. Journey J17 — Organisateur : commencer avec Eventix

### Objectif

Devenir organisateur et publier un événement.

```text
Création / utilisation du compte
             ↓
       Accès organisateur
             ↓
       Création événement
```

---

## 20. Journey J18 — Organisateur : créer un événement

```text
Créer événement
      ↓
Brouillon
      ↓
Informations générales
      ↓
Configuration
      ↓
Billets
      ↓
Prix / disponibilité
      ↓
Enregistrer
```

L'événement reste privé tant qu'il n'est pas validé et publié.

---

## 21. Journey J19 — Organisateur : soumettre un événement

```text
DRAFT
  ↓
SUBMITTED
  ↓
UNDER_REVIEW
```

Eventix analyse la crédibilité de l'organisateur et de l'événement.

---

## 22. Journey J20 — Organisateur : événement validé

```text
Vérification
     ↓
VALIDÉ
     ↓
PUBLICATION
     ↓
Événement visible
     ↓
Ventes
```

---

## 23. Journey J21 — Organisateur : événement refusé

```text
Soumission
    ↓
Vérification
    ↓
REFUS
```

L'événement n'est pas publié.

Le refus concerne la possibilité de publier l'événement sur Eventix.

---

## 24. Journey J22 — Organisateur : gérer un événement publié

```text
Événement publié
      ↓
Suivi
      ↓
Ventes
      ↓
Statistiques
      ↓
Gestion
```

L'organisateur peut notamment suivre :

* billets vendus ;
* disponibilité ;
* état de l'événement ;
* activité financière pertinente.

---

## 25. Journey J23 — Organisateur : annuler un événement

```text
Événement actif
      ↓
Décision d'annulation
      ↓
ANNULÉ
      ↓
Ventes bloquées
      ↓
Participants informés
      ↓
Traitement des billets
      ↓
Remboursements
```

---

## 26. Journey J24 — Organisateur : reporter un événement

```text
Événement
   ↓
Décision de report
   ↓
Nouvelle date
   ↓
Participants informés
   ↓
Billets conservés
```

---

## 27. Journey J25 — Organisateur : fin d'événement

```text
Événement terminé
       ↓
Ventes fermées
       ↓
Contrôles terminés
       ↓
Clôture financière
```

La fin de l'événement ne rend pas automatiquement les fonds disponibles.

---

## 28. Journey J26 — Organisateur : clôture financière

```text
Événement terminé
       ↓
Paiements pris en compte
       ↓
Remboursements
       ↓
Frais / commissions
       ↓
Calcul du montant net
       ↓
Solde disponible
```

---

## 29. Journey J27 — Organisateur : retirer ses fonds

```text
Solde disponible
      ↓
Demande de retrait
      ↓
Montant ≤ solde
      ↓
Traitement
```

L'organisateur peut effectuer plusieurs retraits.

### Exemple

```text
Solde : 500 000 FCFA
       ↓
Retrait : 300 000 FCFA
       ↓
Solde : 200 000 FCFA
       ↓
Retrait ultérieur possible
```

---

## 30. Journey J28 — Organisateur : retrait échoué

```text
Demande de retrait
       ↓
Traitement
       ↓
ÉCHEC
       ↓
Montant restitué au solde
       ↓
Nouvelle tentative
```

Le retrait échoué reste enregistré pour assurer la traçabilité.

---

## 31. Journey J29 — Organisateur : fraude confirmée

```text
Fraude confirmée
      ↓
Organisation bannie
      ↓
Nouvelles activités bloquées
      ↓
Événements existants évalués
```

Un événement existant n'est pas automatiquement supprimé sans analyse.

Selon la situation :

```text
Événement existant
       ↓
┌──────┼─────────┐
↓      ↓         ↓
Maintien Suspension Annulation
```

Une annulation peut ensuite entraîner le remboursement des participants.

---

## 32. Journey J30 — Organisateur : tentative de contournement

```text
Nouvelle organisation
       ↓
Analyse des signaux
       ↓
┌──────────────┬──────────────────┐
↓              ↓                  ↓
Aucun lien   Lien suspect      Lien confirmé
↓              ↓                  ↓
Activité     Vérification      Blocage
normale      renforcée
```

Une simple ressemblance ne suffit pas automatiquement à établir une fraude.

---

## 33. Journey J31 — Agent de contrôle : début de mission

```text
Événement
    ↓
Agent autorisé
    ↓
Accès au contrôle
    ↓
Scanner prêt
```

L'agent doit savoir quel événement il contrôle.

---

## 34. Journey J32 — Agent : contrôle normal

```text
Scanner
   ↓
QR Code
   ↓
Vérification
   ↓
Résultat
```

### Valide

```text
VALID
 ↓
Accès
 ↓
USED
```

### Invalide

```text
INVALID
 ↓
Accès refusé
```

---

## 35. Journey J33 — Agent : deux scanners

Lorsque plusieurs scanners sont utilisés :

```text
Scanner A ─┐
           ├── État partagé fiable
Scanner B ─┘
```

Les deux peuvent contrôler simultanément.

L'objectif est d'éviter qu'un même billet soit accepté deux fois.

---

## 36. Journey J34 — Agent : perte de synchronisation

Si Eventix ne peut plus garantir la cohérence de l'état partagé :

```text
Perte de synchronisation
          ↓
Mode dégradé
          ↓
Un seul scanner actif
```

Les autres scanners ne doivent pas continuer à valider indépendamment des billets.

### Trade-off accepté

```text
Débit ↓
Fiabilité ↑
```

> Eventix accepte de sacrifier de la rapidité afin de préserver la fiabilité du contrôle.

---

## 37. Journey J35 — Eventix : détection d'un événement suspect

```text
Signalement
     ↓
Analyse
     ↓
Évaluation
     ↓
┌─────────────┬──────────────────┐
↓             ↓                  ↓
Normal       Suspect            Fraude
↓             ↓                  ↓
Aucune       Vérification       Sanction
action       renforcée
```

---

## 38. Journey J36 — Eventix : fraude confirmée

```text
Fraude confirmée
      ↓
Bannissement organisation
      ↓
Blocage nouvelles activités
      ↓
Analyse événements existants
      ↓
Mesures adaptées
```

Eventix doit empêcher autant que possible qu'une organisation frauduleuse poursuive simplement son activité sous une autre identité.

---

## 39. Journey J37 — Vue globale du participant

```text
              DÉCOUVRIR
                  ↓
              CONSULTER
                  ↓
          CHOISIR UN BILLET
                  ↓
              RÉSERVER
                  ↓
                PAYER
                  ↓
           OBTENIR BILLET
                  ↓
              PRÉPARER
                  ↓
             SE RENDRE
                  ↓
                SCANNER
                  ↓
               ACCÉDER
                  ↓
              PARTICIPER
```

---

## 40. Vue globale de l'organisateur

```text
             COMPTE
                ↓
        CRÉER ÉVÉNEMENT
                ↓
           CONFIGURER
                ↓
            SOUMETTRE
                ↓
          VÉRIFICATION
                ↓
         ┌──────┴──────┐
         ↓             ↓
      VALIDÉ          REFUS
         ↓
      PUBLIER
         ↓
       VENDRE
         ↓
       GÉRER
         ↓
      TERMINER
         ↓
       CLÔTURER
         ↓
    SOLDE DISPONIBLE
         ↓
       RETRAIT
```

---

## 41. Moments critiques du parcours

### Participant

Les moments les plus sensibles sont :

#### Avant l'achat

```text
Crédibilité
    ↓
Confiance
    ↓
Achat
```

#### Pendant le paiement

Le participant doit savoir :

* si le paiement est réussi ;
* si la réservation est encore active ;
* si le billet sera délivré.

#### Après paiement

Le participant doit être certain d'avoir obtenu un billet valide.

#### À l'entrée

La validation doit être rapide et compréhensible.

#### En cas de problème

Le participant doit comprendre la raison :

* paiement échoué ;
* réservation expirée ;
* billet invalide ;
* billet déjà utilisé ;
* événement annulé.

### Organisateur

Les moments les plus sensibles sont :

* vérification ;
* publication ;
* suivi des ventes ;
* calcul des revenus ;
* retrait des fonds.

Le moment financier critique est :

```text
Événement terminé
       ↓
Clôture
       ↓
Montant net
       ↓
Retrait
```

---

## 42. Principes UX déduits des journeys

### Transparence

L'utilisateur doit connaître l'état actuel de son opération.

### Prévisibilité

Une réservation, un paiement, un billet ou un remboursement doivent avoir un état compréhensible.

### Continuité

Une erreur technique ne doit pas obliger l'utilisateur à recommencer une opération déjà réussie.

### Confiance

La vérification des organisateurs doit contribuer directement à réduire le risque d'arnaque.

### Fiabilité

Le contrôle d'un billet doit privilégier la certitude de la décision plutôt que le simple débit maximal.

---

## 43. MVP vs fonctionnalités futures

### MVP

* Découverte d'événements
* Consultation
* Réservation
* Paiement
* Obtention du billet
* Email
* Téléchargement
* Contrôle QR Code
* Annulation
* Report
* Remboursement
* Signalement
* Vérification
* Bannissement
* Clôture financière
* Retrait organisateur

### Fonctionnalités futures

* Vente physique
* Points de vente
* Agents vendeurs
* WhatsApp
* Marketplace
* Transfert / revente de billets
* Quiz live
* Reels
* Photos / vidéos cloud
* Fonctionnalités sociales avancées
* Contrôle offline distribué avancé

---

## 44. Questions révélées par les journeys

Les User Journeys révèlent plusieurs questions qui ne doivent pas être inventées.

Elles seront centralisées dans :

`questions-metier-ouvertes.md`

### Exemples

* Comment exactement l'organisateur est-il vérifié ?
* Quelles preuves doit-il fournir ?
* Quels moyens de paiement sont disponibles dans le MVP ?
* Comment Eventix communique-t-il une annulation ?
* Quel est le délai de remboursement ?
* Qui décide qu'une fraude est confirmée ?
* Quels critères déclenchent une vérification renforcée ?
* Quelle politique exacte de frais / commissions est appliquée ?
* Quelles sont les conditions exactes de retrait ?

Ces questions restent ouvertes tant qu'une décision métier explicite n'a pas été prise.

---

## 45. Synthèse

Le parcours central d'Eventix est :

```text
                    CONFIANCE
                       ↓
                 DÉCOUVERTE
                       ↓
                  RÉSERVATION
                       ↓
                    PAIEMENT
                       ↓
                  OBTENTION
                  DU BILLET
                       ↓
                    CONTRÔLE
                       ↓
                    ACCÈS
                       ↓
              FIN DE L'ÉVÉNEMENT
                       ↓
                  CLÔTURE
                       ↓
                    RETRAIT
```

Les mécanismes transversaux sont :

```text
        ┌──────────────────────────────┐
        │          CONFIANCE           │
        │ Vérification / Signalement   │
        │ / Fraude / Bannissement      │
        └────────────┬─────────────────┘
                     │
                     ↓
                  EVENTIX
                     ↑
                     │
        ┌────────────┴─────────────────┐
        │           FINANCE            │
        │ Paiement / Remboursement     │
        │ Clôture / Retrait            │
        └──────────────────────────────┘
```

> **Les User Journeys relient les processus métier à l'expérience réelle des acteurs d'Eventix.**
