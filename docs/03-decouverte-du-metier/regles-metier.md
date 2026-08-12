# Règles métier — Eventix

**Version :** 1.0
**Statut :** Validé — MVP
**Marché :** Cameroun
**Dernière mise à jour :** 2026-08-10

---

## 1. Objectif

Ce document définit les règles métier qui doivent être respectées par Eventix.

Contrairement aux besoins métier, ces règles définissent les **contraintes et invariants** du fonctionnement de la plateforme.

> Une règle métier doit rester vraie indépendamment de la technologie utilisée.

---

# 2. Billetterie et disponibilité

## RM01 — Une réservation bloque temporairement une disponibilité

Lorsqu'un participant ou un agent initie une réservation valide, la disponibilité correspondante est temporairement bloquée.

```text
Disponible
    ↓
Réservation PENDING
    ↓
Disponibilité temporairement bloquée
```

---

## RM02 — Une réservation expire après son délai

Une réservation `PENDING` qui n'est pas finalisée dans le délai défini par Eventix passe à `EXPIRED`.

```text
PENDING
   ↓
Délai dépassé
   ↓
EXPIRED
   ↓
Disponibilité libérée
```

L'expiration d'une réservation ne signifie pas automatiquement que le paiement a échoué.

---

## RM03 — Réservation, paiement et billet sont distincts

Eventix doit traiter séparément :

```text
Réservation
     ≠
Paiement
     ≠
Billet
```

L'état de l'un ne doit pas être utilisé pour déduire automatiquement l'état des autres.

---

## RM04 — Un paiement confirmé après expiration doit être réconcilié

Si un paiement est confirmé après l'expiration d'une réservation, Eventix doit déterminer si le billet peut encore être attribué.

### Si le billet est disponible

```text
Paiement confirmé
       ↓
Billet attribué
```

### Si le billet a déjà été attribué

```text
Paiement confirmé
       ↓
Billet indisponible
       ↓
Remboursement si paiement encaissé
```

Un paiement confirmé ne doit donc jamais être ignoré uniquement parce que la réservation a expiré.

---

## RM05 — Un paiement ne peut produire ses effets qu'une seule fois

Les notifications ou confirmations répétées d'un même paiement ne doivent pas :

* créer plusieurs billets ;
* enregistrer plusieurs ventes ;
* déclencher plusieurs remboursements.

```text
PAYMENT_SUCCESS
PAYMENT_SUCCESS
PAYMENT_SUCCESS
        ↓
   Une seule opération métier
```

---

## RM06 — Un billet ne peut être attribué à deux participants simultanément

Un même billet ne peut avoir qu'un seul propriétaire actif.

```text
Billet EVX-123
      ↓
Jean
```

et jamais :

```text
Billet EVX-123
   ├── Jean
   └── Marie
```

---

# 3. Prix et ventes

## RM07 — Le prix payé est définitif

Le prix d'un billet est celui qui était applicable au moment de la finalisation de l'achat.

Une modification ultérieure du prix ne modifie pas le montant déjà payé.

```text
Jean → 10 000 FCFA
Prix futur → 15 000 FCFA

Jean reste à 10 000 FCFA.
```

Une baisse ultérieure du prix ne donne pas automatiquement droit à un remboursement de la différence.

---

## RM08 — Les ventes physiques et en ligne utilisent une disponibilité centralisée

Pour le MVP, les ventes réalisées :

* en ligne ;
* dans les points physiques ;

doivent utiliser le même inventaire Eventix.

```text
             Eventix
                │
       ┌────────┴────────┐
       ↓                 ↓
    En ligne        Point physique
       │                 │
       └────────┬────────┘
                ↓
       Inventaire central
```

Un point physique ne possède donc pas un stock indépendant dans le MVP.

---

## RM09 — Une vente physique nécessite une connexion Eventix

Pour le MVP, un point physique doit être connecté à Eventix pour réaliser une nouvelle vente.

En cas de perte de connexion :

```text
Connexion disponible → Vente autorisée
Connexion absente    → Vente impossible
```

Le mode hors ligne est une évolution future.

---

## RM10 — Une vente physique peut être réglée en espèces

Une vente réalisée dans un point physique peut utiliser le mode de paiement `CASH`.

