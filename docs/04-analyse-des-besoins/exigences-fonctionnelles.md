# Exigences fonctionnelles — Eventix

> **Phase :** 04 — Analyse des besoins  
> **Projet :** Eventix  
> **Périmètre :** MVP  
> **Marché initial :** Cameroun  
> **Statut :** Version de référence — à valider par l'équipe

---

# 1. Objectif

Ce document définit les exigences fonctionnelles auxquelles Eventix doit répondre dans son MVP.

Il transforme les besoins métier, les parcours utilisateurs, les processus métier et les règles métier en exigences :

- identifiables ;
- suffisamment précises ;
- vérifiables ;
- traçables ;
- indépendantes des choix technologiques.

Il répond à la question :

> **Que doit permettre Eventix ?**

Il ne définit pas encore :

- l'architecture technique ;
- les technologies ;
- les API ;
- les bases de données ;
- les microservices ;
- l'infrastructure ;
- les mécanismes techniques d'implémentation.

Ces éléments seront traités dans les phases ultérieures.

---

# 2. Sources de référence

Les exigences sont dérivées principalement de :

- `03-decouverte-du-metier/besoins-metier.md`
- `03-decouverte-du-metier/user-journeys.md`
- `03-decouverte-du-metier/regles-metier.md`
- `03-decouverte-du-metier/processus-metier.md`
- `03-decouverte-du-metier/questions-metier-ouvertes.md`

Les décisions prises au cours de l'analyse sont également prises en compte lorsqu'elles ont déjà été validées par l'équipe.

---

# 3. Principes de construction

## 3.1 Faible couplage

Les exigences fonctionnelles doivent respecter le principe de **faible couplage**.

Chaque responsabilité métier doit être clairement délimitée.

Une exigence ne doit pas mélanger inutilement plusieurs responsabilités.

### Exemple à éviter

