# Glossaire métier — Eventix

**Version :** 1.0
**Statut :** Validé — MVP
**Marché initial :** Cameroun
**Dernière mise à jour :** 2026-08-10

---

## 1. Objectif

Ce document définit le vocabulaire métier officiel utilisé dans la conception d'Eventix.

L'objectif est d'éviter qu'un même terme soit utilisé avec plusieurs significations et de fournir un langage commun entre :

* l'équipe produit ;
* les métiers ;
* les développeurs ;
* les architectes ;
* les administrateurs ;
* les partenaires ;
* la documentation ;
* les futurs systèmes et services.

> **Le vocabulaire métier est une référence fonctionnelle. Il ne correspond pas nécessairement aux noms des tables, classes ou services techniques.**

---

# 2. Produit et plateforme

## Eventix

Plateforme de billetterie permettant notamment de créer, publier, découvrir, vendre et contrôler des billets pour des événements.

Dans le MVP, Eventix couvre principalement :

* la gestion des événements ;
* la billetterie ;
* les ventes en ligne ;
* les ventes physiques ;
* les paiements ;
* les contrôles d'accès ;
* les statistiques ;
* le règlement des organisateurs.

---

## Plateforme

Ensemble des services et interfaces constituant Eventix.

---

## MVP

**Minimum Viable Product.**

Première version d'Eventix contenant uniquement les fonctionnalités nécessaires au lancement.

---

## Participant

Personne qui souhaite participer ou participe à un événement.

Un participant peut notamment :

* rechercher un événement ;
* consulter ses informations ;
* obtenir un billet ;
* présenter son billet à l'entrée.

> Un participant n'est pas nécessairement l'acheteur du billet.

---

## Acheteur

Personne qui réalise une opération d'achat d'un billet.

L'acheteur peut acheter un billet pour lui-même ou pour une autre personne.

---

## Propriétaire du billet

Personne à laquelle le billet est actuellement attribué.

Le propriétaire peut être différent de l'acheteur.

Exemple :

```text
Jean achète un billet pour Marie.

Acheteur      → Jean
Propriétaire  → Marie
Participant   → Marie
```

---

# 3. Événement

## Événement

Activité organisée à laquelle des participants peuvent assister.

Un événement possède notamment :

* un nom ;
* une description ;
* une date ;
* un lieu ;
* un organisateur ;
* un statut ;
* un type de tarification ;
* des catégories de billets ;
* éventuellement des places numérotées.

---

## Organisateur

Acteur responsable de la création et de l'organisation d'un événement sur Eventix.

L'organisateur peut notamment :

* créer un événement ;
* configurer l'événement ;
* définir les billets ;
* publier l'événement après validation ;
* suivre les ventes ;
* consulter les statistiques ;
* gérer les éléments de son événement autorisés par Eventix.

---

## Équipe de l'organisateur

Personnes auxquelles l'organisateur peut attribuer certaines responsabilités liées à son événement.

Les membres de l'équipe ne sont pas nécessairement des employés d'Eventix.

---

## Administrateur Eventix

Acteur agissant au nom d'Eventix pour administrer et contrôler la plateforme.

Il peut notamment intervenir dans :

* la vérification des événements ;
* la vérification des organisateurs ;
* la sécurité ;
* la lutte contre la fraude ;
* les suspensions ;
* les opérations nécessitant une intervention d'Eventix.

---

## Vérification d'un événement

Processus par lequel Eventix contrôle un événement avant sa publication publique.

---

## Événement vérifié

Événement ayant passé avec succès le processus de vérification Eventix.

---

## Événement publié

Événement rendu visible publiquement conformément à son état et aux règles Eventix.

---

## Événement masqué

Événement qui existe toujours dans Eventix mais qui n'est plus visible publiquement.

---

## Événement archivé

Événement conservé dans l'historique mais qui n'est plus considéré comme actif.

---

## Événement annulé

Événement qui ne se déroulera pas conformément à la programmation initiale.

Une annulation peut déclencher les procédures métier prévues pour les billets et remboursements.

---

## Événement reporté

Événement dont la date ou la période a été déplacée à une nouvelle date.

Les billets existants peuvent rester valables selon les règles définies.

---

# 4. Billetterie

## Billetterie

Ensemble des activités permettant de :

* définir des billets ;
* gérer leur disponibilité ;
* les vendre ;
* les attribuer ;
* les contrôler ;
* suivre leur utilisation.

---

## Billet

Titre permettant à une personne d'obtenir le droit d'accès à un événement selon les conditions définies par Eventix et l'organisateur.

Un billet possède une identité unique.

---

## Billet gratuit

Billet dont le prix est nul.

Un billet gratuit consomme néanmoins une disponibilité.

---

## Billet payant

