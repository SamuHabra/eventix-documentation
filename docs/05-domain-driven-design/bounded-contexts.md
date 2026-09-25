# Bounded Contexts — Eventix

| Champ | Valeur |
|---|---|
| **Phase** | 05 — Domain-Driven Design |
| **Projet** | Eventix |
| **Périmètre** | MVP |
| **Marché initial** | Cameroun |
| **Statut** | Version de référence — à valider par l'équipe |

---

## Table des matières

1. [Objectif](#1-objectif)
2. [Sources de référence](#2-sources-de-référence)
3. [Principes de délimitation](#3-principes-de-délimitation)
4. [Convention d'identification](#4-convention-didentification)
5. [Bounded Contexts principaux](#5-bounded-contexts-principaux)
6. [Bounded Contexts de soutien](#6-bounded-contexts-de-soutien)
7. [Bounded Contexts génériques](#7-bounded-contexts-génériques)
8. [Cartographie des relations](#8-cartographie-des-relations)
9. [Contextes partagés et partenariats](#9-contextes-partagés-et-partenariats)
10. [Alignement avec le core domain](#10-alignement-avec-le-core-domain)
11. [Ce que les bounded contexts ne préjugent pas](#11-ce-que-les-bounded-contexts-ne-préjugent-pas)
12. [Résumé](#12-résumé)
13. [Critères de qualité du document](#13-critères-de-qualité-du-document)
14. [Statut](#14-statut)

---

# 1. Objectif

Ce document délimite les frontières logiques autour des sous-domaines définis dans `sous-domaines.md` et autour des sous-domaines cœur identifiés dans `core-domaines.md`. Pour chaque bounded context, il précise :

- la frontière logique et son contenu en sous-domaines ;
- les responsabilités propres au contexte ;
- les relations avec les contextes voisins ;
- le statut de différenciation (cœur, soutien, générique).

Il ne redéfinit ni les sous-domaines, ni les promesses, ni les règles métier déjà établies dans les sources. Ces éléments sont utilisés par référence et supposés connus du lecteur.

---

# 2. Sources de référence

Ce document est dérivé principalement de :

- `05-domain-driven-design/sous-domaines.md` — découpage en trente-six sous-domaines
- `05-domain-driven-design/core-domaines.md` — classification cœur / soutien / générique

Toute définition, règle métier ou exigence mentionnée implicitement renvoie à ces documents sources.

---

# 3. Principes de délimitation

## 3.1. Une frontière par cohérence métier

Un bounded context regroupe les sous-domaines qui partagent un même langage ubiquitaire, les mêmes invariants métier et une même responsabilité fonctionnelle. La frontière suit la logique métier, pas la logique technique.

## 3.2. Une frontière par cycle de vie

Les sous-domaines regroupés dans un même contexte partagent un cycle de vie cohérent : ils sont créés, modifiés et archivés ensemble, ou leur enchaînement est si étroit qu'aucune frontière intermédiaire n'aurait de sens métier.

## 3.3. Une frontière par niveau de différenciation

Les sous-domaines de même catégorie (cœur, soutien, générique) sont regroupés autant que possible. Un contexte ne mélange pas de manière arbitraire des sous-domaines cœur et génériques sans justification métier explicite.

## 3.4. Une frontière minimale

Les bounded contexts sont aussi petits que possible tout en préservant la cohérence métier. Une frontière plus large que nécessaire crée du couplage ; une frontière plus petite crée de la fragmentation.

## 3.5. Une frontière explicite

Chaque bounded context possède une frontière clairement délimitée, documentée et vérifiable. Les ambiguïtés de frontière sont des sources de bugs métier.

---

# 4. Convention d'identification

Chaque bounded context possède un identifiant unique :

| Identifiant | Bounded Context | Catégorie |
|---|---|---|
| `BC-01` | Identity & Access Management | Générique |
| `BC-02` | Event Catalog | Cœur |
| `BC-03` | Event Discovery | Cœur |
| `BC-04` | Booking & Availability | Soutien |
| `BC-05` | Payment Processing | Générique |
| `BC-06` | Ticketing & Fulfillment | Cœur |
| `BC-07` | Access Control | Cœur |
| `BC-08` | Financial Settlement | Soutien |
| `BC-09` | Refund Management | Soutien |
| `BC-10` | Trust & Safety | Générique |
| `BC-11` | Analytics & Observability | Cœur |
| `BC-12` | Communication | Générique |

---

# 5. Bounded Contexts principaux

## 5.1. BC-02 — Event Catalog

| Champ | Valeur |
|---|---|
| **Catégorie** | Cœur |
| **Sous-domaines inclus** | SD-02-1, SD-02-2, SD-02-3, SD-02-4 |
| **Domaine parent** | CATALOG |

### Frontière logique

Ce contexte englobe tout ce qui concerne la création, la configuration et le cycle de vie des événements. Il constitue le référentiel central des événements et de leur configuration.

### Responsabilités propres

- Créer et configurer les événements.
- Piloter les transitions d'état de l'événement.
- Décrire les espaces, zones et capacités.
- Définir les catégories de billets et la disponibilité courante.
- Conserver la trace des opérations de configuration.

### Relations avec les contextes voisins

- **Reçoit de** : BC-01 (organisateur autorisé), BC-10 (décisions de sécurité).
- **Fournit à** : BC-03 (événements publiés), BC-04 (catégories et disponibilités), BC-06 (événements pour émission de billets), BC-07 (événements et points d'entrée pour contrôle).

### Justification de la frontière

Les quatre sous-domaines de CATALOG partagent le même langage (événement, espace, zone, capacité, catégorie), les mêmes invariants (capacité non réductible, historique non réécrit) et un cycle de vie commun (création → configuration → publication → archivage). Les séparer créerait une fragmentation artificielle.

---

## 5.2. BC-03 — Event Discovery

| Champ | Valeur |
|---|---|
| **Catégorie** | Cœur |
| **Sous-domaines inclus** | SD-03-1, SD-03-2, SD-03-3 |
| **Domaine parent** | DISCOVERY |

### Frontière logique

Ce contexte englobe tout ce qui concerne la recherche, le filtrage et la consultation des événements par les participants. Il constitue la porte d'entrée du parcours participant.

### Responsabilités propres

- Exposer le catalogue des événements publiés.
- Transformer les critères de recherche en sélection.
- Présenter le détail des événements et la disponibilité visible.

### Relations avec les contextes voisins

- **Reçoit de** : BC-02 (événements publiés et disponibilités).
- **Fournit à** : Participant (parcours de découverte), BC-04 (demande de réservation).

### Justification de la frontière

Les trois sous-domaines de DISCOVERY forment un parcours utilisateur cohérent (recherche → filtrage → consultation) qui ne partage pas de langage avec les contextes de gestion interne. La séparation avec BC-02 est nette : BC-02 gère, BC-03 expose.

---

## 5.3. BC-06 — Ticketing & Fulfillment

| Champ | Valeur |
|---|---|
| **Catégorie** | Cœur |
| **Sous-domaines inclus** | SD-06-1, SD-06-2, SD-06-3, SD-06-4 |
| **Domaine parent** | TICKETING |

### Frontière logique

Ce contexte englobe tout ce qui concerne la finalisation des achats, l'émission des billets, la gestion de leur propriété et leur consultation par les participants. Il constitue le cœur de la promesse « obtenir ses billets simplement ».

### Responsabilités propres

- Finaliser les achats lorsque les conditions sont réunies.
- Émettre les billets avec leur QR Code.
- Gérer le propriétaire actif unique et les transferts.
- Permettre la consultation et le téléchargement des billets.

### Relations avec les contextes voisins

- **Reçoit de** : BC-05 (paiement confirmé), BC-02 (événement et catégorie).
- **Fournit à** : BC-07 (billets à contrôler), BC-12 (billets à distribuer), Participant (billets consultables).

### Justification de la frontière

Les quatre sous-domaines de TICKETING partagent l'invariant de l'unicité du propriétaire actif et le cycle de vie du billet (émission → consultation → transfert → utilisation). Les séparer fragmenterait une responsabilité métier unique.

---

## 5.4. BC-07 — Access Control

| Champ | Valeur |
|---|---|
| **Catégorie** | Cœur |
| **Sous-domaines inclus** | SD-07-1, SD-07-2, SD-07-3, SD-07-4 |
| **Domaine parent** | ACCESS |

### Frontière logique

Ce contexte englobe tout ce qui concerne le contrôle des billets à l'entrée des événements, y compris le fonctionnement en mode dégradé et la réintégration après resynchronisation. Il constitue le cœur de la promesse de contrôle d'accès pour l'organisateur.

### Responsabilités propres

- Lire les QR Codes des billets.
- Décider de l'acceptation ou du refus d'un billet.
- Garantir l'usage unique de chaque billet.
- Gérer le mode dégradé à scanner unique.
- Réintégrer les opérations après resynchronisation.

### Relations avec les contextes voisins

- **Reçoit de** : BC-06 (billets émis), BC-02 (événement et points d'entrée).
- **Fournit à** : BC-11 (événements de présence), BC-02 (statut des billets utilisés).

### Justification de la frontière

Les quatre sous-domaines de ACCESS partagent l'invariant de l'unicité de la validation et le même environnement opérationnel (entrée d'événement, connectivité variable). Le mode dégradé et sa réintégration sont des responsabilités métier spécifiques à ce contexte.

---

## 5.5. BC-11 — Analytics & Observability

| Champ | Valeur |
|---|---|
| **Catégorie** | Cœur |
| **Sous-domaines inclus** | SD-11-1, SD-11-2, SD-11-3 |
| **Domaine parent** | OBSERVATION |

### Frontière logique

Ce contexte englobe tout ce qui concerne la collecte des événements métier, la production de statistiques et le maintien de l'historique des opérations. Il constitue le cœur de la promesse « mieux comprendre ses événements » pour l'organisateur.

### Responsabilités propres

- Recueillir les faits marquants de tous les domaines.
- Produire les agrégats d'activité en lecture seule.
- Maintenir le journal chronologique des opérations importantes.

### Relations avec les contextes voisins

- **Reçoit de** : tous les bounded contexts (événements métier).
- **Fournit à** : Organisateur (statistiques et historique), Eventix (pilotage).

### Justification de la frontière

Les trois sous-domaines de OBSERVATION partagent la même nature de données (faits marquants, agrégats, journal) et la même contrainte de lecture seule. Les séparer créerait une fragmentation artificielle entre collecte, analyse et traçabilité.

---

# 6. Bounded Contexts de soutien

## 6.1. BC-04 — Booking & Availability

| Champ | Valeur |
|---|---|
| **Catégorie** | Soutien |
| **Sous-domaines inclus** | SD-04-1, SD-04-2, SD-04-3, SD-04-4 |
| **Domaine parent** | BOOKING |

### Frontière logique

Ce contexte englobe tout ce qui concerne la création, l'expiration et l'arbitrage des réservations temporaires. Il conditionne l'achat fluide sans distinguer Eventix d'un concurrent.

### Responsabilités propres

- Enregistrer les demandes de réservation et opérer le blocage temporaire.
- Libérer les disponibilités au terme du délai ou sur demande.
- Garantir qu'une même disponibilité n'est jamais attribuée simultanément à plusieurs achats valides.

### Relations avec les contextes voisins

- **Reçoit de** : BC-03 (demande de réservation), BC-02 (disponibilités).
- **Fournit à** : BC-05 (réservation valide à payer).

### Justification de la frontière

Les quatre sous-domaines de BOOKING partagent l'invariant de l'unicité d'attribution et le cycle de vie de la réservation (création → blocage → expiration ou annulation). L'arbitre unique de l'attribution est une responsabilité métier spécifique à ce contexte.

---

## 6.2. BC-08 — Financial Settlement

| Champ | Valeur |
|---|---|
| **Catégorie** | Soutien |
| **Sous-domaines inclus** | SD-08-1, SD-08-2, SD-08-3 |
| **Domaine parent** | FINANCE |

### Frontière logique

Ce contexte englobe tout ce qui concerne la clôture financière, la tenue du solde et les retraits organisateur. Il prolonge la visibilité promise sans la différencier.

### Responsabilités propres

- Déterminer le montant net de la clôture.
- Tenir le solde retirable de l'organisateur.
- Gérer les demandes de retrait et les restitutions d'échecs.

### Relations avec les contextes voisins

- **Reçoit de** : BC-06 (achats finalisés), BC-09 (remboursements traités).
- **Fournit à** : Organisateur (solde disponible).

### Justification de la frontière

Les trois sous-domaines de FINANCE partagent le même langage (clôture, montant net, solde, retrait) et le même cycle de vie (clôture → solde → retrait). Les séparer fragmenterait une responsabilité financière unique.

---

## 6.3. BC-09 — Refund Management

| Champ | Valeur |
|---|---|
| **Catégorie** | Soutien |
| **Sous-domaines inclus** | SD-09-1, SD-09-2, SD-09-3 |
| **Domaine parent** | REFUND |

### Frontière logique

Ce contexte englobe tout ce qui concerne la détermination, le calcul et l'exécution des remboursements. Il constitue une garantie de confiance comparable au marché.

### Responsabilités propres

- Qualifier les situations entraînant une obligation de rembourser.
- Calculer le montant de référence et créer le remboursement unique.
- Exécuter les versements, les reprises après échec et le traitement progressif.

### Relations avec les contextes voisins

- **Reçoit de** : BC-05 (paiement confirmé à rembourser), BC-06 (billets concernés).
- **Fournit à** : BC-08 (remboursements traités pour clôture).

### Justification de la frontière

Les trois sous-domaines de REFUND partagent l'invariant de l'unicité du remboursement effectif et le cycle de vie de l'obligation (détermination → création → exécution). Les séparer fragmenterait une responsabilité métier unique.

---

# 7. Bounded Contexts génériques

## 7.1. BC-01 — Identity & Access Management

| Champ | Valeur |
|---|---|
| **Catégorie** | Générique |
| **Sous-domaines inclus** | SD-01-1, SD-01-2, SD-01-3, SD-01-4 |
| **Domaine parent** | IDENTITY |

### Frontière logique

Ce contexte englobe tout ce qui concerne la gestion des comptes, l'autorisation organisateur, la gestion des organisations et la sécurité interne. Il constitue un préalable à tout parcours sans lien avec une promesse distinctive.

### Responsabilités propres

- Créer et maintenir les comptes utilisateurs.
- Gérer la demande, l'octroi et la révocation de la capacité organisateur.
- Créer les organisations et conduire leur cycle de vérification.
- Cloisonner les données et décisions de sécurité.

### Relations avec les contextes voisins

- **Reçoit de** : BC-10 (mesures de sécurité).
- **Fournit à** : tous les bounded contexts (identité des acteurs).

### Justification de la frontière

Les quatre sous-domaines de IDENTITY partagent le même langage (compte, capacité, organisation, sécurité) et le même cycle de vie (création → autorisation → vérification → restriction). Les séparer fragmenterait une responsabilité transversale unique.

---

## 7.2. BC-05 — Payment Processing

| Champ | Valeur |
|---|---|
| **Catégorie** | Générique |
| **Sous-domaines inclus** | SD-05-1, SD-05-2, SD-05-3, SD-05-4 |
| **Domaine parent** | PAYMENT |

### Frontière logique

Ce contexte englobe tout ce qui concerne l'initiation, le suivi, le traitement des échecs et la réconciliation des paiements. Le Mobile Money est le moyen imposé par le marché, non un choix distinctif.

### Responsabilités propres

- Démarrer les opérations de paiement Mobile Money.
- Recevoir les accusés et garantir l'effet unique de la confirmation.
- Enregistrer et traiter les échecs.
- Traiter les paiements tardifs et déterminer leur issue.

### Relations avec les contextes voisins

- **Reçoit de** : BC-04 (réservation valide).
- **Fournit à** : BC-06 (paiement confirmé pour émission), BC-09 (paiement à rembourser).

### Justification de la frontière

Les quatre sous-domaines de PAYMENT partagent le même langage (paiement, confirmation, échec, réconciliation) et le même cycle de vie (initiation → suivi → confirmation ou échec → réconciliation éventuelle). Les séparer fragmenterait une responsabilité métier unique.

---

## 7.3. BC-10 — Trust & Safety

| Champ | Valeur |
|---|---|
| **Catégorie** | Générique |
| **Sous-domaines inclus** | SD-10-1, SD-10-2, SD-10-3 |
| **Domaine parent** | TRUST & SAFETY |

### Frontière logique

Ce contexte englobe tout ce qui concerne la réception des signalements, l'analyse de risque et les décisions de sécurité. Il constitue un attendu de toute plateforme ouverte.

### Responsabilités propres

- Enregistrer les signalements des participants.
- Évaluer le niveau de risque identifié.
- Choisir, appliquer et tracer les mesures de sécurité.

### Relations avec les contextes voisins

- **Reçoit de** : Participant (signalements), tous les bounded contexts (éléments d'analyse).
- **Fournit à** : BC-01 (mesures sur comptes et organisations), BC-02 (mesures sur événements).

### Justification de la frontière

Les trois sous-domaines de TRUST & SAFETY partagent le même langage (signalement, risque, mesure) et le même cycle de vie (réception → analyse → décision). Les séparer fragmenterait une responsabilité métier unique.

---

## 7.4. BC-12 — Communication

| Champ | Valeur |
|---|---|
| **Catégorie** | Générique |
| **Sous-domaines inclus** | SD-12-1, SD-12-2, SD-12-3 |
| **Domaine parent** | COMMUNICATION |

### Frontière logique

Ce contexte englobe tout ce qui concerne la mise à disposition des billets, leur distribution par email et les notifications d'événement. Il constitue un canal standard interchangeable.

### Responsabilités propres

- Rendre les billets accessibles depuis le compte participant.
- Distribuer les billets par email.
- Informer les participants des annulations et reports.

### Relations avec les contextes voisins

- **Reçoit de** : BC-06 (billets émis), BC-02 (événements annulés ou reportés).
- **Fournit à** : Participant (communications).

### Justification de la frontière

Les trois sous-domaines de COMMUNICATION partagent le même langage (mise à disposition, distribution, notification) et la même nature de responsabilité (transmission d'informations). Les séparer fragmenterait une responsabilité métier unique.

---

# 8. Cartographie des relations

```text
                        ┌─────────────┐
                        │    BC-01    │
                        │  IDENTITY   │
                        └──────┬──────┘
                               │
                               ▼
                        ┌─────────────┐
                        │    BC-02    │
                        │   CATALOG   │
                        └──────┬──────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
              ▼                ▼                ▼
        ┌──────────┐     ┌──────────┐     ┌──────────┐
        │   BC-03  │     │   BC-04  │     │   BC-10  │
        │ DISCOVERY│     │ BOOKING  │     │  TRUST   │
        └────┬─────┘     └────┬─────┘     └────┬─────┘
             │                │                │
             │                ▼                │
             │           ┌──────────┐          │
             │           │   BC-05  │          │
             │           │ PAYMENT  │          │
             │           └────┬─────┘          │
             │                │                │
             │                ▼                │
             │           ┌──────────┐          │
             │           │   BC-06  │          │
             │           │ TICKETING│          │
             │           └────┬─────┘          │
             │                │                │
             │                ▼                │
             │           ┌──────────┐          │
             │           │   BC-07  │          │
             │           │  ACCESS  │          │
             │           └────┬─────┘          │
             │                │                │
             │                ▼                │
             │           ┌──────────┐          │
             │           │   BC-09  │          │
             │           │  REFUND  │          │
             │           └────┬─────┘          │
             │                │                │
             │                ▼                │
             │           ┌──────────┐          │
             │           │   BC-08  │          │
             │           │ FINANCE  │          │
             │           └────┬─────┘          │
             │                │                │
             ▼                ▼                ▼
        ┌──────────┐     ┌──────────┐     ┌──────────┐
        │   BC-12  │     │   BC-11  │     │   BC-01  │
        │COMMUNIC. │     │ANALYTICS │     │ (retour) │
        └──────────┘     └──────────┘     └──────────┘



 9. Contextes partagés et partenariats
9.1. Contexte partagé : Événement
Le concept d'événement est partagé entre plusieurs bounded contexts. Chaque contexte possède une vue différente du même événement :

| Contexte | Vue de l'événement                                                          |
| -------- | --------------------------------------------------------------------------- |
| BC-02    | Événement complet avec configuration, espaces, catégories et cycle de vie   |
| BC-03    | Événement publié avec informations de consultation et disponibilité visible |
| BC-06    | Événement associé à un billet émis                                          |
| BC-07    | Événement contrôlé avec points d'entrée                                     |
| BC-11    | Événement source de statistiques et d'historique                            |

9. Contextes partagés et partenariats
9.1. Contexte partagé : Événement
Le concept d'événement est partagé entre plusieurs bounded contexts. Chaque contexte possède une vue différente du même événement :
Table
Contexte	Vue de l'événement
BC-02	Événement complet avec configuration, espaces, catégories et cycle de vie
BC-03	Événement publié avec informations de consultation et disponibilité visible
BC-06	Événement associé à un billet émis
BC-07	Événement contrôlé avec points d'entrée
BC-11	Événement source de statistiques et d'historique
Cette multiplicité de vues est légitime en DDD : chaque contexte possède son propre modèle de l'événement, adapté à ses besoins. La cohérence est assurée par les événements métier émis par BC-02.
9.2. Contexte partagé : Billet
Le concept de billet est partagé entre plusieurs bounded contexts :
Table
Contexte	Vue du billet
BC-06	Billet émis avec propriétaire actif unique et QR Code
BC-07	Billet à contrôler avec état de validation
BC-12	Billet à distribuer au participant
9.3. Partenariats
Table
Partenariat	Nature	Justification
BC-02 ↔ BC-10	BC-10 fournit des décisions de sécurité à BC-02	Les mesures de sécurité portent sur les événements sans en faire partie
BC-05 ↔ BC-09	BC-05 déclenche des remboursements vers BC-09	La réconciliation des paiements tardifs peut aboutir à un remboursement
BC-07 ↔ BC-11	BC-07 fournit les événements de présence à BC-11	Les statistiques d'entrée alimentent l'analyse
BC-06 ↔ BC-12	BC-06 fournit les billets à BC-12	La distribution est une conséquence de l'émission
10. Alignement avec le core domain
10.1. Bounded contexts cœur
Cinq bounded contexts sont classés cœur :
Table
Bounded Context	Sous-domaines cœur	Promesses associées
BC-02	SD-02-1, SD-02-2, SD-02-3	Gestion — « mieux gérer leurs événements »
BC-03	SD-03-2, SD-03-3	Découverte — recherche selon intérêts et localisation
BC-06	SD-06-1, SD-06-2	Billetterie — obtention simple des billets
BC-07	SD-07-2, SD-07-3, SD-07-4	Accès — contrôle des accès et continuité locale
BC-11	SD-11-2	Analyse — « mieux comprendre leurs événements »
10.2. Sous-domaines cœur local
Trois sous-domaines sont classés cœur local par inférence du contexte camerounais :
Table
Sous-domaine	Bounded Context	Justification locale
SD-05-4	BC-05	Réconciliation des paiements tardifs — réalité du marché camerounais
SD-07-3	BC-07	Mode dégradé — contraintes de connectivité locales
SD-07-4	BC-07	Réintégration après resynchronisation — cohérence retrouvée
Ces sous-domaines sont intégrés dans des bounded contexts génériques ou cœur sans créer de contexte dédié. Cette intégration est justifiée par leur étroite relation avec les responsabilités métier de leur contexte d'accueil.
11. Ce que les bounded contexts ne préjugent pas
La délimitation des bounded contexts ne préjuge pas :
de l'architecture technique (monolithe, microservices, etc.) ;
des technologies de développement ;
des bases de données ;
des frameworks ;
des API ;
de l'infrastructure ;
des mécanismes de déploiement.
Un bounded context peut être implémenté comme un module dans un monolithe, comme un microservice, ou comme une combinaison des deux. Cette décision relève des phases ultérieures.
12. Résumé
Ce document délimite douze bounded contexts qui regroupent les trente-six sous-domaines d'Eventix. Cinq bounded contexts sont classés cœur, trois sont classés soutien, et quatre sont classés génériques. Les frontières suivent la cohérence métier, le cycle de vie et le niveau de différenciation. Les relations entre contextes sont explicites et minimales. Aucune décision technique n'est prise ou implicite.
13. Critères de qualité du document
Ce document doit respecter les propriétés suivantes :
chaque bounded context possède une frontière logique clairement délimitée ;
les sous-domaines inclus dans chaque contexte sont explicitement listés ;
les relations avec les contextes voisins sont explicites ;
l'alignement avec la classification cœur / soutien / générique est vérifiable ;
aucune décision technique n'est prise ou implicite.
14. Statut
Table
Champ	Valeur
Document	bounded-contexts.md
Version	1.0
Statut	À valider par l'équipe
Périmètre	MVP Eventix
Marché	Cameroun
Table
Principe	État
Frontière logique par cohérence métier	✅ APPLIQUÉ
Frontière par cycle de vie	✅ APPLIQUÉ
Frontière par niveau de différenciation	✅ APPLIQUÉ
Relations explicites entre contextes	✅ DÉFINIES
Alignement avec le core domain	✅ VÉRIFIÉ
Décisions techniques	⏳ NON PRÉJUGÉES
