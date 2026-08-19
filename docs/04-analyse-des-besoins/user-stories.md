# User Stories — Eventix

> **Phase :** 04 — Analyse des besoins  
> **Projet :** Eventix  
> **Périmètre :** MVP — Billetterie  
> **Marché :** Cameroun  
> **Statut :** Version de référence — à valider

---

# 1. Objectif

Ce document définit les User Stories du MVP Eventix.

Une User Story décrit un besoin ou une capacité du système du point de vue d'un acteur :

> **En tant que [acteur], je veux [besoin], afin de [valeur recherchée].**

Les User Stories sont dérivées des besoins métier et des exigences fonctionnelles.

Elles ne décrivent pas :

- l'architecture technique ;
- les choix technologiques ;
- les APIs ;
- le modèle de données ;
- les critères techniques d'implémentation.

---

# 2. Principes de rédaction

Les User Stories Eventix respectent les principes suivants :

1. Une User Story représente une **valeur utilisateur**.
2. Le découpage ne suit pas les étapes techniques internes.
3. Une User Story peut couvrir plusieurs exigences fonctionnelles lorsqu'elles contribuent à une même valeur.
4. Une dépendance métier nécessaire est autorisée mais doit rester explicite et minimisée.
5. Les systèmes externes ne portent pas de User Stories.
6. Les critères d'acceptation sont définis dans `criteres-d-acceptation.md`.
7. Les exigences non fonctionnelles sont gérées dans `exigences-non-fonctionnelles.md`.
8. Les User Stories du présent document concernent uniquement le MVP.
9. Une décision métier non résolue reste explicitement ouverte.
10. Le principe de faible couplage doit être préservé.

---

# 3. Convention d'identification

Les User Stories utilisent un identifiant global et stable :