L'agent autorisé confirme la réception du montant.

Eventix doit conserver au minimum :

* le point physique ;
* l'agent ;
* l'événement ;
* le billet ;
* le montant ;
* le mode de paiement ;
* le contexte de la vente.

---

## RM11 — Une vente physique doit être enregistrée dans Eventix

Le billet vendu dans un point physique doit être généré à partir d'Eventix.

Le point physique ne doit pas créer indépendamment un billet qui n'existe pas dans le système central.

---

# 4. Billets gratuits

## RM12 — Un billet gratuit consomme une disponibilité

Lorsqu'un participant obtient un billet gratuit, la disponibilité correspondante est consommée.

L'absence ultérieure du participant ne libère pas automatiquement la place.

```text
Capacité = 500
Billets gratuits attribués = 500
        ↓
Plus aucune disponibilité
```

La liste d'attente et la libération automatique des places sont des évolutions futures.

---

# 5. Contrôle des billets

## RM13 — Un billet déjà utilisé est refusé

Lorsqu'un billet a déjà été validé pour l'événement, toute nouvelle tentative d'utilisation doit être refusée.

```text
Billet valide
   ↓
Déjà utilisé ?
   ↓
Oui → Accès refusé
```

---

## RM14 — Un billet ne peut être validé qu'une seule fois

Pour un même événement, une seule tentative de validation peut réussir.

Si deux contrôleurs tentent de valider le même billet simultanément :

```text
Billet
 ├── Tentative A
 └── Tentative B
        ↓
Une seule validation acceptée
        ↓
Toutes les autres refusées
```

La manière technique de garantir cette règle sera définie lors de la conception du système.

---

## RM15 — Une affectation physique n'est pas automatiquement une restriction de billet

L'affectation d'un contrôleur à une entrée sert notamment à identifier le contexte du contrôle.

Exemple :

```text
Olemme
   ↓
Entrée A
```

Cela ne signifie pas automatiquement :

> « Olemme ne peut contrôler que les billets Standard. »

Un contrôleur peut valider un billet valide pour l'événement selon les autorisations qui lui sont réellement attribuées.

---

## RM16 — Les contrôles doivent être traçables

Chaque contrôle important doit pouvoir être associé à son contexte.

Le système doit notamment pouvoir identifier :

* le billet ;
* l'événement ;
* le contrôleur ;
* le point d'entrée ;
* la date et l'heure ;
* le résultat du contrôle.

---

# 6. Participants

## RM17 — Billet et présence sont deux informations différentes

L'existence d'un billet valide ne signifie pas que le participant est présent.

```text
Billet vendu
     ≠
Participant présent
```

La présence est constatée lors du contrôle d'accès.

---

# 7. Remboursements

## RM18 — Le changement d'avis du participant ne déclenche pas automatiquement un remboursement

Si l'événement est maintenu et que le participant :

* change d'avis ;
* ne souhaite plus venir ;
* ne se présente pas ;

aucun remboursement automatique n'est effectué dans le MVP.

---

## RM19 — L'annulation imputable à l'organisateur rend les billets éligibles au remboursement

Lorsqu'un événement est annulé en raison d'un problème imputable à l'organisateur, les billets concernés deviennent éligibles au remboursement selon le processus défini par Eventix.

---

## RM20 — Les remboursements peuvent être traités progressivement

Lorsqu'un événement nécessite un grand nombre de remboursements, Eventix peut les traiter progressivement jusqu'à leur finalisation.

```text
Annulation
   ↓
Billets concernés
   ↓
Éligibilité
   ↓
Remboursements progressifs
   ↓
Finalisation
```

---

## RM21 — Un remboursement ne peut être exécuté plusieurs fois

Un même paiement ne doit jamais faire l'objet de plusieurs remboursements pour une même cause.

Les remboursements doivent être traçables.

Eventix doit pouvoir identifier :

* le paiement concerné ;
* le montant ;
* le motif ;
* l'événement ;
* le participant ;
* le statut du remboursement.

---

# 8. Report d'événement

## RM22 — Un billet est conservé lors d'un report

Lorsqu'un événement est reporté, les billets déjà achetés restent valables et sont transférés vers la nouvelle date.

