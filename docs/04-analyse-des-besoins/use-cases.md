# Use Cases — Eventix

> **Phase :** 04 — Analyse des besoins  
> **Projet :** Eventix  
> **Périmètre :** MVP — Billetterie  
> **Marché :** Cameroun  
> **Version :** 1.0  
> **Statut :** À valider

---

# 1. Objectif

Ce document décrit les principaux Use Cases du MVP Eventix.

Un Use Case représente un **objectif métier poursuivi par un acteur** ou un comportement métier significatif déclenché automatiquement.

Il décrit :

- l'objectif poursuivi ;
- les acteurs impliqués ;
- le déclencheur ;
- les préconditions utiles ;
- le scénario nominal ;
- les scénarios alternatifs ;
- les scénarios d'exception ;
- les postconditions utiles ;
- les règles métier qui influencent le comportement ;
- les relations de traçabilité pertinentes.

Il ne décrit pas :

- l'architecture technique ;
- les APIs ;
- les tables de base de données ;
- les classes ;
- les technologies ;
- les mécanismes d'implémentation.

---

# 2. Principes de conception

Les Use Cases Eventix respectent les principes suivants :

1. Un Use Case représente un **objectif métier**.
2. Une étape technique n'est pas automatiquement un Use Case.
3. Plusieurs User Stories peuvent être couvertes par un même Use Case.
4. Un Use Case n'est découpé que lorsqu'une partie constitue un **objectif métier indépendant**.
5. Les scénarios sont décrits au niveau métier.
6. Les états métier apparaissent lorsqu'ils sont nécessaires à la compréhension du scénario.
7. Les règles métier sont référencées lorsqu'elles influencent le comportement.
8. Les questions métier ouvertes restent explicitement **À préciser**.
9. Les critères d'acceptation détaillés restent dans `criteres-d-acceptation.md`.
10. Les systèmes externes peuvent être des acteurs secondaires.
11. Les relations UML `include` et `extend` ne sont utilisées que lorsqu'elles représentent une véritable relation comportementale.
12. Le faible couplage entre les artefacts doit être préservé.
13. Les Use Cases couvrent le périmètre MVP validé.
14. Les fonctionnalités futures ne sont pas détaillées ici.

---

# 3. Convention d'identification

Les Use Cases utilisent des identifiants globaux :

