# Besoins métier — Eventix

**Version :** 1.0  
**Statut :** V1 — À valider  
**Périmètre :** Eventix MVP — Billetterie  
**Marché :** Cameroun

---

## 1. Objectif du document

Ce document définit les besoins métier auxquels Eventix doit répondre dans son MVP.

Il décrit **ce que le système doit permettre de faire**, sans définir la manière technique de le réaliser.

Les besoins des futurs domaines tels que **Quiz Live** et **Marketplace** seront étudiés séparément lorsqu'ils entreront dans le périmètre produit.

---

# 2. Besoins liés à l'organisateur

## B01 — Gérer un événement

Eventix doit permettre à l'organisateur de :

- créer un événement ;
- le configurer ;
- le publier ;
- suivre son évolution.

---

## B02 — Configurer un événement

Eventix doit permettre à l'organisateur de définir les caractéristiques nécessaires à son événement, notamment :

- informations générales ;
- date ;
- lieu ;
- configuration des espaces ;
- capacité ;
- catégories de billets ;
- prix ;
- périodes de vente ;
- paramètres nécessaires à la billetterie.

---

## B03 — Publier un événement

Eventix doit permettre à l'organisateur de publier un événement afin de le rendre disponible aux participants selon les conditions définies.

Un événement peut notamment passer par différents états tels que :

```text
Brouillon
   ↓
Publié
   ↓
En vente
   ↓
Terminé
```

Les états d'annulation et de report seront définis dans les règles métier.

---

## B04 — Consulter et gérer ses événements

Eventix doit permettre à l'organisateur de :

- consulter ses événements ;
- rechercher un événement ;
- filtrer ses événements ;
- identifier leur état ;
- accéder aux informations et statistiques associées.

---

# 3. Besoins liés à la participation

## B05 — Découvrir un événement

Eventix doit permettre au participant de :

- rechercher des événements ;
- consulter les événements disponibles ;
- accéder aux informations détaillées d'un événement.

---

## B06 — Obtenir un billet

Eventix doit permettre au participant d'obtenir un billet pour un événement gratuit ou payant.

### Événement gratuit

```text
Inscription
    ↓
Émission du billet
```

### Événement payant

```text
Achat
   ↓
Paiement
   ↓
Émission du billet
```

---

## B07 — Récupérer son billet

Pour le MVP, Eventix doit permettre au participant de récupérer son billet :

- depuis la plateforme ;
- par téléchargement ;
- par email.

L'envoi par WhatsApp est considéré comme une évolution future.

---

# 4. Besoins liés à la configuration des espaces

## B08 — Configurer les espaces

Eventix doit permettre à l'organisateur de configurer la structure de son événement :

- espaces ;
- zones ;
- capacités ;
- places ;
- places numérotées ;
- places non numérotées.

---

## B09 — Associer les billets aux emplacements

Eventix doit permettre d'associer les catégories de billets aux espaces, zones ou places correspondants.

Exemple :

```text
Salle
├── VIP
│   ├── A1
│   ├── A2
│   └── A3
│
├── Standard
│   └── Capacité définie
│
└── Fosse
    └── Capacité définie
```

---

# 5. Besoins liés à la vente

## B10 — Centraliser les ventes

Eventix doit permettre à l'organisateur de centraliser les ventes réalisées par différents canaux.

Pour le MVP :

```text
Vente en ligne
      +
Vente physique
      ↓
Inventaire centralisé
```

---

## B11 — Gérer les points physiques

Eventix doit permettre de gérer un réseau de points physiques partenaires.

Un point physique est un partenaire d'Eventix avec lequel un accord de partenariat existe.

Un point physique peut être autorisé à vendre des billets pour certains événements.

> Être partenaire d'Eventix ne signifie pas être automatiquement autorisé à vendre tous les événements.

---

## B12 — Gérer les agents des points physiques

Eventix doit permettre aux points physiques de disposer d'agents chargés de réaliser les ventes.

Chaque opération doit pouvoir être associée au contexte de vente correspondant.

Exemple :

```text
Point physique
      ↓
Agent
      ↓
Vente
      ↓
Événement
      ↓
Billet
```

---

## B13 — Suivre les ventes

Eventix doit permettre à l'organisateur de suivre notamment :

- le nombre de billets vendus ;
- les billets restants ;
- les ventes en ligne ;
- les ventes physiques ;
- les ventes par point physique ;
- les ventes par catégorie ;
- les ventes par période ;
- les recettes.

Le contexte de chaque vente doit être identifiable.

Une vente peut notamment distinguer :

```text
Acheteur
Participant
Canal de vente
Point physique
Agent
Événement
Billet
```

---