Billet dont le prix est supérieur à zéro.

---

## Catégorie de billet

Type de billet commercialisé pour un événement.

Exemples :

* Standard ;
* VIP ;
* Premium.

Une catégorie peut avoir ses propres :

* prix ;
* quantité ;
* conditions ;
* caractéristiques.

---

## Place

Emplacement physique auquel un billet peut donner accès.

---

## Place numérotée

Place possédant un numéro ou un identifiant permettant de l'identifier individuellement.

Exemple :

```text
VIP — Place 24
```

---

## Zone

Espace physique d'un événement regroupant éventuellement plusieurs places.

Exemples :

* Zone VIP ;
* Tribune A ;
* Balcon ;
* Entrée générale.

---

## Capacité

Nombre maximal de participants ou de places qu'une zone, catégorie ou configuration peut accueillir selon les règles de l'événement.

---

## Disponibilité

Quantité ou ensemble de places pouvant encore être attribuées ou vendues.

---

## Inventaire

Ensemble des disponibilités gérées par Eventix pour les ventes.

L'inventaire doit être cohérent entre les différents canaux de vente.

---

## Sold Out

État dans lequel aucun billet officiellement disponible à la vente ne reste pour l'offre concernée.

```text
Disponibilité > 0
      ↓
     Vente
      ↓
Disponibilité = 0
      ↓
   SOLD OUT
```

---

# 5. Réservation et achat

## Réservation

Opération permettant de bloquer temporairement une disponibilité pendant qu'une transaction d'achat est en cours.

---

## Réservation `PENDING`

Réservation temporairement active et non encore finalisée.

---

## Réservation `EXPIRED`

Réservation dont le délai autorisé est dépassé sans finalisation.

La disponibilité peut alors être libérée.

---

## Achat

Opération par laquelle un acheteur obtient un ou plusieurs billets selon les conditions de vente.

---

## Vente

Opération commerciale enregistrée par Eventix à la suite de l'obtention d'un billet.

Une vente peut être réalisée par différents canaux.

---

## Vente en ligne

Vente réalisée directement par le participant ou l'acheteur via les interfaces en ligne d'Eventix.

---

## Vente physique

Vente réalisée par l'intermédiaire d'un point physique partenaire.

La vente reste enregistrée dans Eventix.

---

## Canal de vente

Origine ou moyen par lequel une vente est réalisée.

Dans le MVP :

```text
ONLINE
PHYSICAL
```

---

## Attribution

Opération par laquelle une disponibilité devient associée à un billet ou à un participant/propriétaire.

---

# 6. Paiement

## Paiement

Opération financière par laquelle l'acheteur règle le montant associé à une vente.

---

## Paiement confirmé

Paiement dont Eventix a reçu une confirmation fiable indiquant que la transaction a réussi.

---

## Paiement échoué

Paiement dont la transaction n'a pas abouti.

---

## Paiement en attente

Paiement dont le résultat définitif n'est pas encore connu.

---

## Mobile Money

Moyen de paiement électronique utilisé pour les achats en ligne dans le MVP.

---

## CASH

Mode de paiement utilisé notamment pour les ventes physiques lorsqu'un agent confirme avoir reçu l'argent en espèces.

---

## Réconciliation

Processus permettant de comparer et de rapprocher les différentes informations d'une opération afin de déterminer son état réel et les montants correspondants.

La réconciliation peut concerner :

* paiements ;
* ventes ;
* remboursements ;
* frais ;
* settlement.

---

## Transaction

Opération financière ou métier enregistrée par Eventix.

Le terme doit être précisé par son contexte lorsqu'il peut être ambigu :

* transaction de paiement ;
* transaction de vente ;
* transaction de remboursement ;
* transaction de revente future.

---

# 7. Remboursement

## Remboursement

Opération permettant de restituer tout ou partie d'un montant payé par un acheteur.

---

## Éligibilité au remboursement

État indiquant qu'une opération satisfait les conditions permettant de demander ou d'exécuter un remboursement.

---

## Remboursement progressif

Traitement des remboursements par lots ou progressivement plutôt que nécessairement en une seule opération.

---

# 8. Contrôle d'accès

## Contrôle d'accès

Opération permettant de vérifier un billet au moment de l'entrée à un événement.

---

## Contrôleur

Personne autorisée à effectuer le contrôle des billets pour un événement ou une entrée selon les droits qui lui sont attribués.

---

## Scanner

Outil utilisé par un contrôleur pour lire les informations présentes sur un billet, notamment son QR code.

---

## QR code

Code visuel associé à un billet permettant d'identifier le billet lors du contrôle.

---

## Validation du billet

Opération par laquelle Eventix confirme qu'un billet peut être utilisé pour accéder à l'événement.

---