```text
UC-001
UC-002
UC-003
...



Les identifiants restent indépendants de l'organisation du document.



4. Acteurs
4.1 Acteurs principaux
Acteur	Responsabilité principale
Participant	Découvrir un événement, obtenir et utiliser un billet
Organisateur	Gérer ses événements et suivre leur activité
Agent de contrôle	Contrôler les billets à l'entrée
Administrateur	Vérifier, analyser et traiter les situations sensibles
4.2 Acteurs secondaires

Un acteur secondaire participe à un Use Case sans être nécessairement propriétaire de l'objectif métier.

Il peut notamment s'agir :

d'un prestataire de paiement ;
d'un service externe de notification.

Un système externe n'est représenté que lorsqu'il intervient réellement dans le Use Case.

5. Convention de scénario

Chaque Use Case peut contenir :

Préconditions
Déclencheur
Scénario nominal
Scénarios alternatifs
Scénarios d'exception
Postconditions
Règles métier
Traçabilité

Les préconditions et postconditions ne sont présentes que lorsqu'elles sont utiles à la compréhension du comportement métier.

6. Diagramme global
                                      ┌──────────────────────┐
                                      │    Administrateur    │
                                      └──────────┬───────────┘
                                                 │
                         ┌───────────────────────┼──────────────────────┐
                         │                       │                      │
                         ▼                       ▼                      ▼
                  Vérifier événement      Analyser risque       Gérer finance




┌───────────────┐
│ Participant   │
└───────┬───────┘
        │
        ├──────────────► Découvrir événement
        │
        ├──────────────► Obtenir billet
        │                       │
        │                       ├── Billet gratuit
        │                       │
        │                       └── Billet payant
        │
        └──────────────► Récupérer billet




┌───────────────┐
│ Organisateur  │
└───────┬───────┘
        │
        ├──────────────► Gérer événement
        ├──────────────► Configurer événement
        ├──────────────► Publier événement
        ├──────────────► Gérer prix
        ├──────────────► Suivre activité
        ├──────────────► Analyser performances
        └──────────────► Gérer report / annulation




┌──────────────────────┐
│ Agent de contrôle    │
└──────────┬───────────┘
           │
           └────────────► Contrôler billet




              Événements métier / temporels
                          │
                          ▼
              ┌────────────────────────┐
              │ Use Cases automatiques │
              ├────────────────────────┤
              │ Expirer réservation    │
              │ Réconcilier paiement   │
              │ Émettre billet         │
              │ Gérer remboursement    │
              │ Clôturer finances      │
              └────────────────────────┘
7. Participant
UC-001 — Découvrir un événement

Acteur principal : Participant

Objectif :

Permettre au participant de trouver un événement correspondant à ce qu'il recherche.

Besoins métier :

B05

User Stories liées :

US-001
US-002

Déclencheur :

Le participant souhaite trouver un événement.

Scénario nominal
Le participant recherche des événements.
Eventix présente les événements disponibles.
Le participant sélectionne un événement.
Eventix présente les informations détaillées disponibles.
Le participant peut consulter les caractéristiques de l'événement.
Postcondition

Le participant dispose des informations nécessaires pour décider de poursuivre son parcours.

Règles métier

Aucune règle métier critique supplémentaire identifiée.

Traçabilité
Besoin : B05
User Stories : US-001, US-002
Exigences fonctionnelles : à relier dans la matrice de traçabilité.
8. Participant — Billetterie
UC-002 — Obtenir un billet gratuit

Acteur principal : Participant

Objectif :

Permettre au participant d'obtenir un billet pour un événement gratuit.

Besoins métier :

B06

User Story liée :

US-003

Déclencheur :

Le participant souhaite participer à un événement gratuit.

Préconditions
L'événement est disponible.
Une disponibilité est accessible.
Scénario nominal
Le participant demande un billet gratuit.
Eventix vérifie la disponibilité.
Eventix attribue le billet.
La disponibilité correspondante est consommée.
Le billet est émis.
Le billet devient disponible pour le participant.
Scénario alternatif — Absence de disponibilité
Eventix constate qu'aucune disponibilité n'est disponible.
Le billet gratuit n'est pas attribué.
Le participant est informé que la disponibilité n'est plus disponible.
Postcondition

Un billet gratuit est attribué au participant lorsque les conditions sont satisfaites.

Règles métier
RM06 — Un billet ne peut être attribué à deux participants simultanément.
RM12 — Un billet gratuit consomme une disponibilité.
Traçabilité
Besoin : B06
User Story : US-003
Exigences fonctionnelles : à relier dans la matrice.
UC-003 — Acheter un billet

Acteur principal : Participant

Acteurs secondaires :

Prestataire de paiement

Objectif :

Permettre au participant d'obtenir un billet payant pour un événement.

Besoins métier :

B06
B14
B15
B16

User Stories liées :

US-004
US-005
US-006
US-007

Déclencheur :

Le participant souhaite acheter un billet payant.

Préconditions
L'événement est disponible à la vente.
Une disponibilité correspondant au billet recherché existe.
Scénario nominal
Le participant sélectionne un billet disponible.
Eventix vérifie la disponibilité.
Eventix crée une réservation temporaire.
La disponibilité est temporairement bloquée.
Le participant effectue le paiement.
Le prestataire de paiement traite l'opération.
Eventix reçoit la confirmation du paiement.
Eventix finalise l'achat.
Eventix émet le billet.
Le billet devient utilisable.
Le participant peut récupérer son billet.
Scénario alternatif — Paiement échoué
Le participant tente d'effectuer le paiement.
Le paiement échoue.
Eventix ne finalise pas l'achat.
Le billet n'est pas émis sur la base de ce paiement échoué.
La réservation suit les règles applicables à son état.
Scénario alternatif — Réservation expirée avant confirmation
La réservation reste PENDING.
Le délai de réservation est dépassé.
La réservation passe à EXPIRED.
La disponibilité est libérée.
Le paiement peut néanmoins faire l'objet d'une confirmation ultérieure.
Le scénario suit alors le Use Case de réconciliation d'un paiement tardif.
Règles métier
RM01 — Une réservation bloque temporairement une disponibilité.
RM02 — Une réservation expire après son délai.
RM03 — Réservation, paiement et billet sont distincts.
RM05 — Un paiement ne peut produire ses effets qu'une seule fois.
RM06 — Un billet ne peut être attribué à deux participants simultanément.
RM07 — Le prix payé est définitif.
RM25 — La confirmation du paiement rend le billet utilisable.
Postcondition

En cas de succès :

Réservation
    ↓
Paiement confirmé
    ↓
Achat finalisé
    ↓
Billet émis
Traçabilité
Besoins : B06, B14, B15, B16
User Stories : US-004, US-005, US-006, US-007
Règles : RM01, RM02, RM03, RM05, RM06, RM07, RM25
Exigences fonctionnelles : à relier dans la matrice.
UC-004 — Récupérer un billet

Acteur principal : Participant

Objectif :

Permettre au participant d'accéder à son billet après son émission.

Besoin métier :

B07

User Story liée :

US-007

Déclencheur :

Un billet a été émis.

Scénario nominal
Eventix rend le billet accessible au participant.
Le participant consulte son billet depuis son compte.
Le participant peut télécharger le billet.
Eventix peut envoyer le billet par email.
Hors périmètre

WhatsApp n'est pas inclus dans le MVP.

Postcondition

Le participant dispose de son billet.

Traçabilité
Besoin : B07
User Story : US-007
9. Organisateur
UC-005 — Créer et configurer un événement

Acteur principal : Organisateur

Objectif :

Permettre à l'organisateur de préparer un événement pour sa mise en vente.

Besoins métier :

B01
B02
B08

User Stories liées :

US-011
US-012
US-016

Déclencheur :

L'organisateur souhaite créer un nouvel événement.

Scénario nominal
L'organisateur crée un événement.
L'organisateur renseigne les informations générales.
L'organisateur définit la date.
L'organisateur définit le lieu.
L'organisateur configure les espaces.
L'organisateur définit la capacité.
L'organisateur configure les catégories de billets.
L'organisateur définit les prix.
L'organisateur définit les périodes de vente.
Eventix conserve la configuration de l'événement.
Scénario alternatif — Capacité incompatible
L'organisateur tente de réduire la capacité.
Eventix vérifie les billets déjà attribués.
Si la nouvelle capacité est inférieure aux billets déjà attribués, la modification est refusée.
Règles métier
RM23 — Une modification future ne détruit pas les billets existants.
RM24 — La capacité ne peut pas être inférieure aux billets déjà attribués.
Postcondition

L'événement est configuré conformément aux informations fournies et aux règles métier.

Traçabilité
Besoins : B01, B02, B08
User Stories : US-011, US-012, US-016
Règles : RM23, RM24
UC-006 — Publier un événement

Acteur principal : Organisateur

Objectif :

Rendre un événement disponible aux participants selon les conditions définies.

Besoin métier :

B03

User Story liée :

US-013

Déclencheur :

L'organisateur demande la publication de l'événement.

Préconditions
L'événement existe.
Les informations nécessaires sont configurées.
Les contrôles requis par le processus métier ont été effectués.
Scénario nominal
L'organisateur demande la publication.
Eventix vérifie les conditions nécessaires.
L'événement est rendu disponible selon son état métier.
L'événement peut entrer dans son cycle de vente.
Scénario alternatif — Vérification non satisfaite
La vérification nécessaire n'est pas satisfaite.
La publication n'est pas finalisée.
L'événement reste dans l'état approprié.
Règles métier
RM28 — Eventix doit contrôler les organisateurs et les événements.
Postcondition

L'événement est publié lorsque les conditions nécessaires sont satisfaites.

Traçabilité
Besoin : B03
User Story : US-013
Règle : RM28
UC-007 — Consulter et gérer ses événements

Acteur principal : Organisateur

Objectif :

Permettre à l'organisateur de retrouver et suivre ses événements.

Besoin métier :

B04

User Stories liées :

US-014
US-015
Scénario nominal
L'organisateur consulte ses événements.
Eventix présente les événements auxquels il a accès.
L'organisateur recherche un événement si nécessaire.
L'organisateur applique des filtres si nécessaire.
L'organisateur consulte l'état d'un événement.
L'organisateur accède aux informations associées.
Traçabilité
Besoin : B04
User Stories : US-014, US-015
UC-008 — Gérer les prix

Acteur principal : Organisateur

Objectif :

Permettre à l'organisateur de définir et modifier les prix applicables aux ventes futures.

Besoin métier :

B20

User Story liée :

US-017
Scénario nominal
L'organisateur définit un prix.
Eventix utilise ce prix pour les ventes concernées.
L'organisateur modifie ultérieurement le prix si nécessaire.
La nouvelle valeur s'applique aux ventes futures.
Règle métier
RM07 — Le prix payé est définitif.
Conséquence

Une modification future du prix ne modifie pas le montant déjà payé.

Traçabilité
Besoin : B20
User Story : US-017
Règle : RM07
UC-009 — Suivre l'activité d'un événement

Acteur principal : Organisateur

Objectif :

Permettre à l'organisateur de suivre l'activité de son événement.

Besoin métier :

B26

User Story liée :

US-019
Scénario nominal
L'organisateur consulte l'activité de son événement.
Eventix présente les informations disponibles.
L'organisateur peut notamment suivre :
les ventes ;
les entrées ;
les participants présents ;
les disponibilités ;
l'activité des points physiques lorsque cette donnée existe.
Règles métier
RM31 — Les données historiques doivent être conservées.
RM32 — Les ventes doivent être analysables par contexte.
Traçabilité
Besoin : B26
User Story : US-019
Règles : RM31, RM32
UC-010 — Analyser les performances d'un événement

Acteur principal : Organisateur

Objectif :

Permettre à l'organisateur d'analyser les performances de son événement.

Besoins métier :

B31
B32

User Stories liées :

US-020
US-021
Scénario nominal
L'organisateur consulte les statistiques de son événement.
Eventix présente les données disponibles.
L'organisateur analyse notamment :
les ventes globales ;
les ventes par canal ;
les ventes par catégorie ;
les ventes par période ;
le taux de présence ;
le chiffre d'affaires ;
les remboursements.
Eventix peut mettre en évidence certaines tendances.
Limite

Les recommandations automatisées avancées restent dépendantes du périmètre effectivement validé.

Règles métier
RM31 — Les données historiques doivent être conservées.
RM32 — Les ventes doivent être analysables par contexte.
RM33 — Les statistiques ne doivent pas modifier les données métier.
Traçabilité
Besoins : B31, B32
User Stories : US-020, US-021
Règles : RM31, RM32, RM33
UC-011 — Gérer le report d'un événement

Acteur principal : Organisateur

Objectif :

Gérer le report d'un événement en conservant le traitement approprié des billets existants.

Besoin métier :

B19

User Story liée :

US-022
Scénario nominal
L'organisateur déclenche le report de l'événement.
Eventix enregistre la nouvelle date.
Les billets déjà achetés sont conservés.
Les billets sont rattachés à la nouvelle date lorsque les conditions le permettent.
L'événement poursuit son cycle avec sa nouvelle date.
Scénario alternatif — Configuration incompatible
La nouvelle configuration est incompatible avec certains billets existants.
Eventix identifie les billets concernés.
Le traitement applicable doit être déterminé selon les conditions métier.
Statut

À préciser pour le traitement détaillé des cas incompatibles.

Règle métier
RM22 — Un billet est conservé lors d'un report.
RM23 — Une modification future ne détruit pas les billets existants.
Traçabilité
Besoin : B19
User Story : US-022
Règles : RM22, RM23
Question ouverte : à référencer lorsqu'un identifiant QMO validé sera disponible.
UC-012 — Gérer l'annulation d'un événement

Acteur principal : Organisateur

Objectif :

Gérer les conséquences métier de l'annulation d'un événement.

Besoin métier :

B18

User Story liée :

US-023
Scénario nominal
L'organisateur demande l'annulation de l'événement.
Eventix met l'événement dans l'état approprié.
L'événement n'accepte plus de nouvelles ventes.
Les billets concernés deviennent invalides.
Eventix identifie les conséquences financières.
Les billets éligibles suivent le processus de remboursement.
Règles métier
RM19 — L'annulation imputable à l'organisateur rend les billets éligibles au remboursement.
RM20 — Les remboursements peuvent être traités progressivement.
RM21 — Un remboursement ne peut être exécuté plusieurs fois.
Postcondition

L'événement est annulé et les conséquences métier associées sont prises en charge.

Traçabilité
Besoin : B18
User Story : US-023
Règles : RM19, RM20, RM21
UC-013 — Suivre le règlement de l'organisateur

Acteur principal : Organisateur

Objectif :

Permettre à l'organisateur de suivre le cycle financier associé à ses ventes.

Besoin métier :

B33

User Story liée :

US-024
Scénario nominal
Les paiements des participants sont enregistrés.
Les fonds restent en attente.
Les opérations nécessaires de réconciliation sont réalisées.
L'événement se termine.
La clôture financière est effectuée.
Le montant disponible pour l'organisateur est déterminé.
Le solde disponible peut ensuite être retiré selon les règles applicables.
Règles métier
RM26 — Le règlement de l'organisateur est différé.
L'organisateur ne peut pas retirer plus que son solde disponible.
Statut

Les conditions financières détaillées restent à préciser lorsqu'elles ne sont pas encore définies dans les règles métier.

Traçabilité
Besoin : B33
User Story : US-024
Question ouverte : à référencer lorsque nécessaire.
10. Agent de contrôle
UC-014 — Contrôler un billet

Acteur principal : Agent de contrôle

Objectif :

Déterminer si un billet peut être utilisé pour accéder à l'événement.

Besoins métier :

B21
B22
B23
B24

User Stories liées :

US-025
US-026
US-027
US-028

Déclencheur :

Le participant présente son billet à l'entrée.

Préconditions
L'agent est autorisé à effectuer le contrôle.
L'événement est accessible au contrôle.
Scénario nominal
L'agent scanne le billet.
Eventix vérifie son authenticité.
Eventix vérifie que le billet correspond à l'événement.
Eventix vérifie son statut.
Eventix vérifie s'il a déjà été utilisé.
Si les conditions sont satisfaites, le contrôle est accepté.
Le billet devient USED.
L'accès est autorisé.
Le contrôle est enregistré.
Scénario alternatif — Billet déjà utilisé
Eventix détecte que le billet a déjà été utilisé.
Le contrôle est refusé.
L'accès est refusé.
Le contrôle est enregistré.
Scénario alternatif — Mauvais événement
Eventix constate que le billet ne correspond pas à l'événement.
Le contrôle est refusé.
L'accès est refusé.
Le contrôle est enregistré.
Scénario alternatif — Billet annulé
Eventix constate que le billet est annulé.
Le contrôle est refusé.
L'accès est refusé.
Le contrôle est enregistré.
Règles métier
RM13 — Un billet déjà utilisé est refusé.
RM14 — Un billet ne peut être validé qu'une seule fois.
RM15 — Une affectation physique n'est pas automatiquement une restriction de billet.
RM16 — Les contrôles doivent être traçables.
RM17 — Billet et présence sont deux informations différentes.
Postcondition

En cas de succès :

Billet valide
    ↓
Accès autorisé
    ↓
Billet USED
    ↓
Présence constatée
Traçabilité
Besoins : B21, B22, B23, B24
User Stories : US-025 à US-028
Règles : RM13, RM14, RM15, RM16, RM17
UC-015 — Contrôler les billets avec plusieurs scanners

Acteur principal : Agent de contrôle

Objectif :

Permettre le contrôle avec plusieurs scanners lorsque l'état partagé est fiable.

User Story liée :

US-029
US-030
Préconditions
Plusieurs scanners sont disponibles.
Un état partagé fiable est disponible.
Scénario nominal
Plusieurs scanners sont connectés.
L'état partagé est fiable.
Plusieurs contrôles peuvent être effectués.
Les validations respectent l'unicité du contrôle.
Scénario alternatif — État partagé non fiable
Eventix détecte que l'état partagé n'est plus fiable.
Le système passe en mode dégradé.
Un seul scanner reste autorisé à effectuer les validations.
Les autres scanners attendent le rétablissement d'un état partagé fiable.
Le contrôle reprend en mode multi-scanners lorsque la synchronisation redevient fiable.
Règles métier
RM14 — Une seule validation peut réussir.
Principe métier de contrôle multi-scanners : plusieurs scanners nécessitent un état partagé fiable.
Traçabilité
User Stories : US-029, US-030
11. Administrateur
UC-016 — Vérifier un organisateur ou un événement

Acteur principal : Administrateur

Objectif :

Réduire les risques liés aux faux organisateurs, faux événements et comportements frauduleux.

Besoin métier :

B28

User Story liée :

US-031
Scénario nominal
L'administrateur examine un organisateur ou un événement.
Les informations nécessaires à la vérification sont consultées.
L'administrateur détermine si les conditions de confiance sont suffisantes.
La décision est enregistrée.
Scénario alternatif — Vérification insuffisante
Les informations disponibles ne permettent pas de considérer la situation comme suffisamment fiable.
Des contrôles supplémentaires peuvent être appliqués.
L'événement ou l'organisation peut rester limité jusqu'à résolution.
Règle métier
RM28 — Eventix doit contrôler les organisateurs et les événements.
Traçabilité
Besoin : B28
User Story : US-031
Règle : RM28
UC-017 — Analyser un signalement

Acteur principal : Administrateur

Objectif :

Analyser un problème ou une activité suspecte afin de déterminer une mesure adaptée.

Besoins métier :

B29
B30

User Stories liées :

US-010
US-032
US-033
Déclencheur

Un utilisateur signale un problème ou une activité suspecte.

Scénario nominal
Eventix reçoit le signalement.
L'administrateur analyse la situation.
Les éléments disponibles sont examinés.
Le niveau de risque est évalué.
Une mesure adaptée est déterminée.
La décision est enregistrée.
Scénario alternatif — Aucun risque confirmé
Le signalement est analysé.
Aucun risque suffisant n'est confirmé.
Aucune sanction définitive n'est appliquée.
Le signalement reste traçable.
Scénario alternatif — Risque confirmé
Le risque est confirmé.
Eventix applique une mesure proportionnée au niveau de risque.
La mesure est enregistrée.
Règles métier
RM29 — Le niveau de risque détermine la réponse.
RM30 — Les décisions sensibles doivent être traçables.
Un signalement n'est pas automatiquement une preuve de fraude.
Traçabilité
Besoins : B29, B30
User Stories : US-010, US-032, US-033
Règles : RM29, RM30
UC-018 — Limiter ou suspendre une activité à risque

Acteur principal : Administrateur

Objectif :

Appliquer une mesure proportionnée lorsqu'un niveau de risque le justifie.

Besoin métier :

B29

User Story liée :

US-034
Scénario nominal
Une situation à risque est identifiée.
Le niveau de risque est évalué.
Eventix applique la mesure correspondant au niveau identifié.
La décision est enregistrée.
Correspondance métier
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
Règles métier
RM29
RM30
Postcondition

La mesure appliquée correspond au niveau de risque identifié et reste traçable.

UC-019 — Auditer une décision sensible

Acteur principal : Administrateur

Objectif :

Permettre l'analyse ultérieure des décisions sensibles.

Besoins métier :

B29
B30

User Story liée :

US-035
Scénario nominal
L'administrateur consulte une décision sensible.
Eventix restitue son contexte.
L'administrateur identifie l'action réalisée.
L'administrateur peut consulter les informations nécessaires à l'audit.
Informations métier concernées
acteur ;
action ;
contexte ;
événement ;
date ;
résultat.
Règles métier
RM30 — Les décisions sensibles doivent être traçables.
RM31 — Les données historiques doivent être conservées.
12. Finance
UC-020 — Gérer un remboursement

Acteur principal : Administrateur

Objectif :

Traiter un remboursement lorsqu'une obligation de remboursement existe.

Besoin métier :

B17

User Story liée :

US-036
Déclencheurs possibles
annulation d'un événement ;
paiement tardif avec billet déjà attribué ;
autre situation explicitement éligible selon les règles métier.
Scénario nominal
Eventix identifie un paiement éligible au remboursement.
Le remboursement est créé.
Le remboursement est traité.
Le statut du remboursement évolue jusqu'à sa finalisation.
L'opération est enregistrée.
Scénario alternatif — Remboursements nombreux
Plusieurs billets sont éligibles.
Eventix traite les remboursements progressivement.
Les opérations déjà traitées ne sont pas répétées.
Le traitement se poursuit jusqu'à finalisation.
Règles métier
RM19 — L'annulation imputable à l'organisateur rend les billets éligibles au remboursement.
RM20 — Les remboursements peuvent être traités progressivement.
RM21 — Un remboursement ne peut être exécuté plusieurs fois.
Traçabilité
Besoin : B17
User Story : US-036
Règles : RM19, RM20, RM21
UC-021 — Réconcilier un paiement confirmé après expiration

Acteur principal : Eventix — comportement métier automatique

Acteur secondaire éventuel : Prestataire de paiement

Objectif :

Déterminer le traitement à appliquer lorsqu'une confirmation de paiement arrive après l'expiration de la réservation.

Besoin métier :

B16

User Story liée :

US-037

Déclencheur :

Une confirmation de paiement arrive alors que la réservation est déjà EXPIRED.

Préconditions
Un paiement a été confirmé.
La réservation correspondante est expirée.
Scénario nominal — Billet disponible
Eventix reçoit la confirmation.
Eventix vérifie l'état de la réservation.
Eventix constate que la réservation est expirée.
Eventix vérifie si le billet est encore disponible.
Le billet peut encore être attribué.
Eventix attribue le billet.
Le traitement de l'achat est finalisé.
Scénario alternatif — Billet déjà attribué
Eventix reçoit la confirmation.
Eventix constate que la réservation est expirée.
Eventix vérifie la disponibilité.
Le billet a déjà été attribué.
Eventix ne peut pas attribuer le billet au paiement concerné.
Si le paiement a été encaissé, un remboursement est déclenché.
Scénario d'exception — Confirmation répétée
Eventix reçoit plusieurs confirmations correspondant au même paiement.
Eventix identifie qu'elles concernent la même opération.
Une seule opération métier est produite.
Règles métier
RM02
RM03
RM04
RM05
RM06
Postcondition

Le paiement tardif est traité selon la disponibilité réelle du billet.

UC-022 — Gérer la clôture financière

Acteur principal : Eventix — comportement métier automatique / administratif

Objectif :

Finaliser le traitement financier d'un événement terminé avant de rendre les fonds disponibles pour retrait.

Besoin métier :

B33

User Story liée :

US-038

Déclencheur :

L'événement est terminé.

Scénario nominal
L'événement arrive à sa fin.
Eventix lance la clôture financière.
Les opérations nécessaires de réconciliation sont prises en compte.
Le montant net disponible est déterminé.
Le montant devient disponible dans le solde de l'organisateur.
Scénario alternatif — Réconciliation non terminée
L'événement est terminé.
Certaines opérations financières restent à réconcilier.
La clôture n'est pas considérée comme entièrement finalisée.
Les opérations nécessaires sont poursuivies.
Règles métier
Le règlement de l'organisateur est différé.
Un organisateur ne peut jamais retirer plus que son solde disponible.
Les opérations financières doivent être traçables.
Traçabilité
Besoin : B33
User Story : US-024 / US-038
UC-023 — Effectuer un retrait organisateur

Acteur principal : Organisateur

Objectif :

Permettre à l'organisateur de retirer des fonds disponibles après la clôture financière.

User Story liée :

US-038
Préconditions
L'événement concerné est terminé.
La clôture financière nécessaire est terminée.
Un solde disponible existe.
Scénario nominal
L'organisateur demande un retrait.
Eventix vérifie le solde disponible.
Eventix vérifie que le montant demandé ne dépasse pas le solde.
Le retrait est traité.
Le solde disponible est mis à jour.
Le retrait est enregistré.
Scénario alternatif — Solde insuffisant
L'organisateur demande un montant supérieur au solde disponible.
Eventix refuse le retrait.
Aucun montant supérieur au solde ne peut être retiré.
Règle métier

Un organisateur ne peut jamais retirer plus que son solde disponible.

13. Use Cases déclenchés automatiquement

Cette section regroupe les comportements métier qui ne sont pas initiés directement par un utilisateur.

UC-024 — Expirer une réservation

Déclencheur :

Expiration du délai d'une réservation PENDING.

Objectif :

Libérer une disponibilité lorsque la réservation n'a pas été finalisée dans le délai prévu.

Scénario
Eventix détecte qu'une réservation PENDING a dépassé son délai.
La réservation passe à EXPIRED.
La disponibilité correspondante est libérée.
Une éventuelle confirmation de paiement ultérieure suit le processus de réconciliation.
Règles métier
RM01
RM02
RM03
Résultat
PENDING
   ↓
EXPIRED
   ↓
Disponibilité libérée
UC-025 — Émettre un billet

Déclencheur :

Confirmation définitive du paiement pour un achat éligible.

Objectif :

Créer et rendre disponible le billet correspondant à l'achat finalisé.

Scénario nominal
Eventix constate que le paiement est confirmé.
L'achat est considéré comme finalisé.
Eventix émet le billet.
Le billet reçoit son contexte d'événement.
Le billet est rendu utilisable.
Le billet peut être récupéré par le participant.
Scénario d'exception — Échec d'émission
Le paiement est confirmé.
L'émission du billet échoue.
L'achat reste conservé.
Une nouvelle tentative d'émission peut être effectuée.
Aucun nouveau paiement ne doit être demandé.
Aucun deuxième achat ne doit être créé.
Aucun billet supplémentaire ne doit être créé pour la même transaction.
Règles métier
RM03
RM05
RM06
RM25
Traçabilité
B06
B07
US-007
UC-026 — Mettre à jour l'état d'un billet après contrôle

Déclencheur :

Validation réussie lors du contrôle d'accès.

Objectif :

Enregistrer qu'un billet a été utilisé et que l'accès a été autorisé.

Scénario
Le contrôle du billet est accepté.
L'accès est autorisé.
Le billet passe à USED.
La présence du participant peut être constatée.
Le contrôle est enregistré.
Règles métier
RM13
RM14
RM16
RM17
14. Relations include / extend

Les relations UML ne sont pas utilisées automatiquement.

Elles ne doivent être introduites que lorsqu'elles représentent une véritable relation comportementale.

Par exemple :

UC — Acheter un billet

peut contenir dans son scénario :

Réservation
Paiement
Émission

Cela ne signifie pas automatiquement :

Acheter un billet
    <<include>>
Réserver


Acheter un billet
    <<include>>
Payer


Acheter un billet
    <<include>>
Émettre

La décision de créer une relation include ou extend doit être justifiée par une véritable réutilisation comportementale ou une extension conditionnelle.

15. États métier utilisés dans les scénarios

Les états ne sont mentionnés que lorsqu'ils sont utiles à la compréhension du comportement.

Événement
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
Réservation
PENDING
CONFIRMED
EXPIRED
CANCELLED
Paiement
PENDING
CONFIRMED
FAILED
Billet
ISSUED
USED
CANCELLED
Remboursement
PENDING
PROCESSING
FAILED
COMPLETED
Retrait
PENDING
PROCESSING
FAILED
COMPLETED
Organisation
ACTIVE
UNDER_REVIEW
SUSPENDED
BANNED
16. Questions métier ouvertes

Lorsqu'un scénario dépend d'une décision non résolue, aucune décision implicite ne doit être introduite.

La forme attendue est :

> **À préciser**
>
> Ce comportement dépend d'une décision encore ouverte.
>
> Référence :
> `questions-metier-ouvertes.md`

Les scénarios concernés doivent rester exploitables jusqu'au point où l'information est connue.

17. Traçabilité

La traçabilité doit rester pertinente.

Le modèle cible est :

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

Des relations supplémentaires peuvent exister lorsque cela est pertinent :

Use Case ───────► Règle métier
Use Case ───────► Question ouverte

La matrice complète sera centralisée dans :

matrice-de-tracabilite.md

18. Relation avec les User Stories

La relation entre User Stories et Use Cases n'est pas nécessairement 1:1.

Plusieurs User Stories → un Use Case
US-001 ──┐
         ├── UC-001 — Découvrir un événement
US-002 ──┘
Une User Story → un Use Case
US-004
  ↓
UC-003 — Acheter un billet
Règle

Une User Story peut être reliée à un Use Case lorsque celui-ci décrit le comportement permettant de réaliser la valeur exprimée par la User Story.

19. Relation avec les exigences fonctionnelles

Les exigences fonctionnelles décrivent ce que le système doit permettre.

Les Use Cases décrivent comment le comportement métier se déroule du point de vue des interactions.

Une correspondance directe n'est donc pas obligatoire :

1 User Story
      ↓
1 ou plusieurs EF
      ↓
1 ou plusieurs scénarios
      ↓
1 Use Case

Les identifiants exacts des exigences fonctionnelles seront consolidés dans :

matrice-de-tracabilite.md

Aucun identifiant EF ne doit être inventé dans ce document.

20. Relation avec les critères d'acceptation

Les Use Cases décrivent le comportement attendu.

Les critères d'acceptation définissent les conditions permettant de vérifier que ce comportement est correctement satisfait.

Use Case
    ↓
Comportement attendu
    ↓
Critères d'acceptation
    ↓
Validation

Le détail des critères appartient exclusivement à :

criteres-d-acceptation.md

21. Relation avec les règles métier

Une règle métier n'est pas recopiée intégralement dans chaque Use Case.

Le Use Case indique seulement :

où la règle intervient ;
comment elle influence le scénario ;
son identifiant.

Exemple :

La réservation passe à EXPIRED lorsque le délai est dépassé.
[RM02]

La définition complète de la règle reste dans :

regles-metier.md

22. Faible couplage documentaire

La séparation des responsabilités est la suivante :

Besoins métier
    ↓
Pourquoi le métier a besoin de la capacité


Exigences fonctionnelles
    ↓
Ce que le système doit permettre


User Stories
    ↓
Valeur recherchée par l'acteur


Use Cases
    ↓
Déroulement du comportement métier


Critères d'acceptation
    ↓
Comment vérifier le comportement


Règles métier
    ↓
Contraintes et invariants


Matrice de traçabilité
    ↓
Relations entre les artefacts

Aucun document ne doit devenir le conteneur de tous les autres.

23. Hors périmètre MVP

Les éléments suivants ne font pas partie des scénarios de ce document :

vente physique ;
agents de vente ;
kiosques ;
synchronisation des ventes physiques ;
WhatsApp ;
Marketplace ;
revente de billets ;
Quiz Live ;
QR Cloud photos / vidéos ;
Reels ;
architectures offline avancées ;
synchronisation distribuée avancée.

Ils pourront faire l'objet de Use Cases dédiés lorsqu'ils entreront officiellement dans le périmètre produit.

24. Points de cohérence à préserver

Les Use Cases doivent toujours respecter les principes suivants.

24.1 Réservation ≠ paiement ≠ billet
Réservation
     ≠
Paiement
     ≠
Billet

L'état de l'un ne permet pas de déduire automatiquement l'état des autres.

24.2 Une disponibilité ne peut pas être attribuée deux fois
Disponibilité
      ↓
Réservation
      ↓
Billet
      ↓
Un propriétaire actif
24.3 Un paiement confirmé tardivement n'est pas ignoré
Paiement confirmé
        ↓
Réconciliation
        ↓
┌───────────────┐
│ Billet dispo  │ → Attribution
└───────────────┘


┌───────────────┐
│ Billet vendu  │ → Remboursement
└───────────────┘
24.4 Une confirmation de paiement ne produit qu'un seul effet métier
PAYMENT_SUCCESS
PAYMENT_SUCCESS
PAYMENT_SUCCESS
        ↓
Une seule opération métier
24.5 Un billet ne peut être utilisé avec succès qu'une seule fois
Billet valide
     ↓
Contrôle accepté
     ↓
USED
     ↓
Nouvelle tentative
     ↓
Refus
24.6 Billet ≠ présence
Billet valide
      ≠
Participant présent

La présence est constatée lors du contrôle.

24.7 Signalement ≠ fraude confirmée
Signalement
     ↓
Analyse
     ↓
Évaluation
     ↓
Mesure adaptée

Une sanction définitive ne doit pas être déduite automatiquement du simple signalement.

24.8 Paiement confirmé ≠ règlement organisateur
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
25. Principes de qualité

Un Use Case Eventix doit :

avoir un objectif métier identifiable ;
identifier son acteur principal ;
identifier ses acteurs secondaires lorsqu'ils interviennent ;
rester au niveau métier ;
ne pas contenir de détails techniques ;
avoir un scénario nominal compréhensible ;
documenter les alternatives pertinentes ;
documenter les exceptions supportées par les sources ;
référencer les règles métier pertinentes ;
signaler les décisions ouvertes ;
conserver une traçabilité utile ;
respecter le périmètre MVP ;
éviter les dépendances artificielles ;
préserver le faible couplage documentaire.
26. Statut du document
Document :
use-cases.md


Version :
1.0


Statut :
À valider


Périmètre :
Eventix MVP — Billetterie


Marché :
Cameroun


Méthode :
Use Cases orientés objectifs métier


Organisation :
Acteur principal → Domaine → Use Case


Identifiants :
UC-001, UC-002, ...


Principe transversal :
Faible couplage


Critères d'acceptation :
Document séparé


Traçabilité :
Matrice de traçabilité dédiée
27. Sources de référence
02-vision-produit/objectifs-produit.md
03-decouverte-du-metier/besoins-metier.md
03-decouverte-du-metier/contraintes-metier.md
03-decouverte-du-metier/regles-metier.md
03-decouverte-du-metier/processus-metier.md
03-decouverte-du-metier/user-journeys.md
03-decouverte-du-metier/questions-metier-ouvertes.md
04-analyse-des-besoins/exigences-fonctionnelles.md
04-analyse-des-besoins/exigences-non-fonctionnelles.md
04-analyse-des-besoins/user-stories.md
04-analyse-des-besoins/criteres-d-acceptation.md
04-analyse-des-besoins/matrice-de-tracabilite.md


**Point de cohérence important :** les sources actuelles indiquent explicitement que la **vente physique est hors MVP**, alors que certaines règles métier décrivent néanmoins son fonctionnement futur/éventuel. J'ai donc conservé cette capacité comme hors périmètre plutôt que de créer une contradiction dans les Use Cases. :contentReference[oaicite:4]{index=4} :contentReference[oaicite:5]{index=5}