```text
10 octobre
   ↓
Report
   ↓
17 octobre

Billet existant
   ↓
Toujours valide
```

Le simple report d'un événement ne déclenche pas automatiquement un remboursement.

---

# 9. Configuration des événements

## RM23 — Une modification future ne détruit pas les billets existants

Une modification de la configuration d'un événement ne doit pas invalider ou supprimer automatiquement les billets déjà achetés.

Exemple :

```text
VIP disponible
     ↓
Jean achète
     ↓
Organisateur retire VIP des ventes futures
     ↓
Jean conserve son billet VIP
```

---

## RM24 — La capacité ne peut pas être inférieure aux billets déjà attribués

Une capacité ne peut pas être réduite sous le nombre de billets déjà attribués.

```text
Capacité : 500
Billets attribués : 300

Nouvelle capacité : 200
        ↓
       REFUS
```

Une nouvelle capacité de 300 ou plus peut être envisagée selon les autres contraintes de l'événement.

---

# 10. Paiement et règlement de l'organisateur

## RM25 — La confirmation du paiement rend le billet utilisable

Lorsqu'un paiement est confirmé et que le billet est correctement attribué, le participant peut utiliser son billet sans attendre le règlement financier de l'organisateur.

```text
Paiement confirmé
       ↓
Billet émis
       ↓
Billet utilisable
```

---

## RM26 — Le règlement de l'organisateur est différé

Les fonds issus des ventes ne sont pas immédiatement disponibles pour retrait par l'organisateur.

Le règlement intervient après la fin de l'événement, sous réserve des opérations nécessaires de réconciliation.

```text
Paiement
   ↓
Fonds en attente
   ↓
Événement terminé
   ↓
Réconciliation
   ↓
Settlement
   ↓
Organisateur
```

Ainsi :

```text
Paiement confirmé
       ≠
Règlement organisateur
```

---

# 11. Transfert de billets

## RM27 — Un billet peut changer de propriétaire

Lorsqu'un transfert est autorisé, le billet change de propriétaire sans qu'une copie du billet soit créée.

```text
Jean
 ↓
Billet EVX-123
 ↓
Transfert
 ↓
Marie
```

Le billet conserve :

* son identifiant ;
* son historique ;
* son contexte d'événement ;
* son historique de propriété.

Un billet ne peut avoir qu'un seul propriétaire actif.

---

# 12. Confiance et lutte contre la fraude

## RM28 — Eventix doit contrôler les organisateurs et les événements

Eventix doit appliquer des mécanismes de vérification permettant de réduire les risques liés :

* aux faux organisateurs ;
* aux faux événements ;
* aux comportements frauduleux ;
* aux opérations financières suspectes.

La vérification ne constitue pas une garantie absolue d'absence de fraude.

---

## RM29 — Le niveau de risque détermine la réponse

Eventix doit adapter ses mesures au niveau de risque.

```text
Faible
  ↓
Surveillance

Modéré
  ↓
Contrôles supplémentaires

Élevé
  ↓
Limitation / suspension

Critique
  ↓
Intervention humaine
```

---

## RM30 — Les décisions sensibles doivent être traçables

Les actions importantes liées à la sécurité, à la fraude, aux suspensions et aux restrictions doivent être enregistrées afin de pouvoir être auditées.

---

# 13. Statistiques et données

## RM31 — Les données historiques doivent être conservées

Les opérations importantes doivent conserver leur historique afin de permettre :

* les statistiques ;
* les audits ;
* la réconciliation ;
* la résolution des litiges ;
* l'analyse des performances.

---

## RM32 — Les ventes doivent être analysables par contexte

Eventix doit pouvoir distinguer les ventes notamment selon :

* canal ;
* point physique ;
* agent ;
* catégorie ;
* période ;
* événement.

---

## RM33 — Les statistiques ne doivent pas modifier les données métier

Les statistiques et analyses sont produites à partir des données métier.

Une statistique ou une recommandation ne doit pas modifier directement une vente, un paiement ou un billet.

---

# 14. Évolutions futures

Les règles suivantes ne font **pas partie du MVP**.

## RM-F01 — Mode hors ligne des points physiques

