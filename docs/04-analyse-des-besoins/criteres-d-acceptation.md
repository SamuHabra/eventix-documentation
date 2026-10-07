# Critères d'acceptation — Eventix

> **Phase :** 04 — Analyse des besoins  
> **Projet :** Eventix  
> **Périmètre :** MVP — Billetterie  
> **Marché :** Cameroun  
> **Version :** 1.0  
> **Statut :** À valider par l'équipe

---

# 1. Objectif

Ce document définit les critères d'acceptation permettant de vérifier que les User Stories du MVP Eventix satisfont les comportements métier attendus.

Un critère d'acceptation décrit une condition observable et vérifiable permettant de déterminer si une capacité attendue est correctement satisfaite.

Les critères ne décrivent pas :

- l'architecture technique ;
- les services internes ;
- les APIs ;
- les tables de base de données ;
- les technologies ;
- les mécanismes d'implémentation ;
- les étapes détaillées d'un cas de test.

---

# 2. Principes

Les critères d'acceptation Eventix respectent les principes suivants :

1. Chaque critère possède un identifiant global unique `AC-XXX`.
2. Chaque critère référence explicitement sa User Story.
3. Les critères sont regroupés par User Story.
4. Un critère doit être compréhensible et vérifiable indépendamment autant que possible.
5. Les comportements importants et explicitement attendus doivent être couverts.
6. L'exhaustivité artificielle n'est pas recherchée.
7. Les valeurs concrètes ne sont utilisées que lorsqu'elles sont définies par une source métier validée.
8. Les données d'exemple sont explicitement présentées comme telles et ne constituent pas des règles métier.
9. Les états métier sont mentionnés lorsqu'ils sont nécessaires à la compréhension du comportement.
10. Les événements temporels peuvent constituer des déclencheurs.
11. Les comportements de sécurité observables peuvent constituer des critères.
12. Les exigences non fonctionnelles disposent de leur propre mécanisme de vérification.
13. Les mécanismes techniques internes ne sont pas imposés.
14. Un critère ne doit pas dépendre d'une architecture particulière.
15. Un critère reste stable tant que le comportement métier attendu ne change pas.
16. Une règle métier n'a besoin d'être représentée par un critère que lorsqu'elle influence une User Story et produit un comportement vérifiable.
17. Une décision métier non résolue ne doit jamais être transformée en exigence inventée.

---

# 3. Convention d'écriture

Les critères utilisent prioritairement la forme :

```text
Étant donné ...
Lorsque ...
Alors ...
```

Un critère peut comporter plusieurs conditions `Étant donné` lorsque celles-ci sont nécessaires pour établir le contexte.

Les critères peuvent couvrir :

- le scénario nominal ;
- un scénario alternatif ;
- un scénario d'exception ;
- un comportement temporel ;
- un comportement de sécurité observable ;
- une transition d'état métier.

Les critères ne constituent pas les cas de test détaillés.

---

# 4. Convention d'identification

Les critères utilisent des identifiants globaux et stables :

```text
AC-001
AC-002
AC-003
...
```

L'identifiant est indépendant de la User Story.

La relation est conservée explicitement :

```text
AC-001
User Story : US-001
```

Cela permet de conserver un identifiant stable même lorsque l'organisation des User Stories évolue.

---

# 5. Participant

## US-001 — Découvrir un événement

**Use Case :** UC-001  
**Besoin métier :** B05

### AC-001 — Présenter les événements disponibles

Étant donné que des événements sont disponibles à la consultation,  
lorsque le participant recherche des événements,  
alors Eventix lui présente les événements correspondant aux possibilités de consultation.

### AC-002 — Accéder aux informations d'un événement

Étant donné qu'un événement est présenté dans les résultats,  
lorsque le participant sélectionne cet événement,  
alors Eventix lui permet d'accéder aux informations disponibles de l'événement.

---

## US-002 — Consulter les détails d'un événement

**Use Case :** UC-001  
**Besoin métier :** B05

### AC-003 — Consulter les informations détaillées

Étant donné qu'un événement est accessible,  
lorsque le participant consulte sa fiche,  
alors les informations nécessaires à sa décision de participation lui sont présentées.

---

## US-003 — Obtenir un billet gratuit

**Use Case :** UC-002  
**Besoin métier :** B06  
**Règles :** RM06, RM12

### AC-004 — Attribuer un billet gratuit disponible

Étant donné qu'un événement gratuit est disponible et qu'une disponibilité existe,  
lorsque le participant demande un billet gratuit,  
alors Eventix lui attribue un billet et consomme la disponibilité correspondante.

### AC-005 — Refuser lorsque la disponibilité est épuisée

Étant donné qu'aucune disponibilité n'est disponible pour l'événement gratuit,  
lorsque le participant demande un billet,  
alors Eventix n'attribue aucun billet et l'informe que la disponibilité n'est plus disponible.

### AC-006 — Ne pas attribuer un billet à deux participants

Étant donné qu'une disponibilité ou un billet a déjà été attribué,  
lorsqu'une nouvelle attribution incompatible est tentée,  
alors Eventix refuse cette attribution.

---

## US-004 — Acheter un billet

**Use Case :** UC-003  
**Besoin métier :** B06  
**Règles :** RM01, RM02, RM03, RM05, RM06, RM07, RM25

### AC-007 — Finaliser un achat réussi

Étant donné qu'un événement est disponible à la vente et qu'une disponibilité existe,  
lorsque le paiement est confirmé,  
alors l'achat est finalisé, le billet est émis et devient utilisable.

### AC-008 — Ne pas émettre de billet après un paiement échoué

Étant donné qu'un participant tente d'acheter un billet,  
lorsque le paiement échoue,  
alors l'achat n'est pas finalisé sur la base de ce paiement et aucun billet n'est émis à ce titre.

### AC-009 — Ne produire qu'un seul effet pour une confirmation répétée

Étant donné qu'un paiement a déjà produit ses effets métier,  
lorsque la même confirmation de paiement est reçue à nouveau,  
alors aucun second billet, aucune seconde vente et aucun second effet financier ne sont produits.

