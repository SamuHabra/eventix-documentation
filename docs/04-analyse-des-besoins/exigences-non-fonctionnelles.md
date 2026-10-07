# Exigences non fonctionnelles — Eventix

> **Phase :** 04 — Analyse des besoins  
> **Projet :** Eventix  
> **Périmètre :** MVP — Billetterie  
> **Marché initial :** Cameroun  
> **Statut :** Version de référence — à valider

---

# 1. Objectif

Ce document définit les exigences non fonctionnelles auxquelles Eventix doit satisfaire.

Les exigences non fonctionnelles décrivent les propriétés attendues du système concernant notamment :

- performance ;
- fiabilité ;
- disponibilité ;
- sécurité ;
- cohérence ;
- traçabilité ;
- maintenabilité ;
- évolutivité ;
- résilience ;
- maîtrise de la complexité.

Elles ne décrivent pas principalement **ce que fait Eventix**, mais **avec quel niveau de qualité et quelles contraintes Eventix doit fonctionner**.

---

# 2. Sources de référence

Les exigences de ce document sont dérivées de :

- `02-vision-produit/objectifs-produit.md`
- `03-decouverte-du-metier/besoins-metier.md`
- `03-decouverte-du-metier/contraintes-metier.md`
- `03-decouverte-du-metier/regles-metier.md`
- `03-decouverte-du-metier/processus-metier.md`
- `03-decouverte-du-metier/questions-metier-ouvertes.md`
- `03-decouverte-du-metier/cybersecurite-eventix/README.md` — enjeux, actifs, impacts et gouvernance à confirmer pour la cybersécurité du système.
- `04-analyse-des-besoins/exigences-fonctionnelles.md`

---

# 3. Principes de construction

## 3.1 Ne pas inventer de seuil technique

Une exigence non fonctionnelle ne doit pas introduire arbitrairement une valeur technique.

Exemple à éviter :