# 6. Besoins liés à la réservation et au paiement

## B14 — Gérer les réservations temporaires

Eventix doit permettre de réserver temporairement une disponibilité pendant le processus d'achat.

Une réservation non finalisée doit pouvoir expirer afin de libérer la disponibilité.

---

## B15 — Séparer réservation, paiement et billet

Eventix doit traiter séparément :

```text
Réservation
Paiement
Billet
```

L'expiration d'une réservation ne signifie pas automatiquement que le paiement a échoué.

---

## B16 — Réconcilier les paiements

Eventix doit pouvoir traiter les situations dans lesquelles la confirmation du paiement arrive après l'expiration d'une réservation.

Le système doit déterminer si :

- le billet peut encore être attribué ;
- une autre attribution a déjà eu lieu ;
- un remboursement doit être déclenché.

Les confirmations répétées d'une même transaction ne doivent pas provoquer plusieurs traitements.

---

# 7. Besoins liés aux remboursements

## B17 — Gérer les remboursements

Eventix doit permettre de gérer les remboursements lorsque les conditions prévues par la politique applicable sont réunies.

Les remboursements doivent pouvoir être traités automatiquement lorsque leur volume le nécessite.

L'intervention humaine doit être principalement réservée aux exceptions.

---

## B18 — Gérer l'annulation d'un événement

Lorsqu'un événement est annulé, Eventix doit pouvoir gérer les conséquences sur :

- les billets ;
- les paiements ;
- les remboursements ;
- les participants ;
- les ventes ;
- le règlement de l'organisateur.

---

## B19 — Gérer le report d'un événement

Lorsqu'un événement est reporté :

> les billets déjà achetés sont conservés et transférés vers la nouvelle date lorsque les conditions le permettent.

Si la nouvelle configuration est incompatible avec les billets existants, une procédure spécifique doit être appliquée.

Un participant doit pouvoir conserver son billet ou demander un remboursement lorsque les conditions applicables le permettent.

---

# 8. Besoins liés aux prix

## B20 — Gérer les prix

Eventix doit permettre à l'organisateur de définir et modifier les prix applicables aux futures ventes.

Une modification de prix ne doit pas modifier le prix historiquement payé pour les billets déjà achetés.

Exemple :

```text
Ancien prix : 10 000 FCFA
      ↓
Jean achète
      ↓
Prix payé = 10 000 FCFA
      ↓
Nouveau prix : 15 000 FCFA
      ↓
Jean reste à 10 000 FCFA
```

---

# 9. Besoins liés aux billets

## B21 — Contrôler la validité des billets

Eventix doit permettre de vérifier la validité d'un billet lors de l'accès à l'événement.

Le contrôle doit notamment permettre d'identifier les billets qui ne peuvent pas être utilisés.

---

## B22 — Empêcher la réutilisation d'un billet

Un billet validé à l'entrée ne doit pas pouvoir être utilisé une seconde fois comme un billet valide.

---

## B23 — Suivre les contrôles

Eventix doit enregistrer les opérations de contrôle afin de permettre notamment :

- le suivi des entrées ;
- les statistiques ;
- les audits ;
- l'analyse des incidents.

Le système doit notamment pouvoir identifier le contrôleur et le point d'entrée concerné.

---

## B24 — Affecter les contrôleurs

Eventix doit permettre d'affecter des contrôleurs à des points d'entrée.

Exemple :

```text
Concert
   ↓
Entrée A
   ↓
Olemme
```

Cette affectation permet notamment de savoir qui réalise les contrôles à cette entrée.

> L'affectation physique à une entrée ne constitue pas automatiquement une restriction sur les catégories de billets que le contrôleur peut valider.

---

# 10. Besoins liés aux participants

## B25 — Suivre les participants

Eventix doit permettre à l'organisateur de suivre les participants associés à son événement.

Le système doit distinguer notamment :

```text
Billet vendu
      ≠
Participant attendu
      ≠
Participant présent
```

---

## B26 — Suivre l'activité en temps réel

Eventix doit permettre à l'organisateur de suivre l'activité de son événement en temps réel, notamment :

- ventes ;
- entrées ;
- participants présents ;
- disponibilité ;
- activité des points physiques.

---

# 11. Besoins liés au transfert des billets

## B27 — Transférer un billet

Eventix doit permettre le transfert d'un billet lorsque les conditions de l'événement l'autorisent.

Le transfert doit :

- être traçable ;
- conserver l'historique ;
- respecter les éventuelles restrictions définies pour l'événement.

---

# 12. Besoins liés à la confiance et à la fraude

## B28 — Vérifier les organisateurs et les événements