## Billet utilisé

Billet ayant déjà été validé pour l'accès à l'événement.

Un billet utilisé ne peut pas être validé une seconde fois pour le même accès.

---

## Billet invalide

Billet qui ne satisfait pas les conditions nécessaires à son utilisation.

Exemples :

* billet inexistant ;
* billet annulé ;
* billet déjà utilisé ;
* billet ne correspondant pas à l'événement.

---

## Entrée

Point physique par lequel les participants accèdent à l'événement.

---

## Point d'entrée

Emplacement ou entrée spécifique auquel un contrôleur peut être affecté.

Exemple :

```text
Concert
 ├── Entrée A
 ├── Entrée B
 └── Entrée VIP
```

---

## Présence

Fait qu'un participant soit effectivement entré dans l'événement.

La présence est constatée à travers le contrôle d'accès.

---

# 9. Réseau de points physiques

## Point physique

Partenaire physique permettant de vendre des billets Eventix aux participants.

---

## Partenaire

Organisation ou entité ayant une relation contractuelle ou opérationnelle avec Eventix.

---

## Agent physique

Personne autorisée par Eventix à effectuer certaines opérations dans un point physique.

L'agent physique appartient au réseau opérationnel Eventix et n'est pas un employé créé directement par l'organisateur dans Eventix.

---

## Habilitation

Autorisation accordée à un acteur pour effectuer certaines opérations.

Une habilitation peut être limitée :

* à un événement ;
* à un point physique ;
* à une opération ;
* à une période ;
* à un rôle.

---

## Réseau de distribution

Ensemble des points physiques et agents permettant de distribuer les billets Eventix.

---

# 10. Rôles et autorisations

## Rôle

Ensemble de responsabilités et d'autorisations associées à un acteur.

---

## Permission

Autorisation permettant d'effectuer une opération spécifique.

---

## Accès par événement

Modèle dans lequel les droits d'un acteur sont définis relativement à un événement particulier.

Exemple :

```text
Olemme
 ↓
Contrôleur
 ↓
Concert X
 ↓
Entrée A
```

---

## Équipe

Ensemble des personnes auxquelles des responsabilités sont attribuées pour un événement ou une organisation.

---

# 11. Propriété et transfert

## Propriétaire du billet

Détenteur actuel du billet.

---

## Transfert de billet

Opération permettant de changer le propriétaire d'un billet sans créer une copie du billet.

---

## Historique de propriété

Historique des propriétaires successifs d'un billet.

---

## Revente

Opération par laquelle un propriétaire cherche à céder son billet en échange d'une contrepartie financière.

Dans Eventix, la revente via Marketplace est une fonctionnalité future.

---

## Marketplace

Service permettant de mettre en relation des détenteurs de billets souhaitant revendre leurs billets avec des acheteurs.

Dans Eventix, cette fonctionnalité est prévue pour une version future et doit être activée après le `SOLD OUT` selon les règles définies.

---

# 12. Finance et organisateur

## Settlement

Opération par laquelle Eventix règle à l'organisateur le montant qui lui est effectivement dû après les opérations de réconciliation.

```text
Ventes
 ↓
Paiements
 ↓
Remboursements
 ↓
Frais / ajustements
 ↓
Réconciliation
 ↓
Settlement
```

---

## Montant payable

Montant effectivement dû à l'organisateur après prise en compte des éléments financiers applicables.

---

## Frais Eventix

Montants retenus par Eventix selon les conditions commerciales applicables.

---

## Montant bloqué

Montant temporairement retenu et qui ne peut pas encore être inclus dans le settlement.

---

## Solde organisateur

Montant résultant des opérations financières liées aux ventes de l'organisateur.

Ce terme devra être utilisé avec précision pour éviter de le confondre avec le montant immédiatement retirable.

---

# 13. Statistiques

## Statistique

Information calculée à partir des données Eventix afin d'analyser l'activité.

---

## Vente en temps réel

Information reflétant l'état actuel des ventes avec un délai suffisamment faible pour être considérée comme actuelle dans le contexte métier.

---

## Taux de remplissage

Proportion des disponibilités attribuées ou vendues par rapport à la capacité concernée.

---

## Performance d'un événement

Ensemble d'indicateurs permettant d'évaluer les résultats d'un événement.

---

## Performance d'un point physique

Indicateurs permettant notamment de comparer les ventes réalisées par différents points physiques.

---

## Performance d'une catégorie

Indicateurs permettant d'analyser les ventes d'une catégorie de billet.

Exemple :

```text
VIP       → 85 %
Standard  → 62 %
```

---

# 14. Sécurité et confiance

## Fraude

Comportement ou opération visant à tromper Eventix, un organisateur, un participant ou un partenaire afin d'obtenir un avantage indu.

