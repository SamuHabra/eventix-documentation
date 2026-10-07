# Contraintes — Eventix

> **Phase :** 03 — Découverte du métier
> **Projet :** Eventix
> **Périmètre :** MVP — Billetterie
> **Marché :** Cameroun
> **Version :** 1.0
> **Statut :** Validé pour le MVP

---

# 1. Objectif

Ce document définit les contraintes auxquelles Eventix doit se conformer dans son fonctionnement métier.

Une contrainte limite les possibilités du système ou impose une condition qui doit être respectée.

Les contraintes décrites ici restent indépendantes des choix technologiques.

> Une contrainte métier doit rester valable même si l'architecture, les technologies ou l'implémentation évoluent.

Ce document ne définit donc pas :

* l'architecture technique ;
* les technologies utilisées ;
* les APIs ;
* le modèle de données ;
* l'infrastructure ;
* les mécanismes d'implémentation.

---

# 2. Principes de gestion des contraintes

Les contraintes Eventix respectent les principes suivants :

1. Une contrainte doit être liée au fonctionnement réel du métier.
2. Une contrainte ne doit pas dépendre d'une technologie particulière.
3. Une contrainte doit être distinguée d'une règle métier lorsque nécessaire.
4. Les contraintes doivent respecter le périmètre du MVP.
5. Une décision métier non validée ne doit pas être transformée en contrainte.
6. Les contraintes ne doivent pas imposer prématurément une solution technique.
7. Les contraintes critiques doivent rester traçables.
8. Les contraintes doivent préserver les invariants métier.
9. Les contraintes doivent rester cohérentes avec les règles métier.
10. Le principe de faible couplage documentaire doit être préservé.

---

# 3. Contraintes de périmètre

## C01 — Le MVP est limité à la billetterie

Le MVP Eventix est centré sur la billetterie.

Il couvre notamment :

* la création d'événements ;
* la configuration d'événements ;
* la publication ;
* la découverte d'événements ;
* l'obtention de billets ;
* le paiement ;
* la récupération des billets ;
* le contrôle des billets ;
* le suivi des participants ;
* les statistiques ;
* la gestion des remboursements ;
* la réconciliation financière ;
* l'administration et la sécurité nécessaires au MVP.

---

## C02 — Le marché initial est le Cameroun

Le périmètre métier initial d'Eventix est limité au marché camerounais.

Les contraintes réglementaires, financières, commerciales ou opérationnelles propres à d'autres marchés ne doivent pas être introduites dans le MVP sans décision explicite.

---

## C03 — Les fonctionnalités hors MVP restent hors périmètre

Les fonctionnalités suivantes ne doivent pas être considérées comme des contraintes ou capacités du MVP :

* vente physique ;
* agents de vente ;
* kiosques ;
* synchronisation des ventes physiques ;
* WhatsApp ;
* Marketplace ;
* revente de billets ;
* Quiz Live ;
* QR Cloud photos / vidéos ;
* Reels ;
* architectures offline avancées ;
* synchronisation distribuée avancée.

Elles pourront être étudiées ultérieurement lorsqu'elles entreront officiellement dans le périmètre produit.

---

# 4. Contraintes de billetterie

## C04 — Une disponibilité ne peut pas être attribuée deux fois

Une même disponibilité ne peut pas être attribuée simultanément à plusieurs participants.

Le principe est :

```text
Disponibilité
      ↓
Réservation
      ↓
Billet
      ↓
Un propriétaire actif
```

Cette contrainte constitue un invariant central de la billetterie.

---

## C05 — Un billet ne peut avoir qu'un seul propriétaire actif

Un même billet ne peut être attribué simultanément à deux participants.

```text
Billet
  ↓
Un propriétaire actif
```

Il est interdit d'aboutir à :

```text
Billet
 ├── Participant A
 └── Participant B
```

---

## C06 — Une réservation bloque temporairement une disponibilité

Lorsqu'une réservation valide est initiée, la disponibilité correspondante doit être temporairement protégée contre une autre attribution.