```text
Lorsqu'un paiement est confirmé,
le système doit créer le billet,
envoyer l'email,
mettre à jour les statistiques
et rendre les fonds disponibles à l'organisateur.


Cette formulation mélange plusieurs responsabilités.

Formulation retenue
Paiement
    ↓
Paiement confirmé


Billetterie
    ↓
Émission du billet


Distribution
    ↓
Mise à disposition du billet


Statistiques
    ↓
Observation de l'activité


Finance
    ↓
Gestion des fonds

Les domaines peuvent donc collaborer sans que l'un ne prenne en charge les responsabilités internes des autres.

3.2 Faible couplage ≠ architecture imposée

Le faible couplage est appliqué au niveau des responsabilités métier.

Il ne signifie pas que chaque domaine doit nécessairement devenir :

un microservice ;
une application ;
une base de données indépendante.

La décision d'architecture sera prise ultérieurement.

3.3 Séparation des cycles métier

Eventix doit conserver les distinctions suivantes :

Réservation
     ≠
Paiement
     ≠
Billet
     ≠
Présence
     ≠
Règlement financier

Cette séparation constitue un principe fondamental du modèle métier.

3.4 Une exigence = une responsabilité principale

Une exigence peut avoir des dépendances métier, mais elle ne doit pas absorber les responsabilités des domaines dont elle dépend.

3.5 Les questions ouvertes ne sont pas transformées en décisions

Lorsqu'une exigence dépend d'une décision qui n'est pas encore prise, elle est explicitement marquée :

À PRÉCISER — voir questions-metier-ouvertes.md.

4. Convention d'identification

Chaque exigence possède un identifiant unique :

EF-001
EF-002
EF-003
...

Les priorités utilisées sont :

Priorité	Signification
CRITICAL	Indispensable au fonctionnement fondamental du MVP
HIGH	Importante pour le MVP
MEDIUM	Utile mais non bloquante
LOW	Secondaire
5. Domaine — Gestion du compte participant
EF-001 — Créer un compte participant

Acteur : Participant
Priorité : CRITICAL

Eventix doit permettre à un participant de créer un compte lorsqu'une identification devient nécessaire.

Le parcours ne doit pas obliger l'utilisateur à créer un compte avant toute découverte d'événement.

EF-002 — Créer un compte avec un minimum d'informations

Acteur : Participant
Priorité : CRITICAL

Eventix doit permettre la création minimale d'un compte avec :

un email ou un numéro de téléphone ;
un mot de passe.

Les informations complémentaires peuvent être ajoutées ultérieurement.

EF-003 — S'authentifier

Acteur : Participant
Priorité : CRITICAL

Eventix doit permettre au participant de s'authentifier avec :

email + mot de passe ;
ou téléphone + mot de passe.
EF-004 — Consulter son historique

Acteur : Participant
Priorité : HIGH

Eventix doit permettre au participant de consulter les informations pertinentes relatives à son activité, notamment :

billets ;
achats ;
réservations ;
remboursements ;
événements passés.

Les informations internes de sécurité et de fraude ne doivent pas être exposées.

6. Domaine — Capacités organisateur
EF-005 — Accéder aux capacités d'organisateur

Acteur : Utilisateur
Priorité : CRITICAL

Eventix doit permettre à un compte utilisateur autorisé d'obtenir les capacités nécessaires à l'organisation d'événements.

EF-006 — Utiliser un même compte comme participant et organisateur

Acteur : Utilisateur
Priorité : HIGH

Eventix doit permettre à un même compte utilisateur d'exercer à la fois les capacités de participant et d'organisateur lorsqu'il y est autorisé.

EF-007 — Consulter l'historique organisateur

Acteur : Organisateur
Priorité : HIGH

Eventix doit permettre à l'organisateur de consulter son historique métier, notamment :

événements ;
ventes ;
billets ;
remboursements ;
annulations ;
revenus ;
opérations financières pertinentes.
7. Domaine — Gestion des événements
EF-008 — Créer un événement

Acteur : Organisateur
Priorité : CRITICAL

Eventix doit permettre à l'organisateur de créer un événement.

Un nouvel événement doit commencer dans un état ne permettant pas encore sa publication publique.

EF-009 — Enregistrer un événement comme brouillon

Acteur : Organisateur
Priorité : CRITICAL

Eventix doit permettre à l'organisateur de travailler sur un événement avant sa soumission.

EF-010 — Configurer un événement

Acteur : Organisateur
Priorité : CRITICAL

Eventix doit permettre de configurer notamment :

nom ;
description ;
date ;
heure ;
lieu ;
capacité ;
catégories de billets ;
prix ;
périodes de vente ;
paramètres nécessaires à la billetterie.
EF-011 — Modifier un événement

Acteur : Organisateur
Priorité : HIGH

Eventix doit permettre à l'organisateur de modifier un événement conformément à son état et aux règles métier applicables.

Une modification future ne doit pas réécrire l'historique des opérations déjà réalisées.

EF-012 — Soumettre un événement

Acteur : Organisateur
Priorité : CRITICAL

Eventix doit permettre à l'organisateur de soumettre un événement prêt à être vérifié.

EF-013 — Vérifier un événement

Acteur : Eventix
Priorité : CRITICAL

Eventix doit pouvoir vérifier un événement avant sa publication.

La vérification doit notamment contribuer à réduire les risques liés :

aux événements fictifs ;
aux faux événements ;
aux événements frauduleux.
EF-014 — Vérifier un organisateur

Acteur : Eventix
Priorité : CRITICAL

Eventix doit pouvoir vérifier la crédibilité d'un organisateur ou d'une organisation avant ou pendant son activité.

EF-015 — Prendre une décision de vérification

Acteur : Eventix
Priorité : CRITICAL

Eventix doit pouvoir produire une décision de vérification pouvant notamment aboutir à :

validation ;
refus ;
vérification complémentaire.

Un événement refusé ne doit pas pouvoir être publié.

EF-016 — Publier un événement validé

Acteur : Organisateur / Eventix
Priorité : CRITICAL

Eventix doit permettre la publication d'un événement lorsqu'il satisfait les conditions nécessaires.

EF-017 — Consulter ses événements

Acteur : Organisateur
Priorité : HIGH

Eventix doit permettre à l'organisateur de :

consulter ses événements ;
rechercher ses événements ;
filtrer ses événements ;
identifier leur état ;
accéder aux informations associées.
EF-018 — Arrêter les ventes

Acteur : Organisateur / Eventix
Priorité : CRITICAL

Eventix doit permettre l'arrêt des ventes selon les conditions applicables à l'événement.

Les ventes doivent également être automatiquement arrêtées lorsque l'événement atteint son début selon les règles métier.

EF-019 — Empêcher les nouvelles ventes après la fin de l'événement

Acteur : Eventix
Priorité : CRITICAL

Eventix ne doit plus permettre de nouvelles ventes normales après le début ou la fermeture définitive des ventes selon le cycle de l'événement.

EF-020 — Archiver un événement

Acteur : Eventix
Priorité : HIGH

Eventix doit permettre l'archivage d'un événement lorsque les opérations nécessaires sont terminées.

L'archivage ne doit pas détruire son historique métier.

8. Domaine — Espaces, zones et disponibilités
EF-021 — Configurer les espaces

Acteur : Organisateur
Priorité : CRITICAL

Eventix doit permettre de configurer :

espaces ;
zones ;
capacités ;
places ;
places numérotées ;
places non numérotées.
EF-022 — Associer une catégorie de billet à une disponibilité

Acteur : Organisateur
Priorité : CRITICAL

Eventix doit permettre d'associer les catégories de billets aux espaces, zones ou places correspondants.

EF-023 — Gérer les capacités

Acteur : Organisateur
Priorité : CRITICAL

Eventix doit permettre de définir les capacités applicables aux différentes zones ou catégories.

EF-024 — Empêcher une réduction de capacité incompatible avec les billets attribués

Acteur : Eventix
Priorité : CRITICAL

Eventix doit refuser une réduction de capacité qui rendrait la capacité inférieure au nombre de billets déjà attribués.

9. Domaine — Découverte des événements
EF-025 — Rechercher des événements

Acteur : Participant
Priorité : CRITICAL

Eventix doit permettre au participant de rechercher des événements.

EF-026 — Consulter les événements disponibles

Acteur : Participant
Priorité : CRITICAL

Eventix doit permettre au participant de consulter les événements accessibles à la vente ou à l'inscription.

EF-027 — Consulter le détail d'un événement

Acteur : Participant
Priorité : CRITICAL

Eventix doit permettre au participant de consulter les informations nécessaires avant sa décision de participation ou d'achat.

EF-028 — Filtrer les événements

Acteur : Participant
Priorité : HIGH

Eventix doit permettre de filtrer les événements selon les critères disponibles.

EF-029 — Consulter les disponibilités

Acteur : Participant
Priorité : CRITICAL

Eventix doit permettre de consulter les disponibilités correspondant à la configuration actuelle de l'événement.

La disponibilité doit tenir compte notamment :

des billets vendus ;
des billets temporairement réservés ;
des billets encore disponibles.
10. Domaine — Réservation
EF-030 — Créer une réservation temporaire

Acteur : Participant
Priorité : CRITICAL

Eventix doit permettre de réserver temporairement une disponibilité avant la finalisation de l'achat.

La réservation ne constitue pas un achat.

EF-031 — Bloquer temporairement une disponibilité

Acteur : Eventix
Priorité : CRITICAL

Lorsqu'une réservation valide est créée, la disponibilité correspondante doit être temporairement bloquée.

EF-032 — Limiter une réservation à cinq minutes

Acteur : Eventix
Priorité : CRITICAL

Une réservation PENDING doit expirer après cinq minutes lorsqu'elle n'est pas finalisée conformément aux conditions applicables.

EF-033 — Libérer une disponibilité après expiration

Acteur : Eventix
Priorité : CRITICAL

Lorsqu'une réservation expire, Eventix doit libérer la disponibilité correspondante.

L'expiration ne doit pas être interprétée automatiquement comme un échec du paiement.

EF-034 — Empêcher la double attribution d'une disponibilité

Acteur : Eventix
Priorité : CRITICAL

Eventix doit empêcher qu'une même disponibilité soit attribuée simultanément à plusieurs achats valides.

11. Domaine — Paiement
EF-035 — Initier un paiement

Acteur : Participant
Priorité : CRITICAL

Eventix doit permettre au participant d'initier le paiement d'une réservation payante.

EF-036 — Accepter Mobile Money dans le MVP

Acteur : Participant
Priorité : CRITICAL

Le MVP doit permettre le paiement par Mobile Money.

Les autres moyens de paiement ne font pas partie du périmètre actuel.

EF-037 — Suivre l'état d'un paiement

Acteur : Eventix
Priorité : CRITICAL

Eventix doit pouvoir distinguer les différents états applicables à une opération de paiement, notamment :

PENDING ;
CONFIRMED ;
FAILED.
EF-038 — Traiter une confirmation de paiement

Acteur : Eventix
Priorité : CRITICAL

Eventix doit pouvoir traiter une confirmation de paiement afin de permettre la finalisation de l'achat lorsque les conditions nécessaires sont réunies.

EF-039 — Garantir l'idempotence du traitement d'un paiement

Acteur : Eventix
Priorité : CRITICAL

Une même confirmation de paiement reçue plusieurs fois ne doit produire qu'un seul effet métier.

Elle ne doit notamment pas provoquer :

plusieurs achats ;
plusieurs ventes ;
plusieurs billets ;
plusieurs remboursements.
EF-040 — Gérer un paiement échoué

Acteur : Eventix
Priorité : CRITICAL

Eventix doit pouvoir enregistrer et traiter un paiement ayant échoué.

EF-041 — Réconcilier un paiement tardif

Acteur : Eventix
Priorité : CRITICAL

Lorsqu'une confirmation de paiement arrive après l'expiration de la réservation, Eventix doit déclencher une réconciliation.

La réconciliation doit déterminer si :

la disponibilité est encore attribuable ;
la disponibilité a déjà été attribuée ;
un remboursement doit être déclenché.

Le paiement tardif ne doit pas être ignoré uniquement parce que la réservation a expiré.

12. Domaine — Achat et billet
EF-042 — Finaliser un achat

Acteur : Eventix
Priorité : CRITICAL

Eventix doit pouvoir finaliser un achat lorsque les conditions nécessaires sont satisfaites.

La finalisation d'un achat ne doit pas être confondue avec le règlement financier ultérieur de l'organisateur.

EF-043 — Émettre un billet

Acteur : Eventix
Priorité : CRITICAL

Eventix doit émettre un billet lorsqu'un achat est définitivement finalisé.

EF-044 — Générer l'identifiant de contrôle du billet

Acteur : Eventix
Priorité : CRITICAL

Eventix doit fournir au billet un mécanisme permettant son identification et son contrôle.

Dans le MVP, ce mécanisme comprend un QR Code.

EF-045 — Associer un billet à un événement

Acteur : Eventix
Priorité : CRITICAL

Chaque billet doit être associé à l'événement pour lequel il a été émis.

EF-046 — Associer un billet à un propriétaire

Acteur : Eventix
Priorité : CRITICAL

Chaque billet doit avoir un propriétaire actif unique.

EF-047 — Garantir l'unicité du propriétaire actif

Acteur : Eventix
Priorité : CRITICAL

Un même billet ne doit jamais avoir simultanément plusieurs propriétaires actifs.

EF-048 — Consulter un billet

Acteur : Participant
Priorité : CRITICAL

Le participant doit pouvoir consulter les billets qui lui appartiennent.

EF-049 — Télécharger un billet

Acteur : Participant
Priorité : HIGH

Le participant doit pouvoir télécharger son billet.

EF-050 — Obtenir un billet par email

Acteur : Participant
Priorité : HIGH

Eventix doit permettre au participant de recevoir son billet par email.

EF-051 — Obtenir un billet pour un événement gratuit

Acteur : Participant
Priorité : CRITICAL

Eventix doit permettre au participant d'obtenir un billet pour un événement gratuit selon le parcours d'inscription applicable.

Un billet gratuit doit néanmoins consommer la disponibilité correspondante.

EF-052 — Ne pas recréer un billet en cas d'échec technique d'émission

Acteur : Eventix
Priorité : CRITICAL

Si le paiement est confirmé mais que l'émission du billet échoue techniquement, Eventix doit pouvoir reprendre l'émission sans :

recréer l'achat ;
débiter à nouveau le participant ;
créer plusieurs billets pour la même transaction.
13. Domaine — Transfert de billet
EF-053 — Transférer un billet

Acteur : Participant
Priorité : HIGH

Eventix doit permettre le transfert d'un billet lorsque les conditions applicables l'autorisent.

EF-054 — Modifier le propriétaire actif lors d'un transfert

Acteur : Eventix
Priorité : HIGH

Lorsqu'un transfert est validé, Eventix doit remplacer le propriétaire actif du billet sans créer une copie du billet.

EF-055 — Conserver l'historique du transfert

Acteur : Eventix
Priorité : HIGH

Eventix doit conserver l'historique des changements de propriété d'un billet.

EF-056 — Ne pas proposer la revente dans le MVP

Acteur : Eventix
Priorité : HIGH

Le MVP ne doit pas fournir de mécanisme de revente de billets.

La Marketplace de revente appartient à une évolution future.

14. Domaine — Contrôle des billets
EF-057 — Affecter un contrôleur à un point d'entrée

Acteur : Organisateur / Eventix
Priorité : HIGH

Eventix doit permettre d'associer un contrôleur à un point d'entrée d'un événement.

EF-058 — Scanner un billet

Acteur : Agent de contrôle
Priorité : CRITICAL

Eventix doit permettre à un agent autorisé de scanner le QR Code d'un billet.

EF-059 — Vérifier l'authenticité d'un billet

Acteur : Eventix
Priorité : CRITICAL

Eventix doit déterminer si le billet présenté correspond à un billet reconnu par le système.

EF-060 — Vérifier l'événement du billet

Acteur : Eventix
Priorité : CRITICAL

Eventix doit vérifier que le billet présenté appartient à l'événement contrôlé.

EF-061 — Vérifier l'état du billet

Acteur : Eventix
Priorité : CRITICAL

Eventix doit vérifier que le billet est dans un état permettant son utilisation.

EF-062 — Refuser un billet déjà utilisé

Acteur : Eventix
Priorité : CRITICAL

Eventix doit refuser un billet ayant déjà été utilisé avec succès.

EF-063 — Refuser un billet annulé

Acteur : Eventix
Priorité : CRITICAL

Eventix doit refuser un billet annulé.

EF-064 — Refuser un billet appartenant à un autre événement

Acteur : Eventix
Priorité : CRITICAL

Eventix doit refuser un billet valide mais associé à un autre événement.

EF-065 — Autoriser un billet valide

Acteur : Eventix
Priorité : CRITICAL

Lorsque toutes les vérifications nécessaires sont positives, Eventix doit permettre l'autorisation d'accès.

EF-066 — Marquer un billet comme utilisé

Acteur : Eventix
Priorité : CRITICAL

Après autorisation effective de l'accès, Eventix doit faire passer le billet à l'état USED.

EF-067 — Garantir une seule validation réussie

Acteur : Eventix
Priorité : CRITICAL

Pour un même événement, une seule tentative de validation d'un billet doit pouvoir réussir.

Les tentatives concurrentes doivent être traitées conformément à cette règle.

EF-068 — Enregistrer le contexte d'un contrôle

Acteur : Eventix
Priorité : HIGH

Eventix doit conserver les informations nécessaires au suivi d'un contrôle, notamment :

billet ;
événement ;
contrôleur ;
point d'entrée ;
date ;
heure ;
résultat.
EF-069 — Permettre plusieurs scanners lorsque l'état partagé est fiable

Acteur : Eventix
Priorité : HIGH

Eventix doit permettre le fonctionnement de plusieurs scanners lorsque leur état partagé peut être maintenu de manière fiable.

EF-070 — Basculer en mode mono-scanner lorsque la synchronisation n'est plus fiable

Acteur : Eventix
Priorité : CRITICAL

Lorsque l'état partagé fiable n'est plus disponible, Eventix doit empêcher les validations concurrentes indépendantes et autoriser un seul scanner actif.

Cette règle privilégie :

Fiabilité
    >
Débit de contrôle
EF-071 — Reprendre la synchronisation après le mode dégradé

Acteur : Eventix
Priorité : HIGH

Lorsque les conditions de synchronisation fiables sont rétablies, les opérations réalisées pendant le mode dégradé doivent pouvoir être réintégrées dans l'état métier cohérent.

15. Domaine — Annulation et report
EF-072 — Annuler un événement

Acteur : Organisateur / Eventix
Priorité : CRITICAL

Eventix doit permettre l'annulation d'un événement selon les règles applicables.

EF-073 — Bloquer les nouvelles ventes après annulation

Acteur : Eventix
Priorité : CRITICAL

Un événement annulé ne doit plus recevoir de nouvelles ventes.

EF-074 — Invalider les billets concernés par une annulation

Acteur : Eventix
Priorité : CRITICAL

Les billets concernés par l'annulation doivent devenir inutilisables conformément aux règles métier.

EF-075 — Informer les participants concernés

Acteur : Eventix
Priorité : HIGH

Eventix doit permettre d'informer les participants concernés par une annulation ou un report.

Le canal et les modalités détaillées devront respecter les capacités de communication du MVP.

EF-076 — Déterminer l'éligibilité au remboursement après annulation

Acteur : Eventix
Priorité : CRITICAL

Eventix doit pouvoir déterminer les billets éligibles à un remboursement lorsqu'un événement est annulé.

Une annulation imputable à l'organisateur rend les billets concernés éligibles au remboursement selon les règles applicables.

EF-077 — Reporter un événement

Acteur : Organisateur / Eventix
Priorité : HIGH

Eventix doit permettre le report d'un événement conformément aux règles métier.

EF-078 — Conserver les billets lors d'un report

Acteur : Eventix
Priorité : HIGH

Lorsqu'un événement est reporté, les billets existants doivent être conservés et rattachés à la nouvelle date conformément aux règles applicables.

Un simple report ne doit pas déclencher automatiquement un remboursement.

EF-079 — Gérer une incompatibilité lors d'un report

Acteur : Eventix
Priorité : HIGH

Eventix doit pouvoir gérer les situations dans lesquelles la nouvelle configuration d'un événement est incompatible avec les billets existants.

Statut : À PRÉCISER

Les règles détaillées restent ouvertes dans :

03-decouverte-du-metier/questions-metier-ouvertes.md

16. Domaine — Remboursement
EF-080 — Déterminer qu'un remboursement est requis

Acteur : Eventix
Priorité : CRITICAL

Eventix doit pouvoir déterminer lorsqu'une situation métier entraîne une obligation de remboursement.

EF-081 — Créer une demande de remboursement

Acteur : Eventix
Priorité : CRITICAL

Lorsqu'un remboursement est requis, Eventix doit pouvoir enregistrer l'obligation de remboursement correspondante.

EF-082 — Calculer le montant de référence du remboursement

Acteur : Eventix
Priorité : CRITICAL

Le montant de référence d'un remboursement doit correspondre au montant effectivement payé pour la transaction concernée.

EF-083 — Suivre l'état d'un remboursement

Acteur : Eventix
Priorité : CRITICAL

Eventix doit pouvoir suivre les états d'un remboursement, notamment :

PENDING ;
PROCESSING ;
FAILED ;
COMPLETED.
EF-084 — Retenter un remboursement échoué

Acteur : Eventix
Priorité : HIGH

Lorsqu'un remboursement échoue techniquement, Eventix doit conserver l'obligation de remboursement et permettre une nouvelle tentative.

EF-085 — Garantir l'idempotence des remboursements

Acteur : Eventix
Priorité : CRITICAL

Une même obligation de remboursement ne doit pas produire plusieurs remboursements effectifs.

EF-086 — Traiter les remboursements progressivement

Acteur : Eventix
Priorité : HIGH

Eventix doit pouvoir traiter progressivement un volume important de remboursements jusqu'à leur finalisation.

EF-087 — Ne pas rembourser automatiquement un participant qui change d'avis

Acteur : Eventix
Priorité : HIGH

Dans le MVP, le changement d'avis, la non-présentation ou le souhait de ne plus participer ne doivent pas déclencher automatiquement un remboursement lorsque l'événement est maintenu.

17. Domaine — Signalement et sécurité
EF-088 — Signaler un événement

Acteur : Participant
Priorité : HIGH

Eventix doit permettre à un participant de signaler un événement.

EF-089 — Signaler un organisateur

Acteur : Participant
Priorité : HIGH

Eventix doit permettre à un participant de signaler un organisateur.

EF-090 — Enregistrer un motif de signalement

Acteur : Participant
Priorité : HIGH

Un signalement doit comporter un motif prédéfini obligatoire.

EF-091 — Ajouter une description au signalement

Acteur : Participant
Priorité : MEDIUM

Eventix doit permettre d'ajouter une description facultative à un signalement.

EF-092 — Analyser un signalement

Acteur : Eventix
Priorité : CRITICAL

Eventix doit permettre l'analyse des signalements reçus.

Un signalement ne doit pas être considéré automatiquement comme une preuve de fraude.

EF-093 — Évaluer le niveau de risque

Acteur : Eventix
Priorité : HIGH

Eventix doit pouvoir adapter la réponse au niveau de risque identifié.

Les réponses peuvent notamment comprendre :

surveillance ;
contrôles supplémentaires ;
limitation ;
suspension ;
intervention humaine.
EF-094 — Traçabiliser les décisions de sécurité

Acteur : Eventix
Priorité : CRITICAL

Les décisions sensibles relatives à la sécurité, aux restrictions et à la fraude doivent être traçables.

18. Domaine — Bannissement
EF-095 — Bannir une organisation

Acteur : Eventix
Priorité : CRITICAL

Eventix doit pouvoir bannir une organisation lorsqu'une fraude est confirmée et que les règles applicables le justifient.

EF-096 — Bloquer l'activité d'une organisation bannie

Acteur : Eventix
Priorité : CRITICAL

Une organisation bannie ne doit plus pouvoir poursuivre librement son activité d'organisation.

EF-097 — Évaluer les événements existants d'une organisation bannie

Acteur : Eventix
Priorité : HIGH

Lorsqu'une organisation est bannie, Eventix doit pouvoir évaluer séparément les événements existants.

Un événement existant ne doit pas être automatiquement supprimé sans analyse.

EF-098 — Appliquer une mesure aux événements concernés

Acteur : Eventix
Priorité : HIGH

Après évaluation, Eventix doit pouvoir appliquer une mesure appropriée à un événement existant, notamment :

maintien ;
suspension ;
annulation ;
nouvelle vérification.
19. Domaine — Statistiques et pilotage
EF-099 — Suivre les ventes

Acteur : Organisateur
Priorité : HIGH

Eventix doit permettre à l'organisateur de suivre les ventes de ses événements.

EF-100 — Analyser les ventes par contexte

Acteur : Organisateur
Priorité : HIGH

Eventix doit permettre d'analyser les ventes notamment selon :

événement ;
catégorie ;
période ;
canal lorsque celui-ci existe dans le périmètre ;
point de vente lorsque celui-ci existe dans le périmètre ;
agent lorsque celui-ci existe dans le périmètre.
EF-101 — Suivre les entrées

Acteur : Organisateur
Priorité : HIGH

Eventix doit permettre de suivre les contrôles et les entrées enregistrées.

EF-102 — Suivre les participants

Acteur : Organisateur
Priorité : HIGH

Eventix doit permettre de distinguer :

Billet vendu
     ≠
Participant attendu
     ≠
Participant présent
EF-103 — Consulter l'activité de l'événement

Acteur : Organisateur
Priorité : HIGH

Eventix doit permettre de suivre l'activité pertinente de l'événement, notamment :

ventes ;
entrées ;
participants présents ;
disponibilité.
EF-104 — Produire des statistiques sans modifier les données métier

Acteur : Eventix
Priorité : CRITICAL

Les statistiques doivent être produites à partir des données métier sans modifier directement :

une vente ;
un paiement ;
un billet ;
une réservation.
20. Domaine — Clôture financière
EF-105 — Préparer la clôture financière

Acteur : Eventix
Priorité : CRITICAL

Après la fin de l'événement, Eventix doit pouvoir traiter les opérations financières nécessaires à la détermination du montant net dû à l'organisateur.

EF-106 — Déterminer le montant net organisateur

Acteur : Eventix
Priorité : CRITICAL

Eventix doit pouvoir déterminer le montant net dû à l'organisateur à partir notamment :

Ventes confirmées
      −
Remboursements
      −
Frais / commissions applicables
      =
Montant net organisateur

Les règles précises concernant les frais, commissions et taxes restent soumises aux règles financières applicables.

EF-107 — Clôturer financièrement un événement

Acteur : Eventix
Priorité : CRITICAL

Eventix doit pouvoir clôturer financièrement un événement lorsque les opérations nécessaires sont terminées.

EF-108 — Rendre le solde disponible après clôture

Acteur : Eventix
Priorité : CRITICAL

Après la clôture financière, le montant net disponible doit être ajouté au solde retirable de l'organisateur.

21. Domaine — Retraits organisateur
EF-109 — Consulter le solde disponible

Acteur : Organisateur
Priorité : CRITICAL

L'organisateur doit pouvoir consulter son solde disponible.

EF-110 — Demander un retrait

Acteur : Organisateur
Priorité : CRITICAL

L'organisateur doit pouvoir demander le retrait de tout ou partie de son solde disponible.

EF-111 — Empêcher un retrait supérieur au solde

Acteur : Eventix
Priorité : CRITICAL

Eventix doit refuser un retrait dont le montant est supérieur au solde disponible.

EF-112 — Suivre l'état d'un retrait

Acteur : Eventix
Priorité : CRITICAL

Eventix doit pouvoir suivre notamment les états :

PENDING ;
PROCESSING ;
FAILED ;
COMPLETED.
EF-113 — Restituer un retrait échoué au solde

Acteur : Eventix
Priorité : HIGH

Lorsqu'un retrait échoue, le montant concerné doit être restitué au solde disponible conformément au processus financier.

EF-114 — Autoriser plusieurs retraits successifs

Acteur : Organisateur
Priorité : HIGH

Eventix doit permettre plusieurs retraits successifs tant qu'un solde disponible reste suffisant.

22. Domaine — Historique et traçabilité
EF-115 — Conserver l'historique des opérations importantes

Acteur : Eventix
Priorité : CRITICAL

Eventix doit conserver les principales opérations métier afin de garantir leur traçabilité.

Les opérations concernées comprennent notamment :

création d'événement ;
modification importante ;
soumission ;
vérification ;
publication ;
réservation ;
paiement ;
achat ;
émission de billet ;
transfert ;
contrôle ;
remboursement ;
annulation ;
report ;
clôture ;
retrait ;
décisions de sécurité.
EF-116 — Associer une opération à son contexte

Acteur : Eventix
Priorité : CRITICAL

Lorsqu'une opération métier doit être tracée, Eventix doit pouvoir l'associer à son contexte pertinent, notamment :

acteur ;
événement ;
objet métier ;
date ;
résultat.
EF-117 — Préserver l'historique après modification

Acteur : Eventix
Priorité : CRITICAL

Une modification ultérieure d'un événement, d'un prix ou d'une configuration ne doit pas réécrire l'historique des opérations déjà réalisées.

23. Domaine — Vente et canaux

Important : le périmètre de la vente physique présente une divergence entre certains documents historiques de la découverte métier et la consolidation actuelle des processus métier. Le processus-metier.md de référence indique explicitement que la vente physique est hors MVP. Les exigences ci-dessous suivent donc la consolidation actuelle du processus métier.

EF-118 — Vendre un billet en ligne

Acteur : Participant
Priorité : CRITICAL

Eventix doit permettre l'achat de billets en ligne dans le MVP.

EF-119 — Centraliser la disponibilité des ventes

Acteur : Eventix
Priorité : CRITICAL

Les opérations de vente entrant dans le périmètre Eventix doivent s'appuyer sur une vision cohérente de la disponibilité.

EF-120 — Reporter les fonctionnalités de vente physique

Acteur : Eventix
Priorité : Hors MVP

Les fonctionnalités suivantes ne font pas partie du MVP actuel :

points de vente physiques ;
agents vendeurs ;
vente physique ;
vente offline ;
synchronisation des ventes physiques.

Elles devront faire l'objet d'une analyse spécifique lorsqu'elles entreront dans le périmètre produit.

24. Domaine — Communications
EF-121 — Mettre le billet à disposition du participant

Acteur : Eventix
Priorité : CRITICAL

Une fois le billet émis, Eventix doit permettre au participant d'y accéder depuis son compte.

EF-122 — Distribuer le billet par email

Acteur : Eventix
Priorité : HIGH

Eventix doit pouvoir distribuer le billet par email.

EF-123 — Informer lors des changements importants

Acteur : Eventix
Priorité : HIGH

Eventix doit pouvoir informer les participants concernés par des événements métier importants tels que :

annulation ;
report ;
situation nécessitant une information participant.

Les modalités détaillées de communication seront précisées dans les exigences correspondantes.

25. Domaine — Suppression et conservation
EF-124 — Empêcher la suppression d'un événement ayant des opérations irréversibles

Acteur : Eventix
Priorité : CRITICAL

Lorsqu'un événement a fait l'objet d'une opération métier irréversible, Eventix ne doit pas permettre sa suppression définitive.

EF-125 — Remplacer la suppression par une gestion d'état appropriée

Acteur : Eventix
Priorité : HIGH

Lorsqu'une suppression définitive n'est plus possible, Eventix doit pouvoir appliquer selon le contexte :

annulation ;
masquage ;
archivage.
26. Exigences fonctionnelles transversales
EF-126 — Préserver l'indépendance des cycles métier

Acteur : Eventix
Priorité : CRITICAL

Eventix doit maintenir des états distincts pour :

Réservation
Paiement
Billet
Présence
Règlement financier

L'état d'un cycle ne doit pas être utilisé pour déduire automatiquement celui d'un autre.

EF-127 — Garantir l'unicité des effets métier critiques

Acteur : Eventix
Priorité : CRITICAL

Les opérations critiques doivent produire un effet métier unique lorsque le même événement métier est reçu ou traité plusieurs fois.

Cette exigence concerne notamment :

paiements ;
remboursements ;
émission de billets ;
validations de billets.
EF-128 — Préserver une source de vérité métier cohérente

Acteur : Eventix
Priorité : CRITICAL

Eventix doit maintenir une vision cohérente des disponibilités et des états métier critiques.

EF-129 — Ne pas exposer les responsabilités internes d'un domaine

Acteur : Eventix
Priorité : CRITICAL

Une fonctionnalité appartenant à un domaine ne doit pas imposer à un autre domaine de gérer directement ses données ou ses règles internes.

Les collaborations entre domaines doivent s'effectuer à partir des résultats et informations métier nécessaires.

27. Principes de faible couplage appliqués aux exigences

Les exigences précédentes sont organisées autour de responsabilités métier distinctes.

CONFIANCE
    │
    ├── Création
    ├── Configuration
    ├── Publication
    ├── Annulation
    └── Report


DISPONIBILITÉ
    │
    ├── Espaces
    ├── Zones
    ├── Places
    └── Capacités


RÉSERVATION
    │
    ├── Blocage temporaire
    └── Expiration


PAIEMENT
    │
    ├── Initiation
    ├── Confirmation
    ├── Échec
    └── Réconciliation


BILLETTERIE
    │
    ├── Achat
    ├── Émission
    ├── QR Code
    └── Propriété


DISTRIBUTION
    │
    ├── Compte
    ├── Téléchargement
    └── Email


ACCÈS
    │
    ├── Scan
    ├── Validation
    ├── Consommation
    └── Présence


SÉCURITÉ
    │
    ├── Signalement
    ├── Analyse
    ├── Suspension
    └── Bannissement


REMBOURSEMENT
    │
    ├── Éligibilité
    ├── Exécution
    └── Suivi


FINANCE
    │
    ├── Clôture
    ├── Solde
    └── Retrait


STATISTIQUES
    │
    ├── Observation
    └── Analyse


HISTORIQUE
    │
    └── Traçabilité

Cette organisation ne préjuge pas de l'architecture technique future.

28. Dépendances fonctionnelles autorisées

Les domaines peuvent dépendre de résultats produits par d'autres domaines sans absorber leurs responsabilités.

Exemple principal :

Réservation
     │
     │ réservation créée
     ↓
Paiement
     │
     │ paiement confirmé
     ↓
Achat
     │
     │ achat finalisé
     ↓
Billet
     │
     │ billet émis
     ↓
Distribution

La dépendance représente ici un enchaînement métier, et non une fusion des responsabilités.

29. Cas de réconciliation

La réconciliation constitue un point important du modèle Eventix.

Réservation
    ↓
PENDING
    ↓
5 minutes
    ↓
EXPIRED
    ↓
Paiement confirmé tardivement
    ↓
Réconciliation
       │
       ├── Disponibilité disponible
       │        ↓
       │      Billet
       │
       └── Disponibilité indisponible
                ↓
            Remboursement

Le domaine Paiement ne devient donc pas responsable du billet ou du remboursement.

Il fournit le résultat nécessaire à la poursuite du processus.

30. Cas de contrôle dégradé

Le contrôle des billets constitue une autre application importante du principe de faible couplage et de fiabilité.

État partagé fiable
        ↓
Plusieurs scanners possibles
        ↓
Contrôles parallèles

Si l'état partagé fiable n'est plus disponible :

État partagé non fiable
        ↓
Mode dégradé
        ↓
Un seul scanner actif
        ↓
Contrôles séquentiels

Le choix métier est volontaire :

La fiabilité du contrôle prime sur le débit lorsque la cohérence ne peut plus être garantie.

31. Exigences dépendant de décisions encore ouvertes

Certaines exigences sont volontairement maintenues avec un statut À PRÉCISER.

EF-079 — Incompatibilité lors d'un report

Les règles détaillées restent à définir dans :

03-decouverte-du-metier/questions-metier-ouvertes.md
EF-011 — Modification importante après publication

Les conséquences précises d'une modification d'événement après publication et après vente doivent être précisées.

Référence :

03-decouverte-du-metier/questions-metier-ouvertes.md
EF-106 — Frais, commissions et taxes

Le calcul précis des éléments financiers applicables doit être précisé par les règles financières et commerciales.

Référence :

03-decouverte-du-metier/questions-metier-ouvertes.md
32. Fonctionnalités hors MVP

Les fonctionnalités suivantes ne font pas partie des exigences fonctionnelles du MVP :

vente physique ;
points de vente physiques ;
agents vendeurs ;
vente offline ;
WhatsApp ;
Marketplace de revente ;
Quiz Live ;
Reels ;
cloud photos/vidéos ;
fonctionnalités sociales avancées ;
architectures offline avancées ;
synchronisation distribuée avancée.

Ces fonctionnalités feront l'objet d'une analyse dédiée lorsqu'elles entreront dans le périmètre produit.

33. États métier pris en compte

Les exigences doivent respecter les états métier définis dans les processus et règles métier.

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
34. Invariants fonctionnels majeurs

Les exigences doivent préserver les invariants suivants :

Une disponibilité
        ↓
Une attribution
        ↓
Un billet
        ↓
Un propriétaire actif
Une confirmation de paiement
        ↓
Un seul effet métier
Une obligation de remboursement
        ↓
Un seul remboursement effectif
Un billet validé
        ↓
USED
        ↓
Aucune seconde validation réussie
Un organisateur
        ↓
Ne peut retirer
        ↓
Plus que son solde disponible
Une opération métier réalisée
        ↓
Reste traçable
35. Traçabilité attendue

Chaque exigence devra pouvoir être reliée ultérieurement à :

Besoin métier
      ↓
User Story
      ↓
Use Case
      ↓
Exigence fonctionnelle
      ↓
Critère d'acceptation

La matrice de traçabilité sera définie dans :

matrice-de-tracabilite.md
36. Résumé

Le MVP Eventix doit principalement permettre :

                    CONFIANCE
                       ↓
                  ORGANISATEUR
                       ↓
                   ÉVÉNEMENT
                       ↓
                 PUBLICATION
                       ↓
                  DISPONIBILITÉ
                       ↓
                  RÉSERVATION
                       ↓
                    PAIEMENT
                       ↓
                     ACHAT
                       ↓
                    BILLET
                       ↓
                    CONTRÔLE
                       ↓
                  PARTICIPATION
                       ↓
              CLÔTURE FINANCIÈRE
                       ↓
                     SOLDE
                       ↓
                    RETRAIT

Avec deux mécanismes transversaux :

SÉCURITÉ
    ↓
Signalement → Analyse → Mesure


HISTORIQUE
    ↓
Traçabilité des opérations importantes
37. Critères de qualité du document

Les exigences de ce document doivent respecter les propriétés suivantes :

une responsabilité principale par exigence ;
faible couplage entre responsabilités ;
absence de décision technique prématurée ;
absence de règle métier inventée ;
distinction entre les cycles métier ;
exigences vérifiables ;
identifiants uniques ;
traçabilité future ;
explicitation des questions encore ouvertes ;
séparation claire entre MVP et fonctionnalités futures.
38. Statut
Document : exigences-fonctionnelles.md


Version : 1.0
Statut : À valider par l'équipe
Périmètre : MVP Eventix
Marché : Cameroun


Principe de faible couplage : APPLIQUÉ
Séparation des cycles métier : APPLIQUÉE
Traçabilité : PRÉPARÉE
Questions ouvertes : EXPLICITEMENT CONSERVÉES
Architecture technique : NON DÉCIDÉE


### Point important


J'ai volontairement **corrigé la granularité** par rapport à notre première tentative : au lieu d'avoir une exigence qui fait « paiement + billet + email + statistiques », les responsabilités sont maintenant séparées. Cela prépare directement les prochaines phases `user-stories.md`, `use-cases.md`, `criteres-d-acceptation.md` et `matrice-de-tracabilite.md`.


J'ai aussi conservé explicitement les points non tranchés plutôt que de les transformer en règles arbitraires. Les règles métier validées confirment notamment l'idempotence, l'unicité des billets, la séparation des cycles et le traitement du contrôle dégradé. :contentReference[oaicite:2]{index=2} :contentReference[oaicite:3]{index=3}


**Une incohérence documentaire subsiste concernant la vente physique :** `besoins-metier.md` contient encore des besoins de points physiques, alors que la consolidation de `processus-metier.md` les place hors MVP. Je l'ai volontairement résolue en faveur de la **référence processus métier consolidée**, tout en la signalant dans le fichier, plutôt que de masquer la divergence. :contentReference[oaicite:4]{index=4} :contentReference[oaicite:5]{index=5}