Eventix pourra ultérieurement permettre aux points physiques de fonctionner temporairement hors connexion.

Cette évolution devra résoudre notamment :

* la synchronisation ;
* la concurrence ;
* les conflits ;
* le risque de double vente ;
* la réconciliation.

---

## RM-F02 — Marketplace de revente après sold-out

Eventix pourra ultérieurement proposer une Marketplace permettant aux détenteurs de billets de revendre leurs billets.

La Marketplace sera activée **après l'épuisement des billets officiels (`SOLD OUT`)**, selon les règles qui seront définies lors de la conception de ce domaine.

Principes envisagés :

```text
Billets officiels disponibles
        ↓
Vente Eventix classique
        ↓
SOLD OUT
        ↓
Marketplace
        ↓
Revente entre participants
```

La Marketplace devra notamment garantir :

* un seul propriétaire actif par billet ;
* l'impossibilité de vendre deux fois le même billet ;
* le transfert sécurisé après confirmation de la transaction ;
* la traçabilité des reventes ;
* la protection de l'acheteur ;
* la gestion des commissions ;
* la gestion des prix ;
* la lutte contre la spéculation.

Les règles détaillées de la Marketplace feront l'objet d'un document métier spécifique.

---

## RM-F03 — Quiz Live

Le Quiz Live sera étudié comme une fonctionnalité ou un domaine métier distinct.

Ses règles seront définies lors de son intégration au périmètre produit.

---

## RM-F04 — Marketplace et autres services événementiels

Les futurs services tels que :

* Marketplace ;
* Quiz Live ;
* QR Cloud photos/vidéos ;
* Reels ;
* autres services événementiels ;

feront l'objet d'une analyse métier spécifique avant leur intégration.

---

# 15. Principes métier fondamentaux

Les règles Eventix reposent sur plusieurs principes structurants.

### P01 — Une disponibilité ne peut être attribuée deux fois

```text
Une disponibilité
        ↓
Une réservation
        ↓
Un billet
        ↓
Un propriétaire actif
```

---

### P02 — Les cycles métier sont séparés

```text
Réservation
    ≠
Paiement
    ≠
Billet
    ≠
Présence
    ≠
Settlement
```

---

### P03 — L'historique est préservé

Une modification future ne doit pas réécrire l'historique d'une transaction ou d'un billet déjà attribué.

---

### P04 — Les opérations critiques sont traçables

Les opérations importantes doivent pouvoir être reliées à leur contexte, leur acteur, leur événement et leur date.

---

### P05 — La vente officielle est prioritaire sur la revente

Tant que des billets officiels sont disponibles, la vente classique Eventix reste le canal principal.

La Marketplace de revente est une évolution future activée après `SOLD OUT`.

---

### P06 — Le MVP privilégie la cohérence

Pour le MVP :

```text
Connexion obligatoire
        ↓
Inventaire centralisé
        ↓
Pas de vente offline
```

La complexité distribuée sera introduite ultérieurement si le besoin métier le justifie.






Règle métier
Plusieurs scanners peuvent fonctionner lorsque leur état est partagé et synchronisé de manière fiable.
Si cette synchronisation devient indisponible, Eventix bascule vers un mode mono-scanner.
Un seul scanner est alors autorisé à effectuer les validations.
Les autres scanners doivent attendre le rétablissement d'une synchronisation fiable.
Les contrôles effectués pendant le mode dégradé sont synchronisés avec Eventix lorsque la connectivité est rétablie.
Pourquoi ce choix ?

Le trade-off est volontaire :

Mode	Rapidité	Fiabilité
Plusieurs scanners synchronisés	🟢 élevée	🟢 élevée
Un seul scanner	🟠 réduite	🟢 très élevée
Plusieurs scanners indépendants hors ligne	🟢 élevée	🔴 insuffisante

---

# 16. Statut du document

**Version :** 1.0
**Statut :** Validé pour le MVP
**Périmètre :** Billetterie Eventix
**Marché initial :** Cameroun

Ce document constitue la référence des règles métier actuellement validées.

Toute modification d'une règle existante ou ajout d'une nouvelle règle doit être discuté et validé par l'équipe selon le processus de décision du projet.