```text
Disponible
    ↓
PENDING
    ↓
Disponibilité temporairement bloquée
```

---

## C07 — Une réservation non finalisée doit pouvoir expirer

Une réservation `PENDING` non finalisée dans le délai défini par Eventix doit pouvoir passer à `EXPIRED`.

```text
PENDING
   ↓
Délai dépassé
   ↓
EXPIRED
   ↓
Disponibilité libérée
```

L'expiration ne doit pas être assimilée automatiquement à un échec du paiement.

---

## C08 — Réservation, paiement et billet restent distincts

Les cycles suivants doivent rester distincts :

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

L'état de l'un ne doit pas permettre de déduire automatiquement l'état des autres.

---

# 5. Contraintes de paiement

## C09 — Un paiement confirmé après expiration doit être traité

L'expiration d'une réservation ne permet pas d'ignorer automatiquement une confirmation de paiement reçue ultérieurement.

Le système doit déterminer si le billet peut encore être attribué.

```text
Paiement confirmé
        ↓
Réconciliation
        ↓
┌─────────────────┐
│ Billet disponible│ → Attribution
└─────────────────┘

┌─────────────────┐
│ Billet attribué │ → Remboursement si encaissé
└─────────────────┘
```

---

## C10 — Un paiement ne doit produire ses effets qu'une seule fois

Une confirmation répétée d'un même paiement ne doit pas :

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

## C11 — Le prix payé est conservé

Le prix applicable lors de la finalisation de l'achat constitue le prix payé par le participant.

Une modification ultérieure du prix ne doit pas réécrire le prix historiquement payé.

## Cette contrainte protège également l'historique des transactions.

## C12 — La confirmation du paiement ne dépend pas du règlement organisateur

Une fois le paiement confirmé et le billet correctement attribué, le participant doit pouvoir utiliser son billet sans attendre le règlement financier de l'organisateur.

```text
Paiement confirmé
       ↓
Billet émis
       ↓
Billet utilisable
```

---

## C13 — Le règlement organisateur est différé

Les fonds issus des ventes ne sont pas immédiatement disponibles pour retrait par l'organisateur.

Le cycle financier est distinct du cycle de paiement du participant.

```text
Paiement
   ↓
Fonds en attente
   ↓
Événement terminé
   ↓
Réconciliation
   ↓
Clôture financière
   ↓
Solde
   ↓
Retrait
```

---

# 6. Contraintes de prix et de configuration

## C14 — Une modification future ne détruit pas les billets existants

Une modification de la configuration d'un événement ne doit pas invalider ou supprimer automatiquement les billets déjà achetés.

Exemple :

```text
Catégorie VIP disponible
        ↓
Participant achète
        ↓
Organisateur retire VIP des ventes futures
        ↓
Billet existant conservé
```

---

## C15 — La capacité ne peut pas être inférieure aux billets attribués

La capacité d'un événement ne peut pas être réduite sous le nombre de billets déjà attribués.

Exemple :

```text
Capacité : 500
Billets attribués : 300

Nouvelle capacité : 200
        ↓
       REFUS
```

Une capacité de 300 ou plus peut être envisagée selon les autres contraintes de l'événement.

---

# 7. Contraintes d'accès

## C16 — Un billet valide ne peut être utilisé avec succès qu'une seule fois

Lorsqu'un billet valide est contrôlé avec succès, il devient `USED`.

Une nouvelle tentative de contrôle doit être refusée.

```text
Billet valide
      ↓
Contrôle accepté
      ↓
USED
      ↓
Nouvelle tentative
      ↓
Refus
```

---

## C17 — Billet et présence sont deux concepts distincts

La possession d'un billet valide ne signifie pas que le participant est présent à l'événement.

La présence est constatée lors du contrôle.

```text
Billet valide
     ≠
Participant présent
```

---

## C18 — La cohérence des contrôles est prioritaire sur le fonctionnement offline

Pour le MVP, Eventix privilégie la cohérence de l'inventaire et des contrôles.

Le principe retenu est :

