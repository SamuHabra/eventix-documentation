# Analyse des besoins — Eventix

> **Phase :** 04 — Analyse des besoins  
> **Statut :** En cours  
> **Projet :** Eventix  
> **Périmètre :** MVP

---

## 1. Objectif

L'analyse des besoins transforme les connaissances acquises pendant la Business Discovery en exigences structurées et vérifiables pour Eventix.

La Business Discovery nous a permis de comprendre :

- le métier de la billetterie ;
- les acteurs ;
- leurs objectifs ;
- les processus métier ;
- les parcours utilisateurs ;
- les problèmes et situations rencontrés ;
- les questions métier encore ouvertes.

L'analyse des besoins répond maintenant à une question différente :

> **Qu'est-ce qu'Eventix doit réellement fournir et garantir ?**

---

## 2. Position dans la démarche

```text
Business Discovery
        ↓
Comprendre le métier
        ↓
Analyse des besoins
        ↓
Formaliser les besoins et exigences
        ↓
DDD
        ↓
Modéliser le domaine
        ↓
UML
        ↓
Modéliser le système
        ↓
Architecture
        ↓
Concevoir le système


Cette phase constitue donc un pont entre la compréhension du métier et la conception du système.

3. Objectifs de la phase

Cette phase doit permettre de :

identifier les besoins fonctionnels ;
identifier les exigences non fonctionnelles ;
formaliser les attentes des utilisateurs ;
formaliser les interactions avec le système ;
définir les critères d'acceptation ;
identifier les contraintes ;
expliciter les hypothèses ;
prioriser les besoins ;
assurer la traçabilité des exigences.
4. Principes
4.1 Ne pas concevoir trop tôt

Cette phase ne définit pas encore :

l'architecture technique ;
les microservices ;
les bases de données ;
les API ;
les technologies ;
l'infrastructure ;
le déploiement.

Ces décisions appartiennent aux phases suivantes.

4.2 Séparer besoin et solution

Nous devons distinguer :

Besoin
  ↓
Exigence
  ↓
Solution

Exemple :

Besoin :
Le participant doit obtenir un billet après un paiement réussi.


Exigence :
Eventix doit émettre un billet lorsque le paiement est confirmé.


Solution technique :
À définir ultérieurement.
4.3 Les exigences doivent être vérifiables

Une exigence doit être suffisamment précise pour permettre de déterminer si elle est respectée.

Éviter :

Le système doit être rapide.

Préférer :

Le système doit permettre l'affichage de la disponibilité d'un événement dans un délai acceptable défini par les exigences de performance.

Les seuils précis seront définis lorsque les informations nécessaires seront disponibles.

4.4 Ne pas inventer les décisions métier

Lorsqu'une information n'a pas encore été décidée :

Question ouverte
       ↓
Hypothèse éventuelle
       ↓
Validation
       ↓
Exigence définitive

Une hypothèse ne doit pas être présentée comme une règle métier définitive.

Les questions non résolues restent référencées dans :

../03-business-discovery/questions-metier-ouvertes.md

5. Structure de la phase
04-analyse-des-besoins/
│
├── README.md
├── contraintes.md
├── criteres-d-acceptation.md
├── exigences-fonctionnelles.md
├── exigences-non-fonctionnelles.md
├── hypotheses.md
├── matrice-de-tracabilite.md
├── priorisation.md
├── use-cases.md
└── user-stories.md
6. Rôle des documents
exigences-fonctionnelles.md

Décrit les capacités que le système Eventix doit fournir.

Exemples :

créer un événement ;
rechercher un événement ;
réserver un billet ;
effectuer un paiement ;
obtenir un billet ;
contrôler un billet ;
demander un remboursement.
exigences-non-fonctionnelles.md

Décrit les qualités et contraintes de fonctionnement du système.

Exemples :

performance ;
disponibilité ;
sécurité ;
fiabilité ;
observabilité ;
maintenabilité ;
scalabilité.
user-stories.md

Exprime les besoins selon le point de vue des utilisateurs.

Format général :

En tant que [acteur],
je veux [objectif],
afin de [valeur].
use-cases.md

Décrit les interactions entre les acteurs et Eventix pour atteindre un objectif métier.

Exemples :

acheter un billet ;
créer un événement ;
contrôler un billet ;
annuler un événement ;
demander un remboursement.
criteres-d-acceptation.md

Définit les conditions permettant de considérer une fonctionnalité ou une exigence comme satisfaite.

Les critères doivent être observables et vérifiables.

contraintes.md

Documente les éléments qui limitent les choix possibles.

Exemples :

contraintes métier ;
contraintes géographiques ;
contraintes réglementaires ;
contraintes financières ;
contraintes organisationnelles ;
contraintes techniques déjà imposées.
hypotheses.md

Documente les hypothèses utilisées lorsque certaines informations ne sont pas encore confirmées.

Chaque hypothèse doit pouvoir être :

confirmée ;
infirmée ;
remplacée ;
transformée en décision.
priorisation.md

Détermine l'importance relative des besoins et exigences.

La priorisation permet notamment de distinguer :

indispensable au MVP ;
important ;
secondaire ;
futur.
matrice-de-tracabilite.md

Permet de suivre la relation entre les différents niveaux de spécification.

Exemple :

Besoin
  ↓
User Story
  ↓
Use Case
  ↓
Exigence
  ↓
Critère d'acceptation

L'objectif est d'éviter qu'une exigence importante soit oubliée ou qu'une fonctionnalité apparaisse sans justification métier.

7. Relations avec la Business Discovery

L'analyse des besoins s'appuie directement sur les documents précédents.

03-business-discovery/
        │
        ├── Acteurs
        │      ↓
        │   User Stories
        │
        ├── Processus métier
        │      ↓
        │   Use Cases
        │
        ├── User Journeys
        │      ↓
        │   Exigences
        │
        └── Questions ouvertes
               ↓
            Hypothèses

La Business Discovery reste la source de compréhension du métier.

8. Périmètre
Inclus dans le MVP

Le MVP couvre notamment :

gestion des comptes ;
gestion des organisateurs ;
création et gestion des événements ;
publication ;
découverte et consultation des événements ;
configuration des billets ;
réservation ;
paiement Mobile Money ;
obtention du billet ;
QR Code ;
contrôle des billets ;
annulation ;
report ;
remboursement ;
signalement ;
gestion des organisations ;
bannissement ;
clôture financière ;
retrait des fonds.
Hors MVP

Les fonctionnalités suivantes sont prévues pour des versions futures :

vente physique ;
points de vente ;
agents vendeurs ;
marketplace ;
revente de billets ;
quiz live ;
reels ;
photos / vidéos cloud ;
fonctionnalités sociales avancées ;
contrôle offline distribué avancé.
9. Décisions déjà établies

Certaines décisions provenant de la Business Discovery sont déjà prises et doivent être respectées pendant cette phase.

Sujet	Décision
Marché initial	Cameroun
Paiement MVP	Mobile Money
Frais de paiement	Supportés par l'organisateur
Commission Eventix	Pourcentage sur les ventes
Durée réservation	5 minutes
Paiement tardif	Réconciliation
Transfert de billet	Autorisé gratuitement
Revente	Hors MVP
Sièges numérotés	Supportés ; choix interactif et réservation d'une place individuelle dans le périmètre du MVP
Contrôle	Priorité à la fiabilité
Fonds organisateur	Disponibles après événement + clôture
Retrait partiel	Autorisé
Retrait échoué	Fonds restitués au solde

Les détails qui n'ont pas encore été décidés restent ouverts.

10. Gestion des changements

Les exigences peuvent évoluer pendant le projet.

Toute modification importante doit être :

identifiée ;
justifiée ;
évaluée ;
documentée ;
propagée aux documents concernés.

Exemple :

Modification d'une règle métier
        ↓
Exigence concernée
        ↓
User Story
        ↓
Use Case
        ↓
Critères d'acceptation
        ↓
Matrice de traçabilité
11. Critère de fin de phase

L'analyse des besoins sera considérée comme suffisamment complète lorsque :

les besoins fonctionnels principaux sont identifiés ;
les exigences non fonctionnelles importantes sont identifiées ;
les User Stories sont documentées ;
les Use Cases principaux sont documentés ;
les critères d'acceptation sont définis ;
les contraintes sont explicites ;
les hypothèses sont identifiées ;
les besoins sont priorisés ;
la traçabilité est établie ;
aucune exigence critique n'est laissée sans justification.
12. Résultat attendu

À la fin de cette phase, nous devons disposer d'une réponse claire à la question :

"Que doit faire Eventix et quelles qualités doit-il garantir ?"

Sans encore répondre à :

"Comment allons-nous techniquement construire Eventix ?"