```text
Le système doit répondre en moins de 200 ms.

si aucune source métier ne justifie cette valeur.

Dans ce cas :

Le système doit fournir une expérience suffisamment réactive
pour les opérations critiques.


Valeur cible :
À PRÉCISER.

Les seuils précis seront définis lorsque les données nécessaires seront disponibles.

4. Performance
ENF-001 — Réactivité de la consultation des événements

Priorité : CRITICAL

Eventix doit permettre une consultation suffisamment rapide des événements afin de ne pas dégrader le parcours de découverte et d'achat.

Les pages et informations nécessaires à la découverte d'un événement doivent être accessibles avec une latence compatible avec l'expérience attendue.

Valeur cible : À PRÉCISER

La valeur précise doit être définie ultérieurement.

ENF-002 — Réactivité du parcours d'achat

Priorité : CRITICAL

Le parcours :

Découverte
    ↓
Disponibilité
    ↓
Réservation
    ↓
Paiement
    ↓
Obtention du billet

doit rester suffisamment réactif pour permettre au participant de terminer son achat sans dégradation excessive de l'expérience.

Valeur cible : À PRÉCISER

ENF-003 — Réactivité du contrôle

Priorité : CRITICAL

Le contrôle d'un billet doit fournir une réponse suffisamment rapide pour permettre un flux d'entrée exploitable lors d'un événement.

La recherche d'une validation doit rester compatible avec le contexte opérationnel d'un contrôle à l'entrée.

Valeur cible : À PRÉCISER

ENF-004 — Priorité à la fiabilité lorsque performance et cohérence entrent en conflit

Priorité : CRITICAL

Lorsqu'un compromis oppose vitesse et fiabilité du contrôle, Eventix doit privilégier la fiabilité.

Cette règle est particulièrement applicable lorsque plusieurs scanners ne peuvent plus disposer d'un état partagé fiable.

Dans ce cas :

Plusieurs scanners
        ↓
État partagé fiable
        ↓
Autorisé


État partagé non fiable
        ↓
Mode mono-scanner

Cette décision est explicitement définie dans les règles métier.

5. Cohérence et intégrité
ENF-005 — Cohérence de la disponibilité

Priorité : CRITICAL

Eventix doit maintenir une vision cohérente de la disponibilité.

Une même disponibilité ne doit pas pouvoir être attribuée simultanément à plusieurs achats valides.

Cette contrainte constitue un invariant métier.

ENF-006 — Cohérence des cycles métier

Priorité : CRITICAL

Eventix doit maintenir une séparation fiable entre :

Réservation
≠
Paiement
≠
Billet
≠
Présence
≠
Settlement

L'état d'un cycle ne doit pas être déduit automatiquement de celui d'un autre.

ENF-007 — Unicité des effets critiques

Priorité : CRITICAL

Eventix doit garantir qu'une même opération critique ne produit pas plusieurs effets métier.

Cela concerne notamment :

confirmations de paiement ;
émission de billets ;
remboursements ;
validations de billets.

Les confirmations répétées d'un paiement doivent produire un seul effet métier.

ENF-008 — Unicité du propriétaire actif

Priorité : CRITICAL

Eventix doit garantir qu'un billet ne possède jamais plusieurs propriétaires actifs simultanément.

6. Fiabilité
ENF-009 — Fiabilité du contrôle des billets

Priorité : CRITICAL

Eventix doit garantir qu'un billet validé avec succès ne puisse pas être validé une seconde fois.

Cet invariant doit rester vrai même lorsque plusieurs tentatives de contrôle surviennent.

ENF-010 — Fiabilité en mode dégradé

Priorité : CRITICAL

Lorsque la synchronisation nécessaire au fonctionnement de plusieurs scanners n'est plus fiable, Eventix doit empêcher les validations concurrentes susceptibles de produire un état incohérent.

Le système doit alors appliquer le mode mono-scanner.

Cette décision privilégie explicitement la fiabilité au débit.

ENF-011 — Reprise après perte de synchronisation

Priorité : HIGH

Après rétablissement de la connectivité et d'un état partagé fiable, les contrôles réalisés pendant le mode dégradé doivent pouvoir être réintégrés dans l'état cohérent d'Eventix.

ENF-012 — Fiabilité de la réconciliation

Priorité : CRITICAL

Eventix doit pouvoir traiter de manière fiable les situations dans lesquelles des opérations arrivent dans un ordre différent de celui initialement attendu.

Le cas principal concerne :

Réservation expirée
        ↓
Paiement confirmé tardivement
        ↓
Réconciliation

La réconciliation doit permettre de déterminer si le billet peut encore être attribué ou si un remboursement est nécessaire.

7. Disponibilité
ENF-013 — Disponibilité du parcours critique

Priorité : CRITICAL

Les fonctions critiques du parcours de billetterie doivent rester disponibles pendant les périodes normales d'utilisation.

Les fonctions critiques comprennent notamment :

consultation d'événements ;
réservation ;
paiement ;
obtention du billet ;
contrôle.

Niveau de disponibilité cible : À PRÉCISER

ENF-014 — Disponibilité du contrôle pendant les événements

Priorité : CRITICAL

Le système de contrôle doit être disponible pendant les périodes d'entrée des participants.

Une interruption du contrôle peut directement empêcher ou ralentir l'accès aux événements.

Objectif précis de disponibilité : À PRÉCISER

ENF-015 — Gestion des dégradations de connectivité

Priorité : CRITICAL

Une dégradation de connectivité ne doit pas conduire Eventix à accepter des contrôles concurrents lorsqu'il ne peut plus garantir la cohérence de l'état partagé.

Le système doit privilégier une dégradation contrôlée à un fonctionnement rapide mais incohérent.

8. Sécurité

Les exigences ci-dessous sont alimentées à la fois par les règles de confiance métier et par la découverte des enjeux de cybersécurité du système. La vérification d'un organisateur, la proportionnalité d'une mesure contre la fraude et la protection technique contre une compromission sont des responsabilités distinctes ; leur traçabilité ne doit pas les confondre. Les questions de gouvernance et de criticité encore ouvertes figurent en QMO-053 à QMO-056.

ENF-016 — Protection des comptes

Priorité : CRITICAL

Eventix doit protéger les comptes utilisateurs contre les accès non autorisés.

Les mécanismes précis de sécurité seront définis lors de la conception de l'architecture et de la sécurité.

ENF-017 — Protection des opérations sensibles

Priorité : CRITICAL

Les opérations sensibles doivent être accessibles uniquement aux acteurs autorisés.

Cela concerne notamment :

gestion des événements ;
vérification ;
gestion financière ;
remboursements ;
contrôles ;
décisions de sécurité ;
bannissements.
ENF-018 — Traçabilité des décisions de sécurité

Priorité : CRITICAL

Les actions importantes liées à :

fraude ;
suspension ;
restriction ;
sécurité ;
bannissement

doivent être enregistrées afin de pouvoir être auditées.

ENF-019 — Proportionnalité des mesures de sécurité

Priorité : CRITICAL

Eventix ne doit pas considérer automatiquement un signalement comme une fraude confirmée.

Le système doit conserver la distinction :

Signalement
    ≠
Fraude confirmée

Une suspicion doit pouvoir conduire à une analyse avant une sanction définitive.

ENF-020 — Protection contre la double utilisation d'un billet

Priorité : CRITICAL

Le système doit empêcher la réutilisation frauduleuse ou accidentelle d'un billet déjà validé.

9. Traçabilité
ENF-021 — Traçabilité des opérations critiques

Priorité : CRITICAL

Les opérations critiques doivent être traçables.

La trace doit permettre, lorsque pertinent, d'identifier :

l'opération ;
l'acteur ;
l'événement ;
l'objet concerné ;
la date ;
le résultat.

Cette exigence découle directement du principe métier de traçabilité.

ENF-022 — Traçabilité financière

Priorité : CRITICAL

Les opérations suivantes doivent conserver leur historique :

paiements ;
remboursements ;
retraits.

Cette traçabilité doit permettre notamment la réconciliation et l'audit.

ENF-023 — Conservation de l'historique métier

Priorité : CRITICAL

Une modification future ne doit pas réécrire l'historique d'une transaction ou d'un billet déjà attribué.

ENF-024 — Conservation des données historiques utiles

Priorité : HIGH

Les données historiques nécessaires doivent être conservées pour permettre :

statistiques ;
audits ;
réconciliation ;
résolution des litiges ;
analyse des performances.
10. Résilience et gestion des erreurs
ENF-025 — Ne pas perdre une opération métier critique après une défaillance technique

Priorité : CRITICAL

Une défaillance technique ne doit pas conduire à la perte silencieuse d'une opération métier critique.

Les domaines concernés comprennent notamment :

paiement ;
réservation ;
billet ;
remboursement ;
retrait.
ENF-026 — Reprise des opérations interrompues

Priorité : HIGH

Lorsqu'une opération critique est interrompue par une défaillance technique, Eventix doit pouvoir déterminer son état et reprendre son traitement sans produire de doublon.

ENF-027 — Gestion des remboursements à grande échelle

Priorité : HIGH

Eventix doit pouvoir traiter progressivement un volume important de remboursements jusqu'à leur finalisation.

ENF-028 — Les échecs ne doivent pas détruire l'obligation métier

Priorité : CRITICAL

Un échec technique lors d'une opération de remboursement ou de retrait ne doit pas faire disparaître l'obligation ou le montant concerné.

L'opération doit rester identifiable et traitable.

11. Scalabilité
ENF-029 — Supporter l'augmentation du volume d'utilisation

Priorité : HIGH

Eventix doit pouvoir évoluer lorsque le nombre :

d'utilisateurs ;
d'événements ;
de billets ;
de transactions ;
de contrôles

augmente.

Objectif quantitatif : À PRÉCISER

ENF-030 — Supporter les pics d'activité

Priorité : CRITICAL

Eventix doit pouvoir absorber des périodes de forte activité associées notamment :

à l'ouverture d'une vente ;
à une forte demande pour un événement ;
à une période précédant un événement ;
à l'entrée massive de participants.

Charge cible : À PRÉCISER

ENF-031 — Ne pas sacrifier la cohérence pour absorber la charge

Priorité : CRITICAL

L'augmentation de la charge ne doit pas conduire Eventix à violer les invariants métier.

La cohérence de la disponibilité, des paiements, des billets et des contrôles reste prioritaire.

12. Maintenabilité et évolutivité
ENF-032 — Séparation des responsabilités

Priorité : HIGH

Les responsabilités métier doivent rester suffisamment séparées afin de limiter le couplage entre domaines.

La conception doit préserver notamment :

Réservation
Paiement
Billetterie
Contrôle
Sécurité
Finance

comme responsabilités distinctes.

ENF-033 — Évolutivité indépendante des domaines

Priorité : HIGH

L'évolution d'un domaine ne doit pas nécessiter systématiquement la modification des responsabilités internes des autres domaines.

Cette exigence applique le principe de faible couplage retenu pour Eventix.

ENF-034 — Ne pas imposer l'architecture par les exigences

Priorité : HIGH

Les exigences non fonctionnelles ne doivent pas imposer prématurément :

des microservices ;
une architecture monolithique ;
une technologie ;
un fournisseur cloud ;
une base de données spécifique.

Ces décisions appartiennent aux phases d'architecture.

ENF-035 — Préparer l'évolution vers des fonctionnalités futures

Priorité : MEDIUM

La conception doit éviter de rendre inutilement impossible l'ajout futur de domaines tels que :

Marketplace ;
Quiz Live ;
QR Cloud ;
Reels ;
autres services événementiels.

Ces domaines ne font toutefois pas partie des exigences du MVP.

13. Observabilité
ENF-036 — Permettre l'identification des opérations critiques

Priorité : HIGH

Eventix doit fournir suffisamment d'informations pour identifier les opérations critiques et leurs résultats.

Cela doit permettre notamment :

diagnostic ;
audit ;
réconciliation ;
analyse d'incident.
ENF-037 — Permettre l'analyse des incidents de contrôle

Priorité : HIGH

Les données relatives aux contrôles doivent permettre d'analyser les incidents et les anomalies.

Les besoins métier prévoient explicitement l'utilisation des données de contrôle pour :

suivi des entrées ;
statistiques ;
audits ;
analyse des incidents.
ENF-038 — Distinguer les erreurs techniques des états métier

Priorité : CRITICAL

Une erreur technique ne doit pas être interprétée automatiquement comme un changement d'état métier.

Exemple :

Erreur technique
      ≠
Paiement échoué

ou :

Réservation expirée
      ≠
Paiement échoué

Cette distinction est nécessaire pour préserver la cohérence des cycles métier.

14. Intégrité financière
ENF-039 — Préserver l'intégrité du solde organisateur

Priorité : CRITICAL

Eventix doit garantir qu'un organisateur ne puisse jamais retirer un montant supérieur à son solde disponible.

Cet invariant est explicitement défini dans le processus métier.

ENF-040 — Séparer disponibilité du billet et règlement organisateur

Priorité : CRITICAL

La disponibilité d'un billet pour le participant ne doit pas dépendre du règlement financier de l'organisateur.

Un billet correctement attribué après confirmation du paiement doit être utilisable sans attendre le règlement financier de l'organisateur.

15. Cohérence des statistiques
ENF-041 — Les statistiques doivent être cohérentes avec les données métier

Priorité : HIGH

Les statistiques doivent être produites à partir des données métier disponibles.

ENF-042 — Les statistiques ne doivent pas modifier les opérations métier

Priorité : CRITICAL

Une statistique ou une recommandation ne doit pas modifier directement :

une vente ;
un paiement ;
un billet.

Cette règle est explicitement définie dans les règles métier.

16. Expérience utilisateur
ENF-043 — Simplicité du parcours principal

Priorité : HIGH

Le parcours principal du participant doit rester suffisamment simple pour permettre :

Découvrir
   ↓
Choisir
   ↓
Réserver
   ↓
Payer
   ↓
Obtenir
   ↓
Utiliser

sans introduire de complexité inutile.

ENF-044 — Clarté des états importants

Priorité : HIGH

Les états importants doivent être présentés de manière compréhensible à l'utilisateur.

Cela concerne notamment :

réservation ;
paiement ;
billet ;
remboursement.
ENF-045 — Informer l'utilisateur des situations exceptionnelles

Priorité : HIGH

Lorsqu'une opération ne peut pas être finalisée normalement, Eventix doit fournir une information permettant au participant ou à l'organisateur de comprendre la situation et, lorsque possible, l'action à entreprendre.

17. Conservation et auditabilité
ENF-046 — Préserver l'historique lors des changements

Priorité : CRITICAL

Les changements futurs d'un événement ne doivent pas détruire l'historique des transactions existantes.

Par exemple :

Ancien prix
    ↓
Achat
    ↓
Nouveau prix

Le nouveau prix ne doit pas modifier le prix historiquement payé.

ENF-047 — Préserver les billets existants lors des modifications

Priorité : CRITICAL

Une modification future de la configuration d'un événement ne doit pas automatiquement invalider ou supprimer les billets déjà achetés.

18. Contraintes de complexité du MVP
ENF-048 — Privilégier la cohérence dans le MVP

Priorité : CRITICAL

Le MVP doit privilégier une architecture fonctionnelle cohérente plutôt qu'une distribution prématurée des opérations.

Les règles métier définissent notamment :

Connexion obligatoire
        ↓
Inventaire centralisé
        ↓
Pas de vente offline

La complexité distribuée est reportée jusqu'à ce qu'un besoin métier la justifie.

ENF-049 — Ne pas introduire prématurément le fonctionnement offline

Priorité : CRITICAL

Le MVP ne doit pas dépendre d'un fonctionnement offline pour les ventes.

Le mode offline constitue une évolution future.

ENF-050 — Ne pas introduire prématurément la Marketplace

Priorité : MEDIUM

La Marketplace de revente n'est pas une contrainte du MVP.

Elle sera traitée comme un domaine futur distinct avec ses propres exigences de qualité.

19. Confidentialité et protection des données
ENF-051 — Limiter l'accès aux données selon les responsabilités

Priorité : CRITICAL

Les données doivent être accessibles selon les responsabilités et autorisations de l'acteur.

Un participant ne doit pas accéder aux données internes d'un autre participant.

Un organisateur ne doit accéder qu'aux données auxquelles ses responsabilités lui donnent droit.

ENF-052 — Protéger les informations sensibles

Priorité : CRITICAL

Les informations sensibles relatives :

aux comptes ;
aux paiements ;
aux remboursements ;
aux décisions de sécurité ;
aux opérations financières

doivent être protégées contre les accès non autorisés.

Les mécanismes techniques précis seront définis ultérieurement.

20. Priorisation des qualités

Les qualités les plus critiques pour le MVP sont :

1. Cohérence
2. Fiabilité
3. Sécurité
4. Intégrité financière
5. Traçabilité
6. Disponibilité
7. Performance
8. Scalabilité
9. Maintenabilité

Cette hiérarchie ne signifie pas que les autres qualités sont secondaires dans l'architecture future.

Elle exprime les priorités résultant des règles métier actuelles.

Le choix le plus explicite est :

La cohérence et la fiabilité priment sur la performance lorsqu'elles entrent en conflit dans les opérations critiques.

21. Exigences quantitatives restant à préciser

Les sources métier ne définissent pas encore suffisamment de valeurs quantitatives pour plusieurs attributs de qualité.

Les valeurs suivantes doivent donc rester ouvertes :

Attribut	Valeur cible
Temps de réponse consultation	À préciser
Temps de réponse achat	À préciser
Temps de réponse contrôle	À préciser
Disponibilité globale	À préciser
Disponibilité pendant événements	À préciser
Nombre maximal d'utilisateurs simultanés	À préciser
Nombre maximal de contrôles/minute	À préciser
Nombre maximal de transactions/minute	À préciser
Temps maximal de reprise	À préciser
Volume maximal de remboursements traitables	À préciser

Ces valeurs ne doivent pas être inventées.

Elles devront être définies à partir :

des objectifs produit ;
des contraintes métier ;
des scénarios de charge ;
des hypothèses validées ;
puis des décisions d'architecture.
22. Questions encore ouvertes

Lorsqu'une exigence nécessite une décision qui n'est pas encore définie, elle reste explicitement ouverte.

Les principales valeurs quantitatives relatives à :

performance ;
disponibilité ;
charge ;
reprise ;
capacité

doivent être précisées dans :

03-decouverte-du-metier/questions-metier-ouvertes.md

Aucune valeur arbitraire ne doit être ajoutée à ce document pour masquer cette incertitude.

23. Principes transversaux

Les exigences non fonctionnelles d'Eventix peuvent être résumées ainsi :

                    COHÉRENCE
                        │
                        ↓
                   FIABILITÉ
                        │
                        ↓
                    SÉCURITÉ
                        │
                        ↓
                 TRAÇABILITÉ
                        │
                        ↓
                  DISPONIBILITÉ
                        │
                        ↓
                  PERFORMANCE
                        │
                        ↓
                  SCALABILITÉ

Avec une règle fondamentale :

Performance
     ↓
ne doit jamais
     ↓
détruire
     ↓
la cohérence métier
24. Relation avec les exigences fonctionnelles

Les exigences non fonctionnelles ne remplacent pas les exigences fonctionnelles.

Elles les contraignent.

Exemple :

EF — Contrôler un billet
        │
        ├── ENF — réponse suffisamment rapide
        ├── ENF — validation fiable
        ├── ENF — traçabilité
        ├── ENF — sécurité
        └── ENF — absence de double validation

Autre exemple :

EF — Confirmer un paiement
        │
        ├── ENF — idempotence
        ├── ENF — intégrité
        ├── ENF — traçabilité
        ├── ENF — reprise après erreur
        └── ENF — sécurité
25. Principe de faible couplage

Le faible couplage s'applique également aux exigences non fonctionnelles.

Une qualité transversale ne doit pas forcer un domaine à prendre en charge les responsabilités internes d'un autre.

Exemple :

Paiement
   │
   └── doit être fiable
          │
          ↓
Billetterie
   │
   └── doit être fiable
          │
          ↓
Contrôle
   │
   └── doit être fiable

La fiabilité est commune.

Les responsabilités restent séparées.

26. Critères de qualité du document

Les exigences non fonctionnelles doivent respecter les principes suivants :

être dérivées d'une source identifiable ;
être indépendantes des technologies ;
être vérifiables lorsque cela est possible ;
ne pas inventer de seuil non justifié ;
distinguer les contraintes métier des choix d'architecture ;
préserver la cohérence des cycles métier ;
préserver la sécurité ;
préserver la traçabilité ;
appliquer le principe de faible couplage ;
expliciter les décisions encore ouvertes.
27. Statut
Document : exigences-non-fonctionnelles.md


Version : 1.0
Statut : À valider par l'équipe
Périmètre : MVP Eventix
Marché : Cameroun


Cohérence : PRIORITAIRE
Fiabilité : PRIORITAIRE
Sécurité : PRIORITAIRE
Traçabilité : PRIORITAIRE


Seuils quantitatifs :
À préciser


Architecture technique :
Non décidée


### Point méthodologique important


J'ai volontairement **résisté à la tentation d'inventer des SLA du type « 99,9 % », « <200 ms », « 10 000 utilisateurs simultanés »**. Les sources disponibles établissent clairement les qualités recherchées — cohérence, fiabilité, traçabilité, sécurité, contrôle dégradé, etc. — mais ne fournissent pas encore ces seuils chiffrés. Les règles métier établissent par exemple explicitement que le MVP privilégie la cohérence et l'inventaire centralisé, avec connexion obligatoire et sans vente offline. :contentReference[oaicite:23]{index=23}


C'est important pour la suite : **on ne va pas transformer une hypothèse technique en exigence métier simplement parce qu'un chiffre paraît « professionnel ».** Les seuils seront décidés lorsqu'on aura les données nécessaires.