---

## US-005 — Réserver temporairement une disponibilité

**Use Case :** UC-003, UC-024  
**Besoin métier :** B14  
**Règles :** RM01, RM02, RM03

### AC-010 — Bloquer temporairement une disponibilité

Étant donné qu'une disponibilité est accessible à la réservation,  
lorsque le participant initie une réservation valide,  
alors la disponibilité est temporairement bloquée et la réservation est `PENDING`.

### AC-011 — Expirer une réservation dépassée

Étant donné qu'une réservation est `PENDING`,  
lorsque son délai défini par Eventix est dépassé,  
alors la réservation passe à `EXPIRED` et la disponibilité correspondante est libérée.

### AC-012 — Ne pas assimiler expiration et échec de paiement

Étant donné qu'une réservation est `EXPIRED`,  
lorsqu'une confirmation de paiement arrive ultérieurement,  
alors cette confirmation est traitée selon le processus de réconciliation applicable et n'est pas considérée automatiquement comme un paiement échoué.

---

## US-006 — Finaliser un paiement

**Use Case :** UC-003  
**Besoin métier :** B06 / B16  
**Règles :** RM03, RM05, RM07

### AC-013 — Enregistrer les effets d'un paiement confirmé

Étant donné qu'un participant effectue un paiement pour un achat éligible,  
lorsque le paiement est confirmé,  
alors Eventix considère le paiement comme confirmé et permet la finalisation de l'achat conformément aux règles métier.

### AC-014 — Conserver le prix effectivement payé

Étant donné qu'un achat a été finalisé à un prix donné,  
lorsque l'organisateur modifie ultérieurement le prix,  
alors le montant déjà payé par le participant reste inchangé.

### AC-015 — Garantir l'idempotence du paiement

Étant donné qu'une confirmation de paiement a déjà été traitée,  
lorsque la même confirmation est reçue à nouveau,  
alors elle ne produit aucun nouvel effet métier.

---

## US-007 — Récupérer son billet

**Use Case :** UC-004, UC-025  
**Besoin métier :** B07

### AC-016 — Rendre le billet accessible

Étant donné qu'un billet a été émis,  
lorsque le participant consulte ses billets,  
alors Eventix lui permet d'accéder à son billet.

### AC-017 — Permettre le téléchargement

Étant donné qu'un billet a été émis et est accessible au participant,  
lorsque celui-ci demande son téléchargement,  
alors Eventix lui permet de télécharger le billet.

### AC-018 — Permettre l'envoi par email

Étant donné qu'un billet a été émis,  
lorsque le canal email est disponible,  
alors Eventix peut envoyer le billet au participant par email.

> WhatsApp n'est pas inclus dans le MVP.

---

## US-008 — Utiliser son billet lors de l'événement

**Use Case :** UC-014, UC-026  
**Besoin métier :** B21 / B22  
**Règles :** RM13, RM14, RM17

### AC-019 — Autoriser un billet valide

Étant donné qu'un billet à entrée unique correspond à l'événement et est valide,
lorsque l'agent effectue le contrôle,
alors l'accès est autorisé et, pour un billet à entrée unique, le billet devient `USED`.

### AC-020 — Refuser un billet déjà utilisé

Étant donné qu'un billet est déjà `USED`,  
lorsque l'agent tente de le contrôler à nouveau,  
alors le contrôle est refusé et l'accès n'est pas autorisé.

### AC-021 — Distinguer billet valide et présence

Étant donné qu'un participant possède un billet valide,  
lorsque le participant n'a pas encore été contrôlé,  
alors Eventix ne considère pas automatiquement que le participant est présent.

---

## US-009 — Transférer un billet lorsque cela est autorisé

**Statut :** À PRÉCISER  
**Besoin métier :** B27

### AC-022 — Comportement à préciser

Les conditions exactes de transfert n'étant pas encore suffisamment définies, aucun comportement métier supplémentaire ne doit être inventé.

**Statut :** À préciser.

Les critères définitifs seront ajoutés lorsque les conditions de transfert auront été validées.

---

## US-010 — Signaler une activité suspecte

**Use Case :** UC-017  
**Besoin métier :** B30

### AC-023 — Enregistrer un signalement

Étant donné qu'un participant souhaite signaler une activité suspecte,  
lorsqu'il soumet son signalement,  
alors Eventix enregistre le signalement afin qu'il puisse être analysé.

### AC-024 — Ne pas considérer automatiquement le signalement comme une fraude

Étant donné qu'un signalement a été enregistré,  
lorsqu'il est transmis à l'analyse,  
alors le signalement ne constitue pas automatiquement une preuve de fraude.

---

# 6. Organisateur

## US-011 — Créer un événement

**Use Case :** UC-005  
**Besoin métier :** B01

### AC-025 — Créer un événement

Étant donné qu'un organisateur souhaite préparer un événement,  
lorsqu'il crée l'événement,  
alors Eventix crée le support nécessaire à sa configuration.

---

## US-012 — Configurer un événement

**Use Case :** UC-005  
**Besoin métier :** B02  
**Règles :** RM23, RM24

### AC-026 — Configurer les informations nécessaires

Étant donné qu'un événement existe,  
lorsque l'organisateur le configure,  
alors Eventix permet de définir notamment les informations générales, la date, le lieu, les espaces, la capacité, les catégories de billets, les prix et les périodes de vente.

### AC-027 — Refuser une capacité incompatible

Étant donné qu'un événement possède déjà des billets attribués,  
lorsque l'organisateur tente de réduire la capacité sous le nombre de billets déjà attribués,  
alors Eventix refuse la modification.

### AC-028 — Préserver les billets existants

Étant donné que des billets ont déjà été achetés,  
lorsque l'organisateur modifie une configuration applicable aux ventes futures,  
alors les billets déjà achetés ne sont pas automatiquement détruits ou invalidés.

---

## US-013 — Publier un événement

**Use Case :** UC-006  
**Besoin métier :** B03  
**Règle :** RM28

### AC-029 — Publier un événement conforme