```text
US-001
US-002
US-003
...
L'acteur et le domaine ne sont pas encodés dans l'identifiant.

Cela permet de conserver un identifiant stable même lorsque l'organisation des domaines évolue.

4. Priorisation

La priorité produit utilise la méthode MoSCoW.

Priorité	Signification
MUST	Indispensable au MVP
SHOULD	Important mais non bloquant pour le MVP
COULD	Souhaitable si capacité disponible
WON'T	Hors MVP actuel

La priorité de l'exigence fonctionnelle correspondante reste une information distincte de traçabilité.

5. Acteurs concernés

Les User Stories sont organisées par acteur.

Les acteurs concernés sont :

Participant
Organisateur
Agent de contrôle
Administrateur

Les systèmes externes, tels que les prestataires de paiement ou les canaux de notification, sont traités dans les Use Cases et les documents d'intégration, et non comme porteurs de User Stories.

6. User Stories — Participant
US-001 — Découvrir un événement

Acteur : Participant
Domaine : Découverte
Priorité produit : MUST
Besoin métier : B05

En tant que participant,
je veux rechercher et consulter les événements disponibles,
afin de trouver une activité à laquelle je souhaite participer.

Valeur :

Permettre au participant de découvrir les événements disponibles et d'accéder à leurs informations.

Dépendances :

Aucune dépendance métier forte.
Les informations de l'événement doivent être disponibles à la consultation.
US-002 — Consulter les détails d'un événement

Acteur : Participant
Domaine : Découverte
Priorité produit : MUST
Besoin métier : B05

En tant que participant,
je veux consulter les informations détaillées d'un événement,
afin de décider si je souhaite y participer.

Valeur :

Permettre une décision informée avant l'obtention d'un billet.

Dépendances :

US-001
US-003 — Obtenir un billet gratuit

Acteur : Participant
Domaine : Billetterie
Priorité produit : MUST
Besoin métier : B06

En tant que participant,
je veux obtenir un billet pour un événement gratuit,
afin de pouvoir participer à l'événement.

Valeur :

Permettre l'accès à un événement gratuit sans paiement.

Dépendances :

disponibilité ;
événement configuré ;
conditions d'accès définies.
US-004 — Acheter un billet

Acteur : Participant
Domaine : Billetterie
Priorité produit : MUST
Besoin métier : B06

En tant que participant,
je veux acheter un billet pour un événement payant,
afin de pouvoir participer à l'événement.

Valeur :

Permettre l'achat officiel d'un billet.

Dépendances :

disponibilité ;
réservation ;
paiement ;
émission du billet.

Règles associées :

RM01
RM03
RM05
RM06
RM07
US-005 — Réserver temporairement une disponibilité

Acteur : Participant
Domaine : Réservation
Priorité produit : MUST
Besoin métier : B14

En tant que participant,
je veux réserver temporairement une disponibilité pendant mon achat,
afin d'éviter qu'elle soit attribuée à quelqu'un d'autre pendant mon processus d'achat.

Valeur :

Protéger temporairement la disponibilité pendant la finalisation de l'achat.

Règles associées :

RM01
RM02
RM03

Dépendance :

Cette capacité fait partie du parcours d'achat mais reste distincte du paiement.
US-006 — Finaliser un paiement

Acteur : Participant
Domaine : Paiement
Priorité produit : MUST
Besoin métier : B06 / B16

En tant que participant,
je veux finaliser le paiement de mon billet,
afin de terminer mon achat.

Valeur :

Transformer une intention d'achat en opération financière confirmée.

Règles associées :

RM03
RM05
RM07
US-007 — Récupérer son billet

Acteur : Participant
Domaine : Billetterie
Priorité produit : MUST
Besoin métier : B07

En tant que participant,
je veux récupérer mon billet depuis Eventix,
afin de pouvoir le présenter lors de l'accès à l'événement.

Valeur :

Permettre au participant de disposer de son billet après son obtention.

Canaux MVP :

plateforme ;
téléchargement ;
email.

Hors périmètre :

WhatsApp.
US-008 — Utiliser son billet lors de l'événement

Acteur : Participant
Domaine : Accès
Priorité produit : MUST
Besoin métier : B21 / B22

En tant que participant,
je veux présenter mon billet lors de l'accès,
afin d'être autorisé à entrer lorsque mon billet est valide.

Valeur :

Permettre l'accès à l'événement sur la base d'un billet valide.

Dépendance :

US-007

Règles associées :

RM13
RM14
RM17
US-009 — Transférer un billet lorsque cela est autorisé

Acteur : Participant
Domaine : Billetterie
Priorité produit : À PRÉCISER

En tant que participant,
je veux pouvoir transférer mon billet lorsque les conditions de l'événement l'autorisent,
afin qu'un autre participant puisse en devenir le propriétaire.

Besoin métier : B27

Statut :

À PRÉCISER — dépend des conditions de transfert applicables.

Les règles détaillées du transfert doivent être confirmées avant de définir précisément le comportement.

Règles associées :

conservation de l'historique ;
traçabilité ;
restrictions éventuelles de l'événement.
US-010 — Signaler une activité suspecte

Acteur : Participant
Domaine : Sécurité
Priorité produit : SHOULD
Besoin métier : B30

En tant que participant,
je veux signaler un événement ou une activité suspecte,
afin que la situation puisse être analysée.

Valeur :

Contribuer à la confiance et à la détection des risques.

Règle importante :

Signalement
    ≠
Fraude confirmée
7. User Stories — Organisateur
US-011 — Créer un événement

Acteur : Organisateur
Domaine : Événement
Priorité produit : MUST
Besoin métier : B01

En tant qu'organisateur,
je veux créer un événement,
afin de préparer sa mise en vente.

Valeur :

Créer le support métier nécessaire à l'organisation de l'événement.

US-012 — Configurer un événement

Acteur : Organisateur
Domaine : Événement
Priorité produit : MUST
Besoin métier : B02

En tant qu'organisateur,
je veux configurer mon événement,
afin de définir les informations et paramètres nécessaires à sa billetterie.

Éléments concernés notamment :

informations générales ;
date ;
lieu ;
espaces ;
capacité ;
catégories de billets ;
prix ;
périodes de vente ;
paramètres de billetterie.

Règles associées :

RM23
RM24
US-013 — Publier un événement

Acteur : Organisateur
Domaine : Événement
Priorité produit : MUST
Besoin métier : B03

En tant qu'organisateur,
je veux publier mon événement,
afin de le rendre disponible aux participants selon les conditions définies.

Valeur :

Rendre l'événement visible et exploitable dans le parcours de participation.

Dépendances :

US-011
US-012
US-014 — Consulter ses événements

Acteur : Organisateur
Domaine : Événement
Priorité produit : MUST
Besoin métier : B04

En tant qu'organisateur,
je veux consulter mes événements,
afin de suivre leur état et leur activité.

Valeur :

Centraliser la gestion des événements de l'organisateur.

US-015 — Rechercher et filtrer ses événements

Acteur : Organisateur
Domaine : Événement
Priorité produit : SHOULD
Besoin métier : B04

En tant qu'organisateur,
je veux rechercher et filtrer mes événements,
afin de retrouver rapidement celui que je souhaite gérer.

Valeur :

Réduire la complexité de gestion lorsque plusieurs événements existent.

US-016 — Configurer les espaces et disponibilités

Acteur : Organisateur
Domaine : Espaces / Billetterie
Priorité produit : MUST
Besoin métier : B08

En tant qu'organisateur,
je veux configurer les espaces et les disponibilités de mon événement,
afin de définir ce qui peut être attribué aux participants.

Valeur :

Représenter correctement la capacité et les disponibilités de l'événement.

Règles associées :

une disponibilité ne peut pas être attribuée deux fois ;
la capacité ne peut pas être réduite sous le nombre de billets déjà attribués.
US-017 — Gérer les prix

Acteur : Organisateur
Domaine : Billetterie
Priorité produit : MUST
Besoin métier : B20

En tant qu'organisateur,
je veux définir et modifier les prix applicables aux ventes futures,
afin d'adapter la tarification de mon événement.

Valeur :

Contrôler la tarification future.

Règle associée :

Le prix historiquement payé par un participant ne doit pas être modifié par une modification future du prix.

US-018 — Suivre les participants

Acteur : Organisateur
Domaine : Participants
Priorité produit : MUST
Besoin métier : B25

En tant qu'organisateur,
je veux suivre les participants associés à mon événement,
afin de connaître la situation de la participation.

Distinctions à conserver :

Billet vendu
    ≠
Participant attendu
    ≠
Participant présent
US-019 — Suivre l'activité de l'événement

Acteur : Organisateur
Domaine : Pilotage
Priorité produit : MUST
Besoin métier : B26

En tant qu'organisateur,
je veux suivre l'activité de mon événement,
afin de connaître sa situation opérationnelle.

Informations notamment concernées :

ventes ;
entrées ;
participants présents ;
disponibilité ;
activité des points physiques.
US-020 — Analyser les performances de l'événement

Acteur : Organisateur
Domaine : Statistiques
Priorité produit : SHOULD
Besoin métier : B31

En tant qu'organisateur,
je veux analyser les performances de mon événement,
afin d'évaluer ses résultats.

Indicateurs notamment concernés :

ventes globales ;
ventes par canal ;
ventes par point physique ;
ventes par catégorie ;
ventes par période ;
taux de présence ;
chiffre d'affaires ;
remboursements.
US-021 — Consulter des indicateurs d'aide à la décision

Acteur : Organisateur
Domaine : Statistiques
Priorité produit : COULD
Besoin métier : B32

En tant qu'organisateur,
je veux identifier des tendances dans les données de mon événement,
afin d'améliorer mes décisions commerciales et opérationnelles.

Statut :

Les recommandations automatisées constituent une évolution du besoin ; leur niveau exact dans le MVP reste à confirmer.

US-022 — Gérer un report d'événement

Acteur : Organisateur
Domaine : Événement
Priorité produit : MUST
Besoin métier : B19

En tant qu'organisateur,
je veux gérer le report de mon événement,
afin que les billets déjà achetés puissent être traités conformément aux règles applicables.

Règle associée :

Un billet déjà acheté est conservé lors d'un report lorsque les conditions permettent son transfert vers la nouvelle date.

Dépendance :

Le traitement des cas incompatibles avec la nouvelle configuration doit suivre les règles métier validées.

US-023 — Gérer l'annulation d'un événement

Acteur : Organisateur
Domaine : Événement
Priorité produit : MUST
Besoin métier : B18

En tant qu'organisateur,
je veux gérer l'annulation de mon événement,
afin que ses conséquences sur les billets, paiements, participants, ventes et remboursements soient traitées.

Dépendances :

Billetterie
Paiement
Remboursement
Finance

Règle associée :

Les billets concernés deviennent éligibles au remboursement lorsque l'annulation est imputable à l'organisateur, selon le processus applicable.

US-024 — Consulter le règlement financier

Acteur : Organisateur
Domaine : Finance
Priorité produit : MUST
Besoin métier : B33

En tant qu'organisateur,
je veux suivre le cycle de règlement de mes ventes,
afin de connaître la situation des fonds qui me sont destinés.

Cycle :

Paiement participant
        ↓
Fonds en attente
        ↓
Réconciliation
        ↓
Événement terminé
        ↓
Règlement organisateur

Statut :

Les conditions financières précises restent à préciser.

8. User Stories — Agent de contrôle
US-025 — Contrôler un billet

Acteur : Agent de contrôle
Domaine : Contrôle d'accès
Priorité produit : MUST
Besoin métier : B21

En tant qu'agent de contrôle,
je veux scanner et vérifier un billet,
afin de déterminer s'il peut être utilisé pour accéder à l'événement.

Valeur :

Permettre une décision fiable d'accès.

Contrôles notamment concernés :

billet valide ;
billet déjà utilisé ;
billet correspondant au bon événement ;
billet annulé.
US-026 — Refuser un billet déjà utilisé

Acteur : Agent de contrôle
Domaine : Contrôle d'accès
Priorité produit : MUST
Besoin métier : B22

En tant qu'agent de contrôle,
je veux qu'un billet déjà utilisé soit refusé,
afin d'empêcher sa réutilisation.

Règles associées :

RM13
RM14
US-027 — Enregistrer un contrôle

Acteur : Agent de contrôle
Domaine : Contrôle d'accès
Priorité produit : MUST
Besoin métier : B23

En tant qu'agent de contrôle,
je veux que mes contrôles soient enregistrés,
afin que les entrées puissent être suivies et auditées.

Contexte à conserver notamment :

billet ;
événement ;
contrôleur ;
point d'entrée ;
date et heure ;
résultat.
US-028 — Être affecté à un point d'entrée

Acteur : Agent de contrôle
Domaine : Contrôle d'accès
Priorité produit : MUST
Besoin métier : B24

En tant qu'agent de contrôle,
je veux être affecté à un point d'entrée,
afin que le contexte de mes contrôles soit identifiable.

Règle importante :

L'affectation à un point d'entrée ne constitue pas automatiquement une restriction sur les catégories de billets que l'agent peut valider.

US-029 — Contrôler avec plusieurs scanners lorsque l'état est fiable

Acteur : Agent de contrôle
Domaine : Contrôle d'accès
Priorité produit : MUST

En tant qu'agent de contrôle,
je veux pouvoir travailler avec plusieurs scanners lorsque leur état est partagé et synchronisé de manière fiable,
afin de maintenir un débit d'entrée adapté.

Règle associée :

Plusieurs scanners sont autorisés lorsque la synchronisation est fiable.

US-030 — Passer en mode mono-scanner lorsque la synchronisation devient non fiable

Acteur : Agent de contrôle
Domaine : Contrôle d'accès
Priorité produit : MUST

En tant qu'agent de contrôle,
je veux que le système bascule vers un seul scanner lorsque la synchronisation entre scanners n'est plus fiable,
afin de préserver la fiabilité des validations.

Trade-off retenu :

Plusieurs scanners synchronisés
        ↓
Rapidité élevée + fiabilité élevée


Synchronisation non fiable
        ↓
Un seul scanner
        ↓
Rapidité réduite + fiabilité préservée

Principe :

La fiabilité prime sur la rapidité lorsque les deux entrent en conflit.

9. User Stories — Administrateur
US-031 — Vérifier un organisateur ou un événement

Acteur : Administrateur
Domaine : Confiance / Sécurité
Priorité produit : MUST
Besoin métier : B28

En tant qu'administrateur,
je veux vérifier les organisateurs, organisations et événements,
afin de réduire les risques de faux événements et de fraude.

Valeur :

Préserver la confiance dans la plateforme.

US-032 — Gérer le niveau de risque

Acteur : Administrateur
Domaine : Sécurité
Priorité produit : MUST
Besoin métier : B29

En tant qu'administrateur,
je veux adapter les mesures au niveau de risque détecté,
afin de traiter les situations de manière proportionnée.

Modèle métier :

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
US-033 — Analyser un signalement

Acteur : Administrateur
Domaine : Sécurité
Priorité produit : MUST
Besoin métier : B30

En tant qu'administrateur,
je veux analyser les signalements reçus,
afin de déterminer les mesures appropriées.

Règle fondamentale :

Signalement
    ≠
Fraude confirmée

Une suspicion doit pouvoir être analysée avant une sanction définitive.

US-034 — Suspendre ou limiter une opération à risque

Acteur : Administrateur
Domaine : Sécurité
Priorité produit : MUST
Besoin métier : B29

En tant qu'administrateur,
je veux pouvoir limiter ou suspendre une opération lorsque le niveau de risque le justifie,
afin de protéger Eventix et ses utilisateurs.

Règle associée :

Les décisions sensibles doivent être proportionnées et traçables.

US-035 — Auditer les décisions sensibles

Acteur : Administrateur
Domaine : Sécurité / Audit
Priorité produit : MUST

En tant qu'administrateur,
je veux retrouver l'historique des décisions sensibles,
afin de pouvoir les auditer et comprendre leur contexte.

Données concernées notamment :

action ;
acteur ;
contexte ;
événement ;
date ;
résultat.
10. User Stories — Gestion financière

Certaines capacités financières sont des capacités internes d'Eventix. Elles peuvent être portées par l'administrateur lorsqu'elles correspondent à une capacité utilisateur nécessaire au fonctionnement du MVP.

US-036 — Gérer les remboursements

Acteur : Administrateur
Domaine : Finance
Priorité produit : MUST
Besoin métier : B17

En tant qu'administrateur,
je veux gérer les remboursements lorsqu'ils sont éligibles,
afin de traiter les obligations financières résultant des événements et transactions.

Règles associées :

RM19
RM20
RM21

Propriété importante :

Un même paiement ne doit pas faire l'objet de plusieurs remboursements pour une même cause.

US-037 — Réconcilier un paiement tardif

Acteur : Administrateur / système métier
Domaine : Paiement / Réconciliation
Priorité produit : MUST
Besoin métier : B16

En tant qu'acteur responsable du traitement financier,
je veux pouvoir réconcilier une confirmation de paiement arrivée après l'expiration d'une réservation,
afin de déterminer si le billet peut être attribué ou si un remboursement doit être effectué.

Règles associées :

RM02
RM03
RM04
RM05

Principe :

L'expiration de la réservation ne signifie pas automatiquement que le paiement a échoué.

US-038 — Suivre les opérations financières

Acteur : Administrateur
Domaine : Finance
Priorité produit : MUST

En tant qu'administrateur,
je veux consulter l'historique des opérations financières importantes,
afin de pouvoir assurer leur suivi, leur réconciliation et leur audit.

Opérations concernées :

paiements ;
remboursements ;
retraits.
11. User Stories — Point physique

Note de périmètre : les points physiques apparaissent dans les besoins métier et les règles métier actuels. Leur statut exact dans le MVP doit rester aligné avec la version validée du périmètre avant la finalisation de la matrice de traçabilité.

US-039 — Réaliser une vente depuis un point physique

Acteur : Agent de vente / acteur autorisé
Domaine : Vente
Priorité produit : À PRÉCISER

En tant qu'acteur autorisé d'un point physique,
je veux enregistrer une vente de billet depuis Eventix,
afin de vendre un billet tout en utilisant la disponibilité centralisée.

Règles associées :

RM08
RM09
RM10
RM11

Statut :

À PRÉCISER dans le périmètre MVP.

US-040 — Enregistrer une vente physique avec son contexte

Acteur : Agent de vente / acteur autorisé
Domaine : Vente
Priorité produit : À PRÉCISER

En tant qu'acteur autorisé d'un point physique,
je veux que ma vente soit enregistrée avec son contexte,
afin qu'elle puisse être suivie et auditée.

Contexte notamment concerné :

point physique ;
agent ;
événement ;
billet ;
montant ;
mode de paiement ;
contexte de vente.
12. User Stories transversales

Certaines capacités ne doivent pas être artificiellement attribuées à plusieurs acteurs.

Elles constituent des propriétés du fonctionnement métier et seront couvertes par les Use Cases, exigences fonctionnelles et critères d'acceptation.

12.1 Idempotence des paiements

La règle :

Même confirmation de paiement
        ↓
Un seul effet métier

ne constitue pas une User Story indépendante.

Elle contraint notamment :

US-004 ;
US-006 ;
US-037.
12.2 Cohérence de l'inventaire

La règle :

Une disponibilité
        ↓
Une réservation
        ↓
Un billet
        ↓
Un propriétaire actif

ne constitue pas une User Story indépendante.

Elle contraint notamment :

US-004 ;
US-005 ;
US-016 ;
US-025.
12.3 Préservation de l'historique

La préservation de l'historique est une contrainte transversale.

Elle concerne notamment :

billets ;
prix ;
paiements ;
remboursements ;
contrôles ;
décisions de sécurité.

Elle sera traitée dans les exigences non fonctionnelles et les critères d'acceptation.

13. User Stories dépendantes de décisions ouvertes

Les éléments suivants ne doivent pas être artificiellement finalisés :

User Story	Décision concernée	Statut
US-009	Conditions exactes de transfert	À préciser
US-021	Niveau exact des recommandations	À préciser
US-024	Conditions précises du règlement	À préciser
US-039	Statut exact de la vente physique dans le MVP	À préciser
US-040	Périmètre exact des acteurs de vente physique	À préciser

Ces décisions doivent rester alignées avec :

03-decouverte-du-metier/questions-metier-ouvertes.md

14. Hors périmètre

Les fonctionnalités suivantes ne doivent pas être transformées en User Stories du MVP actuel :

Marketplace de revente ;
Quiz Live ;
QR Cloud photos / vidéos ;
Reels ;
autres services événementiels ;
mode offline avancé ;
synchronisation distribuée avancée.

Ces fonctionnalités feront l'objet de User Stories propres lorsqu'elles entreront officiellement dans le périmètre produit.

15. Matrice synthétique des User Stories
ID	Acteur	Domaine	Besoin	Priorité
US-001	Participant	Découverte	Découvrir un événement	MUST
US-002	Participant	Découverte	Consulter les détails	MUST
US-003	Participant	Billetterie	Obtenir un billet gratuit	MUST
US-004	Participant	Billetterie	Acheter un billet	MUST
US-005	Participant	Réservation	Réserver temporairement	MUST
US-006	Participant	Paiement	Finaliser un paiement	MUST
US-007	Participant	Billetterie	Récupérer son billet	MUST
US-008	Participant	Accès	Utiliser son billet	MUST
US-009	Participant	Billetterie	Transférer un billet	À préciser
US-010	Participant	Sécurité	Signaler une activité suspecte	SHOULD
US-011	Organisateur	Événement	Créer un événement	MUST
US-012	Organisateur	Événement	Configurer un événement	MUST
US-013	Organisateur	Événement	Publier un événement	MUST
US-014	Organisateur	Événement	Consulter ses événements	MUST
US-015	Organisateur	Événement	Rechercher / filtrer	SHOULD
US-016	Organisateur	Espaces	Configurer les espaces	MUST
US-017	Organisateur	Billetterie	Gérer les prix	MUST
US-018	Organisateur	Participants	Suivre les participants	MUST
US-019	Organisateur	Pilotage	Suivre l'activité	MUST
US-020	Organisateur	Statistiques	Analyser les performances	SHOULD
US-021	Organisateur	Statistiques	Consulter les tendances	COULD
US-022	Organisateur	Événement	Gérer un report	MUST
US-023	Organisateur	Événement	Gérer une annulation	MUST
US-024	Organisateur	Finance	Suivre le règlement	MUST
US-025	Agent de contrôle	Contrôle	Contrôler un billet	MUST
US-026	Agent de contrôle	Contrôle	Refuser un billet utilisé	MUST
US-027	Agent de contrôle	Contrôle	Enregistrer un contrôle	MUST
US-028	Agent de contrôle	Contrôle	Être affecté à une entrée	MUST
US-029	Agent de contrôle	Contrôle	Utiliser plusieurs scanners fiables	MUST
US-030	Agent de contrôle	Contrôle	Basculer en mono-scanner	MUST
US-031	Administrateur	Confiance	Vérifier organisateur / événement	MUST
US-032	Administrateur	Sécurité	Gérer le niveau de risque	MUST
US-033	Administrateur	Sécurité	Analyser un signalement	MUST
US-034	Administrateur	Sécurité	Limiter / suspendre	MUST
US-035	Administrateur	Audit	Auditer les décisions	MUST
US-036	Administrateur	Finance	Gérer les remboursements	MUST
US-037	Administrateur	Réconciliation	Réconcilier un paiement tardif	MUST
US-038	Administrateur	Finance	Suivre les opérations financières	MUST
US-039	Agent de vente	Vente physique	Réaliser une vente	À préciser
US-040	Agent de vente	Vente physique	Tracer une vente	À préciser
16. Règles de dépendance

Les dépendances entre User Stories sont volontairement limitées.

Exemple :

US-011
Créer un événement
      ↓
US-012
Configurer un événement
      ↓
US-013
Publier un événement

Cette dépendance est métier et légitime.

À l'inverse, nous évitons :

US-A
    ↓
US-B
    ↓
US-C
    ↓
US-D
    ↓
US-E

lorsqu'une telle chaîne résulte uniquement d'une organisation technique.

17. Relation avec les exigences fonctionnelles

Une User Story peut être liée :

à une seule exigence fonctionnelle ;
à plusieurs exigences fonctionnelles ;
exceptionnellement à une exigence fonctionnelle partagée avec une autre User Story.

La relation n'est donc pas obligatoirement :

1 User Story = 1 Exigence

mais peut être :

             ┌── EF
             │
US ──────────┼── EF
             │
             └── EF

ou :

US ─────── EF
US ─────── EF

La relation exacte sera consolidée dans :

matrice-de-tracabilite.md

18. Relation avec les exigences non fonctionnelles

Les exigences non fonctionnelles ne sont pas directement incorporées dans les User Stories.

Par exemple :

US-025 — Contrôler un billet
        │
        ├── besoin utilisateur
        │
        └── critères d'acceptation

Les qualités telles que :

performance ;
fiabilité ;
sécurité ;
disponibilité ;
traçabilité

restent définies dans :

exigences-non-fonctionnelles.md

Elles pourront ensuite être reliées aux User Stories dans la matrice de traçabilité lorsqu'une relation est pertinente.

19. Relation avec les critères d'acceptation

La responsabilité de chaque document est séparée :

Besoins métier
       ↓
Exigences fonctionnelles
       ↓
User Stories
       ↓
Critères d'acceptation
       ↓
Validation

Une User Story exprime :

Pourquoi l'acteur a besoin de la capacité.

Les critères d'acceptation expriment :

Comment vérifier que la capacité est correctement satisfaite.

20. Relation avec les Use Cases

Les User Stories expriment la valeur utilisateur.

Les Use Cases détailleront ensuite les interactions nécessaires pour réaliser cette valeur.

Exemple :

US-004
Acheter un billet
      ↓
UC — Acheter un billet
      ↓
Scénarios
      ├── réservation
      ├── paiement
      ├── confirmation
      ├── émission
      └── exceptions

La séparation permet de ne pas transformer les User Stories en spécifications techniques.

21. Principe de faible couplage

Le faible couplage est appliqué à trois niveaux.

21.1 Entre acteurs

Une User Story appartient à l'acteur qui possède le besoin.

Elle n'est pas artificiellement partagée entre plusieurs acteurs.

21.2 Entre capacités

Une capacité indépendante possède sa propre User Story.

Exemple :

Créer un événement
        ≠
Publier un événement
21.3 Entre documents

Chaque artefact conserve sa responsabilité :

Besoins métier
    → Pourquoi le métier a besoin de quelque chose


Exigences fonctionnelles
    → Ce que le système doit permettre


User Stories
    → Valeur pour l'acteur


Use Cases
    → Interaction détaillée


Critères d'acceptation
    → Vérification


ENF
    → Qualités et contraintes


Matrice de traçabilité
    → Relations entre les artefacts
22. Règles de qualité

Une User Story Eventix doit :

avoir un acteur clairement identifié ;
exprimer une valeur utilisateur ;
être suffisamment précise pour être vérifiable ;
éviter les détails techniques ;
éviter les responsabilités d'un autre acteur ;
conserver ses dépendances métier nécessaires ;
éviter les dépendances artificielles ;
être reliée à une source métier ;
respecter le périmètre MVP ;
ne pas transformer une question ouverte en décision ;
pouvoir être reliée ultérieurement à des critères d'acceptation.
23. Statut du document
Document : user-stories.md


Version : 1.0
Statut : À valider par l'équipe


Périmètre :
MVP Eventix


Méthode :
User Stories orientées valeur utilisateur


Priorisation :
MoSCoW


Identifiants :
US-001, US-002, ...


Principe architectural transversal :
Faible couplage


Critères d'acceptation :
Document séparé


Use Cases :
Document séparé


Matrice de traçabilité :
Document séparé
24. Sources de référence
02-vision-produit/objectifs-produit.md
03-decouverte-du-metier/besoins-metier.md
03-decouverte-du-metier/contraintes-metier.md
03-decouverte-du-metier/regles-metier.md
03-decouverte-du-metier/processus-metier.md
03-decouverte-du-metier/user-journeys.md
03-decouverte-du-metier/questions-metier-ouvertes.md
04-analyse-des-besoins/exigences-fonctionnelles.md
04-analyse-des-besoins/exigences-non-fonctionnelles.md


**Point important :** j’ai volontairement laissé certaines références exactes aux **EF** à consolider dans la matrice plutôt que d'inventer des identifiants d'exigences fonctionnelles que je ne peux pas vérifier dans la version actuellement disponible du fichier. Les User Stories sont donc traçables dès maintenant par les **besoins métier (`Bxx`) et règles métier (`RMxx`)**, puis pourront être reliées aux EF validées.