Eventix doit mettre en place des mécanismes permettant de vérifier et contrôler :

- les organisateurs ;
- les organisations ;
- les événements ;
- certaines opérations sensibles.

L'objectif est de réduire les risques de faux événements et de fraude.

---

## B29 — Gérer le niveau de risque

Eventix doit pouvoir adapter ses actions au niveau de risque détecté.

```text
Risque faible
    ↓
Surveillance

Risque modéré
    ↓
Contrôles supplémentaires

Risque élevé
    ↓
Limitation / suspension

Risque critique
    ↓
Intervention humaine
```

Les décisions et actions sensibles doivent être traçables.

---

## B30 — Signaler un événement ou une activité suspecte

Eventix doit permettre aux utilisateurs de signaler un problème ou une activité suspecte.

Un signalement peut déclencher une analyse et une mesure adaptée au niveau de risque.

---

# 13. Besoins liés aux statistiques et au pilotage

## B31 — Analyser les performances

Eventix doit permettre à l'organisateur d'analyser les performances de son événement à différents niveaux.

Exemples :

- ventes globales ;
- ventes par canal ;
- ventes par point physique ;
- ventes par catégorie ;
- ventes par période ;
- taux de présence ;
- chiffre d'affaires ;
- remboursements ;
- performances des points de vente.

---

## B32 — Fournir des indicateurs d'aide à la décision

Eventix doit pouvoir mettre en évidence des tendances utiles à l'organisateur.

Exemples :

```text
Point Akwa
→ meilleur point physique

VIP
→ catégorie générant le plus de revenus

Standard
→ catégorie avec le plus grand volume de ventes

Derniers jours
→ accélération des ventes
```

À terme, Eventix pourra proposer des recommandations permettant d'améliorer les décisions commerciales et opérationnelles.

---

# 14. Besoins liés au règlement financier

## B33 — Gérer le règlement de l'organisateur

Eventix doit gérer le cycle :

```text
Paiement participant
        ↓
Fonds en attente
        ↓
Réconciliation
        ↓
Événement terminé
        ↓
Règlement organisateur
```

L'organisateur ne dispose pas automatiquement des fonds immédiatement après chaque vente.

Les conditions précises de règlement seront définies dans les règles financières et métier.

---

## B34 — Gérer les commissions des points physiques

La rémunération des points physiques dépend des conditions contractuelles applicables.

Eventix doit permettre de prendre en compte les différentes conventions de rémunération.

Exemple conceptuel :

```text
Vente
  ↓
Montant brut
  ├── Commission Eventix
  ├── Commission point physique
  └── Montant organisateur
```

Les règles financières précises seront définies ultérieurement.

---

# 15. Besoins hors MVP

Les fonctionnalités suivantes sont envisagées pour l'évolution de la plateforme mais ne font pas partie du périmètre fonctionnel principal du MVP actuel :

```text
Quiz Live
Marketplace
QR Cloud photos / vidéos
Reels
Autres services événementiels
```

Ces domaines feront l'objet d'une analyse métier propre lorsqu'ils seront intégrés au périmètre produit.

---

# 16. Principes métier transverses

Les besoins précédents reposent notamment sur les principes suivants :

### P01 — Source de vérité unique

Les différents canaux de vente doivent partager une vision cohérente de la disponibilité.

### P02 — Historique préservé

Une transaction réalisée ne doit pas être réécrite simplement parce que les paramètres futurs de l'événement changent.

### P03 — Séparation des cycles

```text
Réservation
≠
Paiement
≠
Billet
≠
Présence
```

### P04 — Traçabilité

Les opérations importantes doivent pouvoir être associées à leur contexte et à leur auteur.

### P05 — Contrôle du risque

Les opérations sensibles doivent pouvoir être surveillées, limitées ou suspendues selon le niveau de risque.

### P06 — Automatisation progressive

Les opérations susceptibles de devenir volumineuses doivent pouvoir être automatisées plutôt que dépendre d'un traitement manuel.

---

# 17. Périmètre fonctionnel actuel

```text
EVENTIX MVP
│
├── Gestion des événements
├── Gestion des espaces et places
├── Billetterie
├── Vente en ligne
├── Points physiques partenaires
├── Paiements
├── Réservations
├── Remboursements
├── Contrôle d'accès
├── Gestion des participants
├── Gestion des équipes
├── Statistiques
├── Gestion du risque
└── Règlement financier
```

---

# 18. Statut

**Version : 1.0**

**État : À valider par l'équipe**

Ce document constitue une base de travail pour les phases suivantes.

Les règles métier détaillées, les acteurs, les cas d'utilisation, les scénarios nominaux et exceptionnels seront développés dans les documents correspondants.