```text
Connexion obligatoire
        ↓
Inventaire centralisé
        ↓
Pas de vente offline
```

La complexité distribuée doit être introduite ultérieurement uniquement si le besoin métier la justifie.

---

## C19 — Plusieurs scanners nécessitent un état partagé fiable

Plusieurs scanners peuvent fonctionner simultanément lorsque leur état est partagé et synchronisé de manière fiable.

Si cette synchronisation devient indisponible :

```text
Synchronisation non fiable
        ↓
Mode mono-scanner
        ↓
Un seul scanner valide
```

Les autres scanners doivent attendre le rétablissement d'une synchronisation fiable.

Les contrôles réalisés pendant le mode dégradé sont synchronisés lorsque la connectivité est rétablie.

---

# 8. Contraintes de sécurité et de confiance

Les contraintes de cette section portent principalement sur la confiance métier : analyse des signalements, proportionnalité des décisions et traçabilité des opérations. La cybersécurité du système Eventix (comptes, données, services et incidents) est cadrée séparément dans la [découverte cybersécurité de la phase 03](../03-decouverte-du-metier/cybersecurite-eventix/README.md) et formalisée par les exigences non fonctionnelles ; les deux périmètres se complètent sans se remplacer.

## C20 — Un signalement ne constitue pas une fraude confirmée

Un signalement doit être analysé avant toute mesure définitive.

```text
Signalement
     ↓
Analyse
     ↓
Évaluation
     ↓
Mesure adaptée
```

Une sanction définitive ne doit pas être déduite automatiquement du simple signalement.

---

## C21 — Les décisions sensibles doivent être proportionnées

Lorsqu'une situation présente un risque, la mesure appliquée doit être proportionnée au niveau de risque identifié.

Les décisions sensibles doivent être traçables.

---

## C22 — Les opérations critiques doivent être traçables

Les opérations importantes doivent pouvoir être reliées à leur contexte, leur acteur, leur événement et leur date.

```text
Opération critique
      ↓
Acteur
      +
Contexte
      +
Événement
      +
Date
```

---

# 9. Contraintes de remboursement

## C23 — Une annulation imputable à l'organisateur rend les billets éligibles au remboursement

Lorsqu'un événement est annulé en raison d'un problème imputable à l'organisateur, les billets concernés deviennent éligibles au remboursement selon le processus défini par Eventix.

---

## C24 — Les remboursements peuvent être progressifs

Lorsqu'un événement nécessite un grand nombre de remboursements, ceux-ci peuvent être traités progressivement jusqu'à leur finalisation.

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

## C25 — Un même paiement ne peut être remboursé plusieurs fois pour la même cause

Un même paiement ne doit jamais faire l'objet de plusieurs remboursements pour une même cause.

Les remboursements doivent rester traçables.

Les informations concernées comprennent notamment :

* paiement ;
* montant ;
* motif ;
* événement ;
* participant ;
* statut du remboursement.

---

# 10. Contraintes de report

## C26 — Un billet est conservé lors d'un report

Lorsqu'un événement est reporté, les billets déjà achetés restent valables et sont transférés vers la nouvelle date.

```text
Date initiale
     ↓
Report
     ↓
Nouvelle date

Billet existant
     ↓
Toujours valide
```

Le simple report d'un événement ne déclenche pas automatiquement un remboursement.

---

# 11. Contraintes d'historique

## C27 — L'historique métier doit être préservé

Une modification future ne doit pas réécrire l'historique d'une transaction ou d'un billet déjà attribué.

Cela signifie notamment qu'une modification ultérieure d'une configuration ne doit pas transformer rétroactivement les informations historiques d'une vente.

---

# 12. Contraintes documentaires

## C28 — Les contraintes ne doivent pas devenir des exigences techniques

Une contrainte métier doit exprimer une limitation ou un invariant métier.

Elle ne doit pas imposer prématurément :

```text
❌ framework
❌ langage
❌ base de données
❌ API
❌ protocole
❌ architecture
❌ fournisseur cloud
```

Ces décisions appartiennent aux phases de conception, d'architecture et d'infrastructure.