Étant donné qu'un événement existe, que ses informations nécessaires sont configurées et que les contrôles requis sont satisfaits,  
lorsque l'organisateur demande sa publication,  
alors Eventix rend l'événement disponible conformément à son état métier.

### AC-030 — Refuser une publication non conforme

Étant donné que les conditions nécessaires à la publication ne sont pas satisfaites,  
lorsque l'organisateur demande la publication,  
alors Eventix ne finalise pas la publication.

---

## US-014 — Consulter ses événements

**Use Case :** UC-007  
**Besoin métier :** B04

### AC-031 — Présenter les événements accessibles

Étant donné qu'un organisateur possède des événements auxquels il a accès,  
lorsqu'il consulte ses événements,  
alors Eventix lui présente ces événements et leur état disponible.

---

## US-015 — Rechercher et filtrer ses événements

**Use Case :** UC-007  
**Besoin métier :** B04

### AC-032 — Rechercher un événement

Étant donné que plusieurs événements sont accessibles à l'organisateur,  
lorsqu'il effectue une recherche,  
alors Eventix lui présente les événements correspondant aux critères de recherche.

### AC-033 — Filtrer les événements

Étant donné que plusieurs événements sont accessibles à l'organisateur,  
lorsqu'il applique un filtre disponible,  
alors Eventix limite les résultats aux événements correspondant au filtre.

---

## US-016 — Configurer les espaces et disponibilités

**Use Case :** UC-005  
**Besoin métier :** B08  
**Règle :** cohérence de l'inventaire

### AC-034 — Configurer les disponibilités

Étant donné qu'un événement est en cours de configuration,  
lorsque l'organisateur définit ses espaces et disponibilités,  
alors Eventix enregistre la capacité et les disponibilités correspondantes.

### AC-035 — Ne pas attribuer deux fois une même disponibilité

Étant donné qu'une disponibilité a déjà été attribuée,  
lorsqu'une nouvelle attribution incompatible est tentée,  
alors Eventix refuse cette attribution.

---

## US-017 — Gérer les prix

**Use Case :** UC-008  
**Besoin métier :** B20  
**Règle :** RM07

### AC-036 — Définir un prix

Étant donné qu'un événement est configurable,  
lorsque l'organisateur définit un prix,  
alors ce prix devient applicable aux ventes concernées.

### AC-037 — Appliquer une modification aux ventes futures

Étant donné qu'un prix est modifié,  
lorsque de nouvelles ventes sont réalisées après cette modification,  
alors le nouveau prix s'applique aux ventes concernées.

### AC-038 — Préserver le prix historiquement payé

Étant donné qu'un participant a déjà finalisé un achat,  
lorsque le prix est modifié ultérieurement,  
alors le montant historiquement payé par ce participant n'est pas modifié.

---

## US-018 — Suivre les participants

**Use Case :** UC-009  
**Besoin métier :** B25  
**Règle :** RM17

### AC-039 — Consulter les participants associés

Étant donné qu'un événement possède des billets ou des participants associés,  
lorsque l'organisateur consulte les participants,  
alors Eventix lui présente les informations disponibles relatives à leur situation.

### AC-040 — Distinguer vente, participant attendu et présence

Étant donné qu'un billet a été vendu mais n'a pas encore été contrôlé,  
lorsque l'organisateur consulte la situation de l'événement,  
alors Eventix ne considère pas automatiquement le participant comme présent.

---

## US-019 — Suivre l'activité de l'événement

**Use Case :** UC-009  
**Besoin métier :** B26  
**Règles :** RM31, RM32

### AC-041 — Consulter l'activité

Étant donné qu'un événement possède une activité enregistrée,  
lorsque l'organisateur consulte son activité,  
alors Eventix présente les informations disponibles concernant notamment les ventes, entrées, participants présents et disponibilités.

### AC-042 — Préserver l'historique

Étant donné qu'une opération métier importante a été enregistrée,  
lorsque l'organisateur consulte l'activité historique,  
alors l'information historique pertinente reste disponible.

---

## US-020 — Analyser les performances de l'événement

**Use Case :** UC-010  
**Besoin métier :** B31  
**Règles :** RM31, RM32, RM33

### AC-043 — Consulter les indicateurs

Étant donné qu'un événement possède des données d'activité,  
lorsque l'organisateur consulte ses performances,  
alors Eventix présente les indicateurs disponibles, notamment les ventes, le taux de présence, le chiffre d'affaires et les remboursements.

### AC-044 — Analyser les données par contexte

Étant donné que les données disponibles possèdent différents contextes,  
lorsque l'organisateur analyse les performances,  
alors Eventix permet de distinguer les données pertinentes par canal, catégorie ou période lorsque ces informations existent.

### AC-045 — Ne pas modifier les données métier

Étant donné que l'organisateur consulte ou analyse des statistiques,  
lorsque les indicateurs sont calculés ou présentés,  
alors l'analyse ne modifie pas les données métier.

---

## US-021 — Consulter des indicateurs d'aide à la décision

**Use Case :** UC-010  
**Besoin métier :** B32  
**Statut :** À PRÉCISER

### AC-046 — Comportement à préciser

Le niveau exact des recommandations et indicateurs d'aide à la décision reste à confirmer.

Aucun comportement supplémentaire ne doit être inventé avant validation du périmètre.

**Statut :** À préciser.

---

## US-022 — Gérer un report d'événement

**Use Case :** UC-011  
**Besoin métier :** B19  
**Règles :** RM22, RM23

### AC-047 — Conserver les billets lors d'un report

Étant donné qu'un événement possède des billets déjà achetés,  
lorsque l'événement est reporté,  
alors les billets existants sont conservés et restent associés au nouvel événement ou à sa nouvelle date selon les règles applicables.

### AC-048 — Ne pas rembourser automatiquement un simple report

Étant donné qu'un événement est simplement reporté,  
lorsque le report est enregistré,  
alors le report seul ne déclenche pas automatiquement un remboursement.

### AC-049 — Cas incompatible à préciser

Étant donné qu'une nouvelle configuration est incompatible avec certains billets existants,  
lorsque le report est traité,  
alors les billets concernés sont identifiés et le traitement applicable reste **À préciser** tant que la décision métier correspondante n'est pas validée.

