# Services de domaine — Eventix

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
3. [Définition d'un service de domaine](#3-définition-dun-service-de-domaine)
4. [Règles d'identification](#4-règles-didentification)
5. [Catalogue des services](#5-catalogue-des-services)
6. [Coordination et cohérence](#6-coordination-et-cohérence)
7. [Résumé](#7-résumé)
8. [Critères de qualité du document](#8-critères-de-qualité-du-document)
9. [Statut](#9-statut)

---

# 1. Objectif

Ce document identifie les opérations métier d'Eventix qui ne peuvent être confiées à un agrégat unique. Ces opérations sont modélisées comme des services de domaine : des capacités métier sans état propre, qui coordonnent plusieurs agrégats par leurs racines. Pour chaque service, ce document précise :

- l'opération métier qu'il porte ;
- les agrégats qu'il coordonne ;
- les invariants ou règles métier qu'il garantit ;
- le processus métier dont il résulte.

Il ne redéfinit ni les agrégats, ni les processus métier. Ces éléments sont utilisés par référence et supposés connus du lecteur.

---

# 2. Sources de référence

Ce document est dérivé principalement de :

- `05-domain-driven-design/agregats.md` — frontières de cohérence que les services ne franchissent que par les racines
- `03-decouverte-du-metier/processus-metier.md` — opérations transversales (PM01 → PM72) dont les enchaînements imposent des coordinations

Toute définition d'agrégat ou description de processus mentionnée implicitement renvoie à ces documents sources.

---

# 3. Définition d'un service de domaine

## 3.1. Caractéristiques

Un service de domaine est une capacité métier qui possède :

- **Aucun état propre** : il ne stocke rien ; tout état appartient aux agrégats qu'il coordonne.
- **Une sémantique métier** : son nom et son contrat sont exprimés en termes du langage ubiquitaire.
- **Une coordination par les racines** : il n'accède aux agrégats que par leurs racines, conformément aux références inter-agrégats établies dans `agregats.md`.
- **Une garantie d'invariant** : il existe parce qu'un invariant ou une règle métier ne peut être garanti à l'intérieur d'une seule frontière.

## 3.2. Rôle dans le modèle

Les agrégats garantissent la cohérence à l'intérieur de leurs frontières ; les services de domaine garantissent la cohérence des enchaînements entre frontières. Lorsqu'un processus métier relie plusieurs agrégats par leurs identités, la décision de franchir d'un agrégat à l'autre n'appartient à aucune racine : elle appartient à un service.

---

# 4. Règles d'identification

Les règles suivantes déterminent quand une opération devient un service de domaine :

| Règle | Énoncé |
|---|---|
| S1 — Transversalité | L'opération coordonne au moins deux agrégats distincts par leurs identités. |
| S2 — Aucune appartenance | L'opération ne relève naturellement d'aucune des racines coordonnées ; l'attribuer à l'une des deux lui donnerait une responsabilité étrangère. |
| S3 — Nommage | Le service porte un nom composé de termes du langage ubiquitaire ; aucun service ne porte un nom technique. |
| S4 — Sans état | Le service ne conserve aucune donnée entre deux invocations ; toute trace persistante est déléguée à un agrégat. |
| S5 — Respect des frontières | Le service ne modifie jamais un agrégat autrement que par sa racine et ne contourne aucun invariant de frontière. |

---

# 5. Catalogue des services

## 5.1. BC-02 / BC-10 — Vérification et publication

### ServiceDeVerificationEvenementielle

| Élément | Valeur |
|---|---|
| **Opération** | Déterminer si un organisateur et son événement sont suffisamment crédibles pour être publiés (PM05, PM06) |
| **Agrégats coordonnés** | Agrégat Événement, Agrégat Organisation, Agrégat Mesure de sécurité |
| **Règles garanties** | Un événement refusé ne peut pas être publié ; la décision tient compte de la vérification d'organisation, qui appartient à une frontière distincte de celle de l'événement |

**Justification** : la décision de publication combine l'état de deux agrégats — l'Événement soumis et l'Organisation vérifiée. Aucune des deux racines ne peut décider seule, conformément aux règles S1 et S2.

---

## 5.2. BC-04 — Expiration des réservations

### ServiceDExpirationDeReservation

| Élément | Valeur |
|---|---|
| **Opération** | Faire expirer les réservations dont le délai est dépassé et libérer les disponibilités correspondantes (PM42) |
| **Agrégats coordonnés** | Agrégat Réservation, Agrégat Disponibilité |
| **Règles garanties** | L'expiration libère la disponibilité sans constituer un échec de paiement ; la libération ne concerne que les réservations réellement expirées |

**Justification** : l'invariant d'expiration est porté par la Réservation, mais son effet — la libération — appartient à la Disponibilité. La séparation des deux frontières, choisie dans `agregats.md`, rend l'opération transversale par construction.

---

## 5.3. BC-05 / BC-04 / BC-06 — Confirmation de paiement et émission

### ServiceDeFinalisationDAchat

| Élément | Valeur |
|---|---|
| **Opération** | Traiter une confirmation de paiement, finaliser l'achat, décrémenter la disponibilité et déclencher l'émission du billet (PM45, PM47, PM62) |
| **Agrégats coordonnés** | Agrégat Paiement, Agrégat Réservation, Agrégat Disponibilité, Agrégat Achat, Agrégat Billet |
| **Règles garanties** | Une même confirmation reçue plusieurs fois ne produit qu'un seul effet métier (PM44) ; le système ne débite pas à nouveau, ne crée pas un deuxième achat, ne génère pas plusieurs billets pour la même transaction (PM47) |

**Justification** : l'enchaînement paiement → achat → billet traverse trois frontières tout en devant se comporter comme une opération unique du point de vue métier. L'idempotence exigée par le processus ne peut être garantie par aucun agrégat seul : elle caractérise l'enchaînement lui-même. Le service orchestre les transitions dans l'ordre où chaque frontière reste consistente.

---

## 5.4. BC-05 / BC-09 — Réconciliation des paiements tardifs

### ServiceDeReconciliation

| Élément | Valeur |
|---|---|
| **Opération** | Déterminer l'issue d'un paiement confirmé après expiration de la réservation (PM43, PM63) |
| **Agrégats coordonnés** | Agrégat Paiement, Agrégat Réservation, Agrégat Disponibilité, Agrégat Billet, Agrégat Obligation de remboursement |
| **Règles garanties** | Un paiement tardif n'est jamais ignoré ; il conduit soit à l'attribution d'un billet si la disponibilité existe, soit au remboursement |

**Justification** : l'issue — billet ou remboursement — dépend de l'état d'une disponibilité appartenant à une frontière tierce. La décision ne relève ni du Paiement ni du futur Remboursement ; le service la porte et déclenche ensuite l'opération dans l'agrégat choisi, sans jamais le modifier directement.

---

## 5.5. BC-02 / BC-06 / BC-09 / BC-12 — Annulation d'événement

### ServiceDAnnulationDevenement

| Élément | Valeur |
|---|---|
| **Opération** | Annuler un événement publié : bloquer les ventes, invalider les billets concernés, informer les participants et déclencher les remboursements applicables (PM29, PM64) |
| **Agrégats coordonnés** | Agrégat Événement, Agrégats Billet (par identité, en nombre quelconque), Agrégat Notification, Agrégat Obligation de remboursement |
| **Règles garanties** | Un événement annulé ne peut plus recevoir de nouvelles ventes ; les billets concernés deviennent invalides ; les participants sont informés ; les remboursements applicables sont déclenchés |

**Justification** : l'annulation touche un nombre indéterminé d'agrégats Billet référencés par identité, plus la notification et les remboursements. Aucune racine — y compris celle de l'Événement — ne peut porter une opération dont la portée dépasse sa frontière. Le service ordonne les étapes : transition de l'Événement d'abord, invalidation des billets ensuite, obligations de remboursement et notifications enfin.

---

## 5.6. BC-02 / BC-12 — Report d'événement

### ServiceDeReportDevenement

| Élément | Valeur |
|---|---|
| **Opération** | Reporter un événement à une nouvelle date et informer les participants (PM30, PM65) |
| **Agrégats coordonnés** | Agrégat Événement, Agrégat Notification |
| **Règles garanties** | Un report conserve les billets existants et ne déclenche pas de remboursement automatique |

**Justification** : la modification de la date est une opération interne de l'Agrégat Événement — le service ne prend le relais que pour la coordination avec l'information des participants, qui appartient à une frontière distincte et suit la décision de report.

---

## 5.7. BC-07 / BC-06 — Contrôle d'accès

### ServiceDeControleDAcces

| Élément | Valeur |
|---|---|
| **Opération** | Décider de l'accès à partir d'un scan et enregistrer le résultat (PM14 → PM18, PM22 → PM26, PM66) |
| **Agrégats coordonnés** | Agrégat Billet, Agrégat Point d'entrée |
| **Règles garanties** | La décision de validation remonte à l'Agrégat Billet, seul habilité à faire évoluer son état ; la Présence n'est enregistrée qu'au point d'entrée concerné ; un billet `USED`, annulé ou d'un autre événement est refusé |

**Justification** : le contrôle interroge le billet pour décider, puis enregistre le résultat dans une autre frontière. Le service articule les deux sans jamais modifier le billet en dehors de sa racine.

### ServiceDeGestionDuModeDegrade

| Élément | Valeur |
|---|---|
| **Opération** | Assurer la cohérence du contrôle multi-scanners lorsque l'état partagé fiable n'est plus disponible (PM48, PM67) |
| **Agrégats coordonnés** | Agrégats Point d'entrée (un par scanner actif) |
| **Règles garanties** | Plusieurs scanners nécessitent un état partagé fiable ; à défaut, un seul scanner actif ; Eventix n'autorise jamais plusieurs contrôles concurrents sans état partagé garanti (PM71, Règle 7) |

**Justification** : l'arbitrage entre scanners porte sur la coordination de plusieurs instances d'agrégats distincts ; la règle de fiabilité prime sur le débit. La décision d'activer ou de restreindre un scanner n'appartient à aucune racine de Point d'entrée.

---

## 5.8. BC-08 — Clôture financière

### ServiceDeClotureFinanciere

| Élément | Valeur |
|---|---|
| **Opération** | Déterminer le montant net dû à l'organisateur et alimenter son solde (PM28, PM57, PM58, PM70) |
| **Agrégats coordonnés** | Agrégat Clôture, Agrégat Solde organisateur ; lectures par identité des ventes confirmées, remboursements et frais |
| **Règles garanties** | Le montant net est déterminé lorsque les opérations nécessaires ont été traitées ; l'organisateur ne peut retirer des fonds qu'après cette clôture |

**Justification** : la clôture agrège par identité des données réparties dans plusieurs frontières — achats, remboursements, retraits — avant d'alimenter le solde. Le calcul du montant net ne peut être confié ni à la Clôture seule (qui ne possède pas les sources), ni au Solde (qui n'a pas vocation à lire l'activité commerciale).

---

## 5.9. BC-10 / BC-02 / BC-01 — Application des mesures de sécurité

### ServiceDApplicationDeMesureDeSecurite

| Élément | Valeur |
|---|---|
| **Opération** | Appliquer une mesure de sécurité à sa cible et évaluer les conséquences (PM54, PM55, PM68, PM69) |
| **Agrégats coordonnés** | Agrégat Mesure de sécurité, Agrégat Organisation, Agrégat Événement, Agrégat Utilisateur, Agrégat Notification |
| **Règles garanties** | Un signalement n'est pas automatiquement une preuve de fraude ; une organisation bannie ne peut poursuivre librement son activité ; les événements existants sont évalués individuellement avant toute annulation |

**Justification** : la mesure référence sa cible par identité — Événement, Compte ou Organisation — et déclenche dans chacune une transition d'état distincte, suivie d'une information des parties concernées. La proportionnalité de la décision et son application sur plusieurs frontières relèvent d'une coordination externe à toute cible.

---

# 6. Coordination et cohérence

## 6.1. Ordre des opérations

Lorsqu'un service coordonne plusieurs agrégats, chaque transition est accomplie par la racine concernée, dans un ordre qui garantit qu'aucune frontière n'est laissée inconsistente :

| Service | Ordre de coordination |
|---|---|
| ServiceDeFinalisationDAchat | Paiement → Réservation → Disponibilité → Achat → Billet |
| ServiceDAnnulationDevenement | Événement → Billets (par identité) → Obligations de remboursement → Notifications |
| ServiceDeReconciliation | Paiement → Disponibilité → Billet ou Obligation de remboursement |
| ServiceDeClotureFinanciere | Clôture → Solde organisateur |

## 6.2. Idempotence

Deux services portent une exigence d'idempotence issue des processus métier : le ServiceDeFinalisationDAchat et le ServiceDeReconciliation. Leur contrat garantit qu'une invocation répétée avec les mêmes entrées ne produit qu'un seul effet métier — la vérification de cette propriété relève du service, car aucun agrégat seul ne voit l'enchaînement complet.

## 6.3. Trace des coordinations

Toute coordination laisse une trace dans un agrégat approprié — Historique de configuration, Événement métier, Historique — conformément à l'exigence de traçabilité des processus. Le service lui-même ne conserve rien ; la preuve de la coordination appartient au modèle.

---

# 7. Résumé

Ce document identifie dix services de domaine couvrant les opérations métier d'Eventix qui dépassent une frontière d'agrégat : vérification et publication, expiration des réservations, finalisation d'achat, réconciliation, annulation et report d'événement, contrôle d'accès et mode dégradé, clôture financière, application des mesures de sécurité. Chaque service est sans état, nommé d'après le langage ubiquitaire, et garantit les règles métier que les processus exigent mais qu'aucune racine ne peut porter seule. Ce document constitue la base pour la définition des événements métier et des politiques de cohérence entre agrégats.

---

# 8. Critères de qualité du document

Ce document doit respecter les propriétés suivantes :

- chaque service coordonne au moins deux agrégats distincts ;
- chaque service est sans état et nommé en termes du langage ubiquitaire ;
- chaque service est justifié par une règle métier issue des processus ;
- aucun service ne contourne une frontière d'agrégat ;
- aucune définition d'agrégat ni description de processus n'est reprise des documents sources ;
- aucune décision technique n'est prise ou implicite.

---

# 9. Statut

| Champ | Valeur |
|---|---|
| **Document** | `service-de-domaine.md` |
| **Version** | 1.0 |
| **Statut** | À valider par l'équipe |
| **Périmètre** | MVP Eventix |
| **Marché** | Cameroun |

| Principe | État |
|---|---|
| Transversalité minimale | ✅ APPLIQUÉE |
| Absence d'état propre | ✅ APPLIQUÉE |
| Nommage métier | ✅ APPLIQUÉ |
| Respect des frontières | ✅ APPLIQUÉ |
| Cohérence avec les sources | ✅ RESPECTÉE |
| Décisions techniques | ⏳ NON PRÉJUGÉES |

---

## Notes de rédaction

### Cohérence avec les sources

Ce document ne répète ni les frontières d'agrégat, ni les descriptions de processus. Il identifie les points où les processus franchissent plusieurs frontières et confie ces franchissements à des services, en référençant les sections des sources.

### Limite du catalogue

Un service n'est retenu que si les règles S1 à S5 sont toutes satisfaites. Les opérations internes à un agrégat — modification d'une catégorie de billet, demande de retrait — restent des responsabilités de leurs racines et ne sont pas ici.

### Questions ouvertes

Les questions métier encore ouvertes identifiées dans les phases précédentes ne sont pas tranchées ici et restent référencées dans `questions-metier-ouvertes.md`.