---

## Faux événement

Événement présenté comme réel mais dont l'objectif ou l'existence est frauduleux.

---

## Vérification

Processus de contrôle permettant de déterminer si une information ou une opération satisfait les critères définis par Eventix.

---

## Suspension

Mesure temporaire limitant l'utilisation d'un compte, événement ou service.

---

## Blocage

Interdiction d'effectuer une opération donnée.

---

## Traçabilité

Capacité à retrouver l'origine, l'auteur, le contexte et l'historique d'une opération.

---

## Audit

Examen des opérations et informations enregistrées afin de vérifier leur conformité ou de rechercher des anomalies.

---

# 15. États métier

## DRAFT

État dans lequel un événement est encore en préparation.

---

## PENDING

État indiquant qu'une opération est en attente de finalisation.

---

## EXPIRED

État indiquant qu'une réservation temporaire a dépassé son délai.

---

## PUBLISHED

État indiquant qu'un événement est publié conformément aux règles Eventix.

---

## SOLD OUT

État indiquant qu'aucun billet officiellement disponible à la vente ne reste.

---

## COMPLETED

État indiquant que l'événement est terminé.

---

## POSTPONED

État indiquant que l'événement a été reporté.

---

## CANCELLED

État indiquant que l'événement a été annulé.

---

## USED

État indiquant qu'un billet a déjà été utilisé.

---

# 16. Termes financiers et opérationnels à ne pas confondre

Les termes suivants représentent des concepts différents.

```text
Réservation
     ≠
Achat
     ≠
Vente
     ≠
Paiement
     ≠
Billet
     ≠
Présence
     ≠
Remboursement
     ≠
Settlement
```

De même :

```text
Acheteur
     ≠
Participant
     ≠
Propriétaire du billet
```

Et :

```text
Organisateur
     ≠
Équipe de l'organisateur
     ≠
Administrateur Eventix
     ≠
Agent physique
     ≠
Contrôleur
```

---

# 17. Termes hors MVP

Les termes suivants existent dans la vision produit mais ne font pas partie du périmètre métier détaillé du MVP.

## Quiz Live

Fonctionnalité permettant d'organiser des quiz interactifs pendant un événement.

Les règles métier seront définies dans une version future.

---

## QR Cloud

Service futur permettant potentiellement de centraliser ou distribuer des contenus multimédias associés à un événement.

---

## Reels

Fonctionnalité future permettant de produire ou partager des contenus vidéo courts liés aux événements.

---

## Marketplace de revente

Domaine futur permettant la revente sécurisée de billets après épuisement des billets officiels.

---

# 18. Règles de vocabulaire

Les termes suivants doivent être utilisés de manière cohérente dans la documentation Eventix.

### Utiliser

* **événement** plutôt que « activité » lorsqu'il s'agit d'un objet géré par Eventix ;
* **billet** plutôt que « ticket » dans la documentation métier française ;
* **participant** pour la personne qui participe ;
* **acheteur** pour la personne qui réalise l'achat ;
* **propriétaire du billet** pour le détenteur actuel ;
* **vente** pour l'opération commerciale ;
* **paiement** pour l'opération financière ;
* **settlement** pour le règlement financier de l'organisateur ;
* **contrôleur** pour la personne qui valide les billets ;
* **point physique** pour le lieu partenaire de vente ;
* **agent physique** pour la personne habilitée à vendre dans ce réseau.

---

# 19. Termes nécessitant une précision future

Certains termes devront être précisés lorsque nous approfondirons le modèle métier :

* disponibilité ;
* capacité ;
* zone ;
* place ;
* catégorie ;
* réservation ;
* vente ;
* transaction ;
* solde organisateur ;
* settlement ;
* événement vérifié ;
* événement publié ;
* suspension ;
* habilitation ;
* partenaire ;
* point physique.

Ces termes sont suffisamment définis pour la découverte métier actuelle, mais pourront recevoir des définitions plus précises lors du DDD.

---

# 20. Principe fondamental du glossaire

> **Un terme métier important doit avoir une signification stable et partagée dans l'ensemble du projet.**

Lorsqu'un terme possède plusieurs significations selon le contexte, Eventix doit le préciser explicitement plutôt que de réutiliser le même mot de manière ambiguë.

Le glossaire constitue ainsi la base du **langage ubiquitaire (Ubiquitous Language)** qui sera utilisé lors de la phase DDD.

---

# 21. Statut du document

**Version :** 1.0
**Statut :** Validé pour la découverte métier
**Périmètre :** Eventix MVP + vocabulaire des évolutions futures
**Marché initial :** Cameroun

Toute nouvelle notion métier importante doit être ajoutée au glossaire avant ou pendant son intégration dans le modèle DDD.