---

## US-023 — Gérer l'annulation d'un événement

**Use Case :** UC-012  
**Besoin métier :** B18  
**Règles :** RM19, RM20, RM21

### AC-050 — Bloquer les nouvelles ventes

Étant donné qu'un événement est annulé,  
lorsque l'annulation est prise en compte,  
alors l'événement n'accepte plus de nouvelles ventes.

### AC-051 — Invalider les billets concernés

Étant donné qu'un événement est annulé,  
lorsque l'annulation est prise en compte,  
alors les billets concernés deviennent invalides conformément aux règles métier.

### AC-052 — Rendre les billets éligibles au remboursement

Étant donné qu'un événement est annulé pour une cause imputable à l'organisateur,  
lorsque l'annulation est traitée,  
alors les billets concernés deviennent éligibles au remboursement selon le processus applicable.

### AC-053 — Ne pas rembourser deux fois

Étant donné qu'un remboursement a déjà été exécuté pour une cause donnée,  
lorsqu'une nouvelle tentative de remboursement identique est effectuée,  
alors Eventix ne réalise pas un second remboursement.

---

## US-024 — Consulter le règlement financier

**Use Case :** UC-013, UC-022, UC-023  
**Besoin métier :** B33  
**Statut :** Certaines conditions financières restent À PRÉCISER.

### AC-054 — Suivre le cycle financier

Étant donné que des ventes ont été réalisées pour un événement,  
lorsque l'organisateur consulte son règlement,  
alors Eventix lui permet de suivre les principales étapes du cycle financier jusqu'à la disponibilité du solde.

### AC-055 — Ne pas rendre les fonds disponibles avant la clôture

Étant donné qu'un événement n'est pas encore financièrement clôturé,  
lorsque l'organisateur consulte ses fonds,  
alors les fonds concernés ne sont pas considérés comme disponibles pour retrait.

### AC-056 — Conditions détaillées à préciser

Les conditions financières détaillées qui ne sont pas encore validées restent **À préciser** et ne doivent pas être inventées dans les critères.

---

# 7. Agent de contrôle

## US-025 — Contrôler un billet

**Use Case :** UC-014  
**Besoins métier :** B21, B22, B23, B24  
**Règles :** RM13, RM14, RM15, RM16, RM17

### AC-057 — Autoriser un billet valide

Étant donné qu'un billet est valide et correspond à l'événement contrôlé,  
lorsque l'agent le scanne,  
alors le contrôle est accepté et l'accès est autorisé.

### AC-058 — Refuser un billet du mauvais événement

Étant donné qu'un billet ne correspond pas à l'événement contrôlé,  
lorsque l'agent le scanne,  
alors le contrôle est refusé et l'accès n'est pas autorisé.

### AC-059 — Refuser un billet annulé

Étant donné qu'un billet est annulé,  
lorsque l'agent le scanne,  
alors le contrôle est refusé et l'accès n'est pas autorisé.

### AC-060 — Enregistrer le contrôle

Étant donné qu'un contrôle est effectué,  
lorsque le contrôle aboutit à une décision,  
alors le résultat du contrôle est enregistré avec son contexte métier disponible.

---

## US-026 — Refuser un billet déjà utilisé

**Use Case :** UC-014, UC-026  
**Besoin métier :** B22  
**Règles :** RM13, RM14

### AC-061 — Refuser une seconde utilisation

Étant donné qu'un billet a déjà été utilisé pour l'événement,  
lorsqu'un agent tente de le contrôler à nouveau,  
alors le contrôle est refusé et l'accès est refusé.

### AC-062 — Garantir une seule validation réussie

Étant donné que plusieurs tentatives de validation concernent simultanément le même billet,  
lorsque les contrôles sont traités,  
alors une seule validation peut réussir et les autres sont refusées.

---

## US-027 — Enregistrer un contrôle

**Use Case :** UC-014  
**Besoin métier :** B23  
**Règle :** RM16

### AC-063 — Conserver le contexte du contrôle

Étant donné qu'un contrôle est effectué,  
lorsque son résultat est enregistré,  
alors Eventix conserve le contexte nécessaire, notamment le billet, l'événement, le contrôleur, le point d'entrée, la date, l'heure et le résultat lorsque ces informations sont disponibles.

---

## US-028 — Être affecté à un point d'entrée

**Use Case :** UC-014  
**Besoin métier :** B24  
**Règle :** RM15

### AC-064 — Identifier le point d'entrée

Étant donné qu'un agent est affecté à un point d'entrée,  
lorsqu'il effectue un contrôle,  
alors le contexte du contrôle permet d'identifier le point d'entrée auquel il est affecté.

### AC-065 — Ne pas créer de restriction implicite

Étant donné qu'un agent est affecté à un point d'entrée,  
lorsqu'il contrôle un billet valide pour l'événement,  
alors cette affectation seule ne constitue pas automatiquement une restriction sur la catégorie du billet.

---

## US-029 — Contrôler avec plusieurs scanners lorsque l'état est fiable

**Use Case :** UC-015

### AC-066 — Autoriser plusieurs scanners avec un état fiable

Étant donné que plusieurs scanners sont disponibles et que leur état partagé est fiable,  
lorsque plusieurs agents effectuent des contrôles,  
alors plusieurs contrôles peuvent être réalisés tout en respectant l'unicité de validation d'un billet.

### AC-067 — Préserver l'unicité de validation

Étant donné qu'un même billet est présenté à plusieurs scanners,  
lorsque les validations sont traitées,  
alors une seule validation du billet peut réussir.

---

## US-030 — Passer en mode mono-scanner lorsque la synchronisation devient non fiable

**Use Case :** UC-015

### AC-068 — Basculer en mode dégradé

Étant donné que l'état partagé entre plusieurs scanners n'est plus fiable,  
lorsque cette situation est détectée,  
alors Eventix passe en mode dégradé et n'autorise qu'un seul scanner à effectuer les validations.

### AC-069 — Reprendre le mode multi-scanners

