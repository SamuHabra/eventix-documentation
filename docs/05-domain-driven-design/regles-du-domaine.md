# Règles du domaine — Eventix

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
3. [Méthode de traduction](#3-méthode-de-traduction)
4. [Règles traduites en invariants de frontière](#4-règles-traduites-en-invariants-de-frontière)
5. [Règles traduites en enchaînements d'événements](#5-règles-traduites-en-enchaînements-dévénements)
6. [Règles traduites en politiques](#6-règles-traduites-en-politiques)
7. [Principes fondamentaux](#7-principes-fondamentaux)
8. [Écarts de traduction](#8-écarts-de-traduction)
9. [Statut](#9-statut)

---

# 1. Objectif

Ce document traduit formellement les règles métier validées (`regles-metier.md`) au niveau du modèle de domaine établi dans `agregats.md` et `evenements-de-domaine.md`. Pour chaque règle, il précise :

- le mécanisme du modèle qui la fait respecter (invariant de frontière, enchaînement d'événements, service de coordination ou politique) ;
- les frontières, événements et services impliqués, référencés par leur nom dans les sources ;
- le traitement requis lorsque la règle déborde d'une seule frontière.

Il ne répète ni les énoncés des règles métier, ni les définitions des agrégats, événements ou services. Ces éléments sont utilisés par référence et supposés connus du lecteur.

---

# 2. Sources de référence

Ce document est dérivé de :

- `03-.../regles-metier.md` — règles métier validées (RM01 à RM33, principes P01 à P06, règle multi-scanners)
- `05-domain-driven-design/agregats.md` — frontières de cohérence et invariants de garantie
- `05-domain-driven-design/evenements-de-domaine.md` — événements et services de coordination

Tout énoncé de règle, définition d'agrégat, d'invariant, d'événement ou de service mentionné implicitement renvoie à ces documents sources.

---

# 3. Méthode de traduction

Une règle métier se traduit par exactement un des quatre mécanismes suivants :

| Mécanisme | Condition |
|---|---|
| **Invariant de frontière** | La règle porte sur des données qu'un seul agrégat contrôle ; elle est garantie à la fin de chaque opération sur cet agrégat |
| **Enchaînement d'événements** | La règle traverse plusieurs frontières ; elle est réalisée par la séquence d'événements qui propage les transitions d'un agrégat à l'autre |
| **Service de domaine** | La règle exige la lecture ou la coordination de plusieurs agrégats ; aucune frontière unique ne peut la garantir seule |
| **Politique** | La règle n'est pas une contrainte de cohérence mais un choix d'exploitation ou de périmètre ; elle s'impose par convention, non par invariant |

Lorsqu'une règle combine plusieurs mécanismes, la traduction indique le mécanisme principal puis les compléments.

---

# 4. Règles traduites en invariants de frontière

Ces règles sont garanties sans coordination externe, par la frontière qui les porte.

| Règle | Frontière de garantie | Formalisation |
|---|---|---|
| RM01 | Agrégat Réservation + Agrégat Disponibilité | Le blocage n'existe que comme membre de la création de la réservation ; aucune réservation n'est créée sans le blocage correspondant dans la même opération |
| RM02 | Agrégat Réservation | L'expiration est portée par la frontière (délai membre de l'agrégat) ; la libération est propagée par `RéservationExpirée` → `DisponibilitéLibérée` |
| RM05 | Agrégat Paiement | L'invariant d'unicité d'effet est déclaré dans la frontière ; le doublon est absorbé sans effet (`FinalisationIdempotente`) |
| RM06 | Agrégat Billet | L'invariant du propriétaire actif unique est assuré à la frontière ; aucun transfert ne se conclut sans remplacement du propriétaire |
| RM12 | Agrégat Disponibilité + Agrégat Achat | Un billet gratuit produit le même décrément qu'un achat payé : `AchatFinalisé` → `DisponibilitéDécrémentée`, sans passer par un paiement |
| RM13 | Agrégat Billet | L'invariant de non-validation d'un billet déjà validé est assuré à la frontière |
| RM14 | Agrégat Billet (avec Agrégat Point d'entrée comme source de décision) | La décision remonte à la seule frontière habilitée à faire évoluer l'état du billet ; la séquentialité des tentatives est garantie par cette unicité de décision |
| RM16 | Agrégat Point d'entrée | Le contrôle et la présence, membres de la même frontière, naissent cohérents ; le contexte est porté par les objets de valeur `Résultat de contrôle` et `Point et moment de passage` |
| RM17 | Séparation des frontières | La distinction est structurelle : le billet vit dans sa frontière, la présence dans celle du point d'entrée ; aucune référence ne permet de déduire l'une de l'autre |
| RM21 | Agrégat Obligation de remboursement | Deux invariants de frontière : unicité du remboursement effectif ; plafonnement au montant de référence |
| RM23 | Agrégat Événement + indépendance de l'Agrégat Billet | La modification de configuration n'émet aucun événement vers les billets ; la conservation est l'absence même de propagation |
| RM24 | Agrégat Événement | L'invariant de non-réduction sous les billets attribués est assuré à la frontière ; la lecture des attributions se fait par identité depuis l'Agrégat Disponibilité, en séquentiel avant toute décision |
| RM25 | Séparation des frontières | L'utilisabilité du billet découle de l'enchaînement émission ; elle est indépendante de l'Agrégat Clôture par construction, aucune référence n'existe entre Billet et Clôture |
| RM27 | Agrégat Billet | Le transfert est un membre de la frontière (`Transfert`) ; l'historique de propriété est reconstruit à l'intérieur, cohérent avec l'unicité du propriétaire actif |
| RM29 | Agrégat Mesure de sécurité | Le niveau de risque est un objet de valeur de la frontière ; la décision de mesure en découle à l'intérieur du même agrégat |
| RM33 | Agrégat Statistique | L'invariant est négatif et structurel : absence de toute référence sortante ; une statistique ne peut pas modifier ce qu'elle ne référence pas |

---

# 5. Règles traduites en enchaînements d'événements

Ces règles traversent plusieurs frontières ; leur formalisation est la séquence d'événements qui les réalise.

## 5.1. Cycle distincts et finalisation (RM03, RM07)

| Règle | Enchaînement formalisé |
|---|---|
| RM03 | `RéservationCréée` → `PaiementInitié` → `PaiementConfirmé` → `AchatFinalisé` → `BilletÉmis` : quatre frontières successives, aucune ne déduisant l'état d'une autre |
| RM07 | Le prix applicable est figé au moment de la finalisation : l'objet de valeur `Montant de l'achat` de l'Agrégat Achat est la seule référence du montant payé ; aucune modification ultérieure de l'Agrégat Événement (émetteur de `ConfigurationModifiée` ou `CapacitéModifiée`) n'émet d'événement vers les achats |

## 5.2. Expiration et réconciliation (RM04)

| Règle | Enchaînement formalisé |
|---|---|
| RM04 | `RéservationExpirée` → (`PaiementConfirmé` tardif) → `RéconciliationDébutée` → `RéconciliationConclue` → branche `BilletÉmis` **ou** `ObligationDeRemboursementCréée`. L'issue est déterminée par le ServiceDeReconciliation ; l'agrégat Réconciliation porte la décision, la branche billet ou remboursement est exécutée dans la frontière cible |

## 5.3. Annulation et remboursements (RM19, RM20)

| Règle | Enchaînement formalisé |
|---|---|
| RM19 | `ÉvénementAnnulé` → `RemboursementsDéclenchés` → `ObligationDeRemboursementCréée` : le ServiceDAnnulationDevenement propage l'annulation vers la création des obligations ; l'éligibilité est portée par la détermination dans l'Agrégat Obligation |
| RM20 | `RemboursementCréé` → (`RemboursementEffectué` ou `RemboursementÉchoué` → `RemboursementRetenté` → …) : le traitement progressif est la répétition contrôlée de cette séquence, l'unicité d'exécution restant garantie par l'invariant de frontière RM21 |

## 5.4. Report (RM22)

| Règle | Enchaînement formalisé |
|---|---|
| RM22 | `ÉvénementReporté` se propage vers les participants via le ServiceDeReportDevenement ; aucun événement n'est émis vers l'Agrégat Billet : la validité conservée est l'absence de transition |

## 5.5. Règlement différé (RM26)

| Règle | Enchaînement formalisé |
|---|---|
| RM26 | `ÉvénementTerminé` → `ClôtureEffectuée` → `SoldeAlimenté` → (`RetraitDemandé` → `RetraitEffectué` ou `RetraitÉchoué` → `RetraitRestitué`) : la disponibilité des fonds est bornée par la position du ServiceDeClotureFinanciere ; l'interdiction de dépasser le solde est l'invariant de l'Agrégat Solde organisateur |

## 5.6. Contrôle d'accès et mode dégradé (règle multi-scanners, RM13–RM14 en exploitation)

| Règle | Enchaînement formalisé |
|---|---|
| Multi-scanners | `ModeDégradéActivé` / `ModeDégradéDésactivé` bornent les périodes : pendant l'activation, un seul point d'entrée émet des décisions (`ContrôleDébuté` → `AccèsAutorisé` ou `AccèsRefusé`), ce qui préserve la séquentialité requise par RM14 ; `OpérationsRéintégrées` clôt la période par rejeu contrôlé |

## 5.7. Vérification et confiance (RM28, RM30)

| Règle | Enchaînement formalisé |
|---|---|
| RM28 | `ÉvénementSoumis` → `VérificationDébutée` → `VérificationConclue` → (`ÉvénementValidé` ou `ÉvénementRefusé`) ; l'autorisation de publication est la conjonction de `OrganisationVérifiée` et de `ÉvénementValidé` |
| RM30 | `MesureDeSécuritéDécidée` → `MesureDeSécuritéAppliquée` : la traçabilité est la séquence elle-même, enregistrée conformément à la règle E5 |

---

# 6. Règles traduites en politiques

Ces règles ne sont pas des contraintes de cohérence : elles s'imposent par convention d'exploitation ou de périmètre, et ne trouvent pas d'invariant à porter.

| Règle | Nature de la politique |
|---|---|
| RM09 | Contrainte d'exploitation du MVP : la vente exige la connexion ; rien dans les frontières ne l'exprime, elle se impose à l'orchestration |
| RM15 | Convention de configuration : l'affectation à un point d'entrée identifie le contexte sans restreindre la validation ; la frontière du point d'entrée porte le contexte, non la permission |
| RM18 | Politique de déclenchement : les seules sources d'`ObligationDeRemboursementCréée` sont `ÉvénementAnnulé` et la branche remboursement de `RéconciliationConclue` ; aucun autre chemin n'existe dans le modèle — l'absence de déclencheur est la formalisation |
| RM31 | Politique de conservation portée par la règle E5 et l'Agrégat Historique : tout événement émis laisse une trace |

---

# 7. Principes fondamentaux

Les principes P01 à P06 se lisent comme des propriétés structurelles du modèle, vérifiées par construction :

| Principe | Formalisation structurelle |
|---|---|
| P01 | Chaîne de frontières en correspondance biunivoque : Disponibilité → Réservation → Achat → Billet → propriétaire actif ; chaque maillon référence le précédent par identité, aucun raccourci n'existe |
| P02 | Les cinq cycles sont cinq agrégats distincts sans référence croisée directe (Réservation, Paiement, Billet, Présence, Clôture) ; les transitions ne passent que par des événements |
| P03 | Deux mécanismes complémentaires : l'Agrégat Historique n'offre aucune opération de modification, et `ConfigurationModifiée` ne se propage vers aucune frontière historisée |
| P04 | Porté par la règle E5 en émission et par les objets de valeur de contexte (contrôle, remboursement, mesure) en charge utile |
| P05 | Contrainte hors MVP : aucune frontière de Marketplace n'existe ; l'absence totale du concept dans les agrégats est la garantie de priorité |
| P06 | L'inventaire centralisé est l'Agrégat Disponibilité unique ; aucun agrégat de stock local n'existe — la vente hors ligne est impossible par absence de frontière capable de la réaliser |