---

## C29 — Une contrainte ne doit pas transformer une question ouverte en décision

Lorsqu'une contrainte n'est pas suffisamment définie par les sources métier, elle doit rester explicitement ouverte.

```text
Information manquante
       ↓
Question ouverte
       ↓
Décision métier
       ↓
Contrainte validée
```

Et non :

```text
Information manquante
       ↓
Hypothèse
       ↓
❌ Contrainte officielle
```

---

## C30 — Les contraintes doivent rester faiblement couplées

Les contraintes métier doivent pouvoir être réutilisées par :

* les besoins métier ;
* les User Stories ;
* les Use Cases ;
* les critères d'acceptation ;
* la conception ;
* l'architecture.

Mais aucun de ces documents ne doit absorber la responsabilité des autres.

La séparation documentaire reste :

```text
Besoins métier
    ↓
Pourquoi ?

Contraintes
    ↓
Quelles limites ?

Règles métier
    ↓
Quels invariants ?

User Stories
    ↓
Quelle valeur ?

Use Cases
    ↓
Quel comportement ?

Critères d'acceptation
    ↓
Comment vérifier ?

Architecture
    ↓
Comment construire ?
```

---

# 13. Invariants fondamentaux

Les contraintes Eventix peuvent être résumées par les invariants suivants :

### I01 — Unicité de disponibilité

```text
Une disponibilité
      ↓
Une réservation
      ↓
Un billet
      ↓
Un propriétaire actif
```

### I02 — Séparation des cycles

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

### I03 — Préservation de l'historique

```text
Modification future
        ↓
Ne réécrit pas
        ↓
L'historique passé
```

### I04 — Traçabilité

```text
Opération critique
        ↓
Contexte + acteur + événement + date
```

### I05 — Cohérence prioritaire

```text
MVP
 ↓
Connexion obligatoire
 ↓
Inventaire centralisé
 ↓
Pas de vente offline
```

Ces principes constituent les contraintes structurantes du fonctionnement métier d'Eventix.

---

# 14. Hors périmètre

Les contraintes relatives aux domaines suivants ne sont pas définies dans le présent document pour le MVP :

* Marketplace ;
* revente de billets ;
* vente physique ;
* kiosques ;
* agents de vente ;
* Quiz Live ;
* QR Cloud ;
* Reels ;
* fonctionnement offline avancé ;
* synchronisation distribuée avancée.

Ces domaines feront l'objet d'une analyse spécifique avant leur intégration.

---

# 15. Relation avec les autres artefacts

Les contraintes sont utilisées comme source de cohérence par les autres documents.

```text
contraintes.md
      │
      ├──→ regles-metier.md
      │
      ├──→ besoins-metier.md
      │
      ├──→ user-stories.md
      │
      ├──→ use-cases.md
      │
      └──→ criteres-d-acceptation.md
```

La relation complète entre les artefacts est conservée dans :

```text
matrice-de-tracabilite.md
```

Aucun document ne doit devenir le conteneur de tous les autres.

---

# 16. Règle de maintenance

Une contrainte ne doit être modifiée que lorsqu'une décision métier validée la remet en cause.

Toute modification doit être évaluée sur :

* les règles métier concernées ;
* les besoins métier concernés ;
* les User Stories concernées ;
* les Use Cases concernés ;
* les critères d'acceptation concernés ;
* la traçabilité ;
* le périmètre MVP.

Une modification technique seule ne doit pas modifier automatiquement une contrainte métier.

---

# 17. Statut du document

**Document :** `contraintes.md`
**Version :** 1.0
**Statut :** Validé pour le MVP
**Périmètre :** Eventix — Billetterie
**Marché initial :** Cameroun
**Principe transversal :** Faible couplage
**Source normative associée :** `regles-metier.md`

Ce document constitue la référence des contraintes métier actuellement retenues pour le MVP Eventix.

Toute nouvelle contrainte ou modification d'une contrainte existante doit être discutée, justifiée et validée par l'équipe avant d'être considérée comme normative.