Étant donné que la synchronisation fiable est rétablie,  
lorsque l'état partagé redevient fiable,  
alors le fonctionnement multi-scanners peut reprendre.

---

# 8. Administrateur

## US-031 — Vérifier un organisateur ou un événement

**Use Case :** UC-016  
**Besoin métier :** B28  
**Règle :** RM28

### AC-070 — Effectuer une vérification

Étant donné qu'un organisateur ou un événement doit être vérifié,  
lorsque l'administrateur effectue la vérification,  
alors Eventix permet d'enregistrer la décision résultante.

### AC-071 — Maintenir une situation non vérifiée sous contrôle

Étant donné que les informations disponibles ne permettent pas de considérer une situation comme suffisamment fiable,  
lorsque la vérification est terminée,  
alors la situation peut rester limitée jusqu'à résolution.

---

## US-032 — Gérer le niveau de risque

**Use Case :** UC-017, UC-018  
**Besoin métier :** B29  
**Règles :** RM29, RM30

### AC-072 — Adapter la réponse au risque

Étant donné qu'un niveau de risque est identifié,  
lorsque l'administrateur traite la situation,  
alors la mesure appliquée est proportionnée au niveau de risque identifié.

### AC-073 — Conserver la décision

Étant donné qu'une mesure de sécurité est appliquée,  
lorsque la décision est enregistrée,  
alors la décision reste traçable.

---

## US-033 — Analyser un signalement

**Use Case :** UC-017  
**Besoins métier :** B29, B30  
**Règles :** RM29, RM30

### AC-074 — Analyser un signalement

Étant donné qu'un signalement a été reçu,  
lorsque l'administrateur l'analyse,  
alors il peut examiner les éléments disponibles et déterminer le niveau de risque.

### AC-075 — Ne pas sanctionner automatiquement

Étant donné qu'un signalement ne confirme pas à lui seul une fraude,  
lorsque l'analyse ne confirme aucun risque suffisant,  
alors aucune sanction définitive n'est appliquée sur la seule base du signalement.

### AC-076 — Appliquer une mesure lorsqu'un risque est confirmé

Étant donné qu'un risque est confirmé,  
lorsque l'administrateur détermine la mesure appropriée,  
alors Eventix applique une mesure proportionnée et enregistre la décision.

---

## US-034 — Suspendre ou limiter une opération à risque

**Use Case :** UC-018  
**Besoin métier :** B29  
**Règles :** RM29, RM30

### AC-077 — Appliquer une limitation proportionnée

Étant donné qu'une situation présente un niveau de risque justifiant une limitation ou une suspension,  
lorsque l'administrateur applique la mesure,  
alors l'opération concernée est limitée ou suspendue conformément au niveau de risque.

### AC-078 — Traçabilité de la mesure

Étant donné qu'une limitation ou suspension a été appliquée,  
lorsque la décision est enregistrée,  
alors la mesure reste traçable.

---

## US-035 — Auditer les décisions sensibles

**Use Case :** UC-019  
**Besoins métier :** B29, B30  
**Règles :** RM30, RM31

### AC-079 — Retrouver une décision sensible

Étant donné qu'une décision sensible a été enregistrée,  
lorsque l'administrateur consulte son historique,  
alors Eventix permet de retrouver la décision et son contexte.

### AC-080 — Restituer les informations d'audit

Étant donné qu'une décision sensible est consultée,  
lorsque l'administrateur examine son historique,  
alors les informations disponibles comprennent notamment l'acteur, l'action, le contexte, l'événement, la date et le résultat.

---

# 9. Finance

## US-036 — Gérer les remboursements

**Use Case :** UC-020  
**Besoin métier :** B17  
**Règles :** RM19, RM20, RM21

### AC-081 — Identifier un remboursement éligible

Étant donné qu'un paiement répond aux conditions d'éligibilité au remboursement,  
lorsque Eventix traite la situation,  
alors un remboursement peut être créé conformément aux règles métier.

### AC-082 — Traiter progressivement plusieurs remboursements

Étant donné que plusieurs paiements sont éligibles au remboursement,  
lorsque Eventix traite les remboursements,  
alors ceux-ci peuvent être traités progressivement jusqu'à leur finalisation.

### AC-083 — Ne pas exécuter deux fois le même remboursement

Étant donné qu'un remboursement a déjà été exécuté pour une cause donnée,  
lorsqu'une nouvelle tentative identique est effectuée,  
alors Eventix n'exécute pas un second remboursement.

---

## US-037 — Réconcilier un paiement tardif

**Use Case :** UC-021  
**Besoin métier :** B16  
**Règles :** RM02, RM03, RM04, RM05, RM06

### AC-084 — Attribuer le billet lorsqu'il est encore disponible

Étant donné qu'un paiement est confirmé après l'expiration de la réservation et que le billet est encore disponible,  
lorsque Eventix effectue la réconciliation,  
alors le billet peut être attribué au participant et l'achat est finalisé.

### AC-085 — Rembourser lorsque le billet n'est plus disponible

Étant donné qu'un paiement est confirmé après l'expiration de la réservation et que le billet a déjà été attribué,  
lorsque Eventix effectue la réconciliation,  
alors le billet n'est pas attribué au paiement tardif et, si le paiement a été encaissé, un remboursement est déclenché.

### AC-086 — Garantir l'idempotence de la réconciliation

Étant donné qu'une confirmation de paiement tardive a déjà été réconciliée,  
lorsque la même confirmation est reçue à nouveau,  
alors elle ne produit pas un second achat, un second billet ou un second remboursement.

---

## US-038 — Suivre les opérations financières

**Use Case :** UC-022, UC-023  
**Besoin métier :** B33

### AC-087 — Rendre les opérations financières consultables

Étant donné que des paiements, remboursements ou retraits ont été enregistrés,  
lorsque l'administrateur consulte l'historique financier,  
alors Eventix lui permet de retrouver les opérations disponibles.

### AC-088 — Rendre les fonds disponibles après clôture

Étant donné qu'un événement est terminé et que les opérations nécessaires sont réconciliées,  
lorsque la clôture financière est finalisée,  
alors le montant disponible pour l'organisateur peut être déterminé.

### AC-089 — Refuser un retrait supérieur au solde

Étant donné qu'un organisateur demande un retrait supérieur à son solde disponible,  
lorsque Eventix vérifie la demande,  
alors le retrait est refusé.

### AC-090 — Enregistrer un retrait réussi

Étant donné qu'un organisateur dispose d'un solde suffisant et que la clôture financière nécessaire est terminée,  
lorsqu'il effectue un retrait valide,  
alors le retrait est traité, le solde disponible est mis à jour et l'opération est enregistrée.

---

# 9.1 Pass multi-jours et dons optionnels

## US-041 — Configurer un pass multi-jours

**Use Case :** UC-027
**Règles :** RM23, RM24, RM34

### AC-091 — Définir les dates et le quota du pass

Étant donné qu'un organisateur configure une catégorie de billet,
lorsqu'il la définit comme un pass,
alors il peut définir ses dates de validité et son nombre maximal d'entrées.

### AC-092 — Respecter la disponibilité du pass

Étant donné qu'un pass est proposé à la vente,
lorsqu'un participant l'obtient ou l'achète,
alors un billet unique est émis avec les dates de validité et le quota configurés, et une seule disponibilité correspondante est attribuée.

## US-042 — Contrôler les entrées d'un pass

**Use Case :** UC-028
**Règles :** RM13, RM14, RM16, RM34

### AC-093 — Décrémenter le quota à chaque entrée acceptée

Étant donné qu'un pass est valide et qu'il reste des entrées,
lorsqu'un scan d'entrée est accepté,
alors une entrée est consommée et le participant est autorisé à entrer.

### AC-094 — Consommer une autre entrée après une sortie

Étant donné qu'un participant est sorti sans scan et que son pass a encore des entrées,
lorsqu'il présente à nouveau son pass et que le scan est accepté,
alors une entrée supplémentaire est consommée.

### AC-095 — Refuser un pass épuisé ou hors validité sans consommer d'entrée

Étant donné qu'un pass n'a plus d'entrée ou que la date du scan est hors de sa période de validité,
lorsqu'un agent le scanne,
alors l'accès est refusé et le quota ne diminue pas.

### AC-096 — Ne pas dépasser le quota lors de contrôles concurrents

Étant donné qu'il ne reste qu'une entrée sur un pass,
lorsque plusieurs contrôles de ce même pass sont tentés simultanément,
alors au plus un contrôle est accepté et le quota ne devient pas négatif.

## US-043 — Configurer les dons optionnels

**Use Case :** UC-030
**Règle :** RM35

### AC-097 — Activer les dons et définir des montants suggérés

Étant donné qu'un organisateur configure un événement,
lorsqu'il active les dons optionnels,
alors il peut définir les montants suggérés proposés aux participants.

## US-044 — Ajouter un don à une commande

**Use Case :** UC-029
**Règle :** RM35

### AC-098 — Choisir un montant suggéré ou libre

Étant donné que les dons sont activés pour un événement,
lorsqu'un participant prépare une commande avec un billet gratuit ou payant,
alors il peut choisir un montant suggéré, saisir un montant libre positif ou ne pas faire de don.

### AC-099 — Inclure le don au paiement sans le confondre avec le billet

Étant donné qu'un participant a ajouté un don supérieur à zéro,
lorsqu'Eventix présente le récapitulatif de la commande,
alors le don est affiché séparément du prix du billet et inclus dans le montant total à payer.

### AC-100 — Préserver le parcours du billet gratuit sans don

Étant donné qu'un participant demande un billet gratuit et ne choisit aucun don,
lorsqu'il confirme sa demande,
alors le billet suit le parcours gratuit existant et le participant ne paie rien.

### AC-101 — Exiger le paiement d'un don ajouté à un billet gratuit

Étant donné qu'un participant ajoute un don supérieur à zéro à une demande de billet gratuit,
lorsqu'il confirme sa commande,
alors le don doit être payé avant que le billet soit émis.

### AC-102 — Distinguer les dons dans les rapports

Étant donné qu'une commande comportant un don est finalisée,
lorsque l'organisateur consulte les informations de vente et de suivi financier,
alors le montant du don peut être distingué du montant des billets.

### AC-103 — Enregistrer le don séparément du billet

Étant donné qu'une commande comporte un don supérieur à zéro,
lorsqu'Eventix finalise la commande,
alors le don est enregistré séparément du billet sans créer de billet supplémentaire ni consommer de disponibilité ou d'entrée de pass.

## US-045 — Configurer la billetterie hybride

**Use Case :** UC-031
**Règle :** RM36

### AC-104 — Définir les modes d'accès des billets

Étant donné qu'un organisateur configure un événement,
lorsqu'il définit ses catégories de billets,
alors il peut associer à ces catégories un accès sur place, en ligne au direct ou à la VOD, ou une combinaison de ces accès.

## US-046 — Accéder à un direct ou à une VOD avec son billet

**Use Case :** UC-032
**Règle :** RM36

### AC-105 — Communiquer l'accès en ligne aux détenteurs autorisés

Étant donné qu'un participant détient un billet autorisant l'accès en ligne,
lorsque le contenu associé est disponible,
alors Eventix lui communique les informations d'accès prévues.

### AC-106 — Refuser l'accès en ligne à un billet non éligible

Étant donné qu'un billet n'autorise pas l'accès au direct ou à la VOD,
lorsque son détenteur tente d'obtenir les informations d'accès,
alors Eventix ne lui communique pas les informations d'accès réservées au contenu en ligne.

## US-047 — Créer et appliquer des codes promotionnels

**Use Case :** UC-033
**Règle :** RM37

### AC-107 — Appliquer la réduction d'un code valide

Étant donné qu'un code promotionnel est actif et applicable à la commande,
lorsque le participant le saisit,
alors la réduction configurée est affichée et prise en compte dans le montant à payer.

### AC-108 — Ne pas appliquer un code invalide ou non applicable

Étant donné qu'un code est invalide, inactif ou non applicable à la commande,
lorsque le participant le saisit,
alors le montant à payer n'est pas réduit et le participant en est informé.

## US-048 — Suivre les ventes issues de liens partagés

**Use Case :** UC-034
**Règle :** RM38

### AC-109 — Associer une commande finalisée à sa source de suivi

Étant donné qu'un participant accède à l'événement par un lien de suivi,
lorsqu'il finalise une commande,
alors cette commande est attribuée à la source associée au lien selon la règle d'attribution définie.

### AC-110 — Consulter les résultats par lien de suivi

Étant donné qu'un événement dispose de plusieurs liens de suivi,
lorsque l'organisateur consulte les statistiques,
alors il peut distinguer les visites, commandes finalisées et montants attribués à chaque lien.

## US-049 — Configurer un plan de salle interactif

**Use Case :** UC-035
**Règle :** RM39

### AC-111 — Afficher le plan et l'état des sièges

Étant donné qu'un organisateur a configuré un plan de salle et ses places numérotées,
lorsqu'un participant consulte l'offre de billetterie,
alors il peut consulter le plan interactif et distinguer les places disponibles des places indisponibles.

## US-050 — Choisir une place sur le plan de salle

**Use Case :** UC-036
**Règle :** RM39

### AC-112 — Associer la place sélectionnée au billet

Étant donné qu'un participant sélectionne une place disponible,
lorsque sa commande est finalisée,
alors la place sélectionnée est associée à son billet.

### AC-113 — Empêcher la vente multiple d'une même place

Étant donné que plusieurs participants tentent de réserver simultanément la même place,
lorsque leurs commandes sont finalisées,
alors au plus une commande obtient cette place et les autres participants doivent en sélectionner une autre.

### AC-114 — Configurer la réduction et les conditions d'un code

Étant donné qu'un organisateur configure la billetterie d'un événement,
lorsqu'il crée un code promotionnel,
alors il peut définir la réduction et les conditions selon lesquelles le code est applicable.

## US-051 — Consulter les alertes de cybersécurité

**Use Case :** UC-037 · **Règle :** RM40

### AC-115 — Présenter les alertes et leur contexte disponible

Étant donné qu'Eventix a détecté un signal correspondant à une règle de détection configurée,
lorsque l'analyste cybersécurité consulte le tableau de bord,
alors il peut voir l'alerte, son horodatage, sa catégorie, sa sévérité, son état et les actifs concernés lorsque ces informations sont disponibles.

### AC-116 — Rendre visibles les limites de couverture

Étant donné que les signaux disponibles ne couvrent pas toutes les attaques possibles,
lorsque le tableau de bord présente les alertes et indicateurs,
alors il ne les présente pas comme une preuve exhaustive de sécurité ou comme la garantie qu'aucune attaque n'est en cours.

## US-052 — Analyser et qualifier un incident cybersécurité

**Use Case :** UC-037 · **Règle :** RM40

### AC-117 — Distinguer une alerte d'une attaque confirmée

Étant donné qu'une nouvelle alerte est créée,
lorsque l'analyste l'examine,
alors il peut consigner une qualification et une justification sans qu'une alerte seule soit présentée comme une attaque confirmée.

## US-053 — Autoriser et tracer une réponse à un incident

**Use Case :** UC-037 · **Règle :** RM40

### AC-118 — Exiger une décision humaine pour les mesures

Étant donné qu'une alerte ou un incident est enregistré,
lorsqu'aucun responsable humain habilité n'a décidé d'une mesure,
alors Eventix ne déclenche pas automatiquement de confinement, suspension ou sanction.

### AC-119 — Tracer la décision et le résultat d'une mesure

Étant donné qu'un responsable humain habilité décide d'une mesure,
lorsqu'il consigne la décision,
alors le dossier conserve l'auteur, la justification, le périmètre, la date et le résultat de la mesure.

# 10. User Stories à statut À PRÉCISER

Les User Stories suivantes ne doivent pas recevoir de critères définitifs tant que les décisions métier correspondantes ne sont pas validées :

| User Story | Sujet | Statut |
|---|---|---|
| US-009 | Transfert de billet | À préciser |
| US-021 | Aide à la décision / recommandations | À préciser |
| US-024 | Conditions financières détaillées | À préciser |
| US-039 | Vente physique | À préciser / hors périmètre MVP actuel |
| US-040 | Traçabilité de vente physique | À préciser / hors périmètre MVP actuel |

Aucune valeur ou règle supplémentaire ne doit être inventée pour ces User Stories.

---

# 11. Hors périmètre

Les fonctionnalités suivantes ne font pas partie des critères d'acceptation du MVP actuel :

- marketplace de revente ;
- quiz live ;
- QR Cloud photos / vidéos ;
- reels ;
- services événementiels supplémentaires ;
- mode offline avancé ;
- synchronisation distribuée avancée.

Les fonctionnalités hors MVP devront recevoir leurs propres User Stories et critères lorsqu'elles entreront officiellement dans le périmètre produit.

---

# 12. Relation avec les Use Cases

La relation entre les artefacts suit le modèle :

```text
Objectif produit
       ↓
Besoin métier
       ↓
Exigence fonctionnelle
       ↓
User Story
       ↓
Use Case
       ↓
Critères d'acceptation
       ↓
Validation
```

Un Use Case peut être relié à plusieurs User Stories et une User Story peut être reliée à un ou plusieurs Use Cases.

Les critères couvrent les comportements pertinents sans imposer une correspondance artificielle entre chaque scénario et chaque critère.

La traçabilité complète entre les artefacts reste centralisée dans :

```text
matrice-de-tracabilite.md
```

---

# 13. Relation avec les règles métier

Les règles métier restent définies dans :

```text
03-decouverte-du-metier/regles-metier.md
```

Les critères n'en recopient pas inutilement la définition complète.

Ils vérifient uniquement leur effet observable lorsqu'une règle influence une User Story.

Exemple :

```text
RM02
Une réservation PENDING expire après son délai.
        ↓
US-005
Réserver temporairement une disponibilité.
        ↓
AC-011
La réservation passe à EXPIRED
et la disponibilité est libérée.
```

---

# 14. Relation avec les exigences non fonctionnelles

Les exigences non fonctionnelles ne sont pas transformées automatiquement en critères d'acceptation fonctionnels.

Elles disposent de leurs propres mécanismes de vérification.

Exemples :

```text
Performance
Disponibilité
Sécurité technique
Scalabilité
Résilience
Observabilité
```

Un critère peut néanmoins référencer une exigence non fonctionnelle lorsqu'elle influence directement le comportement attendu.

La relation complète sera centralisée dans :

```text
matrice-de-tracabilite.md
```

---

# 15. Séparation entre critères et tests

Les critères d'acceptation définissent :

> **Ce qui doit être vrai.**

Les tests définissent :

> **Comment vérifier que c'est vrai.**

Ainsi, un critère ne doit pas imposer :

- une requête SQL ;
- un endpoint particulier ;
- un service particulier ;
- une technologie ;
- une structure de base de données ;
- une procédure de test détaillée.

Exemple :

```text
Critère :

Étant donné qu'un billet est déjà USED,
lorsqu'un agent tente de le contrôler,
alors l'accès est refusé.
```

La stratégie de test pourra ultérieurement déterminer comment vérifier ce comportement.

---

# 16. Faible couplage avec l'architecture

Les critères d'acceptation expriment le **quoi observable**, et non le mécanisme technique permettant de l'obtenir.

Exemple :

```text
Paiement confirmé
       ↓
Billet attribué
```

Le critère ne prescrit pas :

```text
Payment Service
       ↓
REST
       ↓
Ticket Service
       ↓
Database
```

L'architecture pourra évoluer sans nécessiter de modification du critère tant que le comportement métier reste identique.

---

# 17. Stabilité des critères

Un critère d'acceptation reste stable tant que le comportement métier attendu ne change pas.

Par exemple, les évolutions suivantes ne nécessitent pas automatiquement de modifier un critère :

```text
Monolithe → services
REST → événements
Base de données A → base de données B
Architecture synchrone → architecture asynchrone
```

La relation correcte est :

```text
Besoin métier
      ↓
User Story
      ↓
Critère d'acceptation
      ↓
Conception
      ↓
Architecture
      ↓
Implémentation
```

---

# 18. Matrice synthétique

| User Story | Critères |
|---|---|
| US-001 | AC-001 → AC-002 |
| US-002 | AC-003 |
| US-003 | AC-004 → AC-006 |
| US-004 | AC-007 → AC-009 |
| US-005 | AC-010 → AC-012 |
| US-006 | AC-013 → AC-015 |
| US-007 | AC-016 → AC-018 |
| US-008 | AC-019 → AC-021 |
| US-009 | À préciser |
| US-010 | AC-023 → AC-024 |
| US-011 | AC-025 |
| US-012 | AC-026 → AC-028 |
| US-013 | AC-029 → AC-030 |
| US-014 | AC-031 |
| US-015 | AC-032 → AC-033 |
| US-016 | AC-034 → AC-035 |
| US-017 | AC-036 → AC-038 |
| US-018 | AC-039 → AC-040 |
| US-019 | AC-041 → AC-042 |
| US-020 | AC-043 → AC-045 |
| US-021 | À préciser |
| US-022 | AC-047 → AC-049 |
| US-023 | AC-050 → AC-053 |
| US-024 | AC-054 → AC-056 |
| US-025 | AC-057 → AC-060 |
| US-026 | AC-061 → AC-062 |
| US-027 | AC-063 |
| US-028 | AC-064 → AC-065 |
| US-029 | AC-066 → AC-067 |
| US-030 | AC-068 → AC-069 |
| US-031 | AC-070 → AC-071 |
| US-032 | AC-072 → AC-073 |
| US-033 | AC-074 → AC-076 |
| US-034 | AC-077 → AC-078 |
| US-035 | AC-079 → AC-080 |
| US-036 | AC-081 → AC-083 |
| US-037 | AC-084 → AC-086 |
| US-038 | AC-087 → AC-090 |
| US-041 | AC-091 → AC-092 |
| US-042 | AC-093 → AC-096 |
| US-043 | AC-097 |
| US-044 | AC-098 → AC-103 |
| US-039 | À préciser / hors MVP à confirmer |
| US-040 | À préciser / hors MVP à confirmer |

---

# 19. Règle de maintenance

Toute modification d'une User Story, d'une règle métier ou d'un comportement attendu doit entraîner une revue des critères concernés.

Toute modification technique qui ne modifie pas le comportement métier attendu ne doit pas entraîner automatiquement une modification des critères.

Avant validation finale, l'équipe doit vérifier :

- la couverture des User Stories du MVP ;
- la cohérence avec les Use Cases ;
- la cohérence avec les règles métier ;
- l'absence de décisions inventées ;
- l'absence de détails techniques ;
- la traçabilité des critères ;
- le traitement explicite des points À préciser.

---

# 20. Sources de référence

- `02-vision-produit/objectifs-produit.md`
- `03-decouverte-du-metier/besoins-metier.md`
- `03-decouverte-du-metier/contraintes-metier.md`
- `03-decouverte-du-metier/regles-metier.md`
- `03-decouverte-du-metier/processus-metier.md`
- `03-decouverte-du-metier/user-journeys.md`
- `03-decouverte-du-metier/questions-metier-ouvertes.md`
- `04-analyse-des-besoins/exigences-fonctionnelles.md`
- `04-analyse-des-besoins/exigences-non-fonctionnelles.md`
- `04-analyse-des-besoins/user-stories.md`
- `04-analyse-des-besoins/use-cases.md`

---

# 21. Statut du document

**Document :** `criteres-d-acceptation.md`  
**Version :** 1.0  
**Statut :** À valider par l'équipe  
**Périmètre :** MVP Eventix  
**Méthode :** Critères d'acceptation orientés comportement métier  
**Convention :** `AC-XXX`  
**Principe transversal :** Faible couplage  
**Traçabilité complète :** `matrice-de-tracabilite.md`

Ce document constitue la référence des critères permettant de valider les comportements métier des User Stories du MVP Eventix.