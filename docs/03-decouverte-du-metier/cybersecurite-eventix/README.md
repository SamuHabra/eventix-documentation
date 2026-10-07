# Découverte des enjeux de cybersécurité — Eventix

## 1. Positionnement

Ce dossier traite de la cybersécurité du **système Eventix**. Il complète la découverte métier en identifiant les actifs, impacts, risques et responsabilités qui devront orienter les exigences et la conception.

**Orientation produit retenue :** un service de supervision cybersécurité et son tableau de bord font partie du MVP. Le service détecte et analyse des signaux, émet des alertes et suit les incidents ; la décision de mesure reste humaine et habilitée. Le détail fonctionnel est spécifié dans la [phase 13 — Sécurité](../../13-securite/README.md).

Il ne décrit ni le service de sécurité et de confiance métier rendu autour des événements, ni une architecture technique, une base de données ou une méthode d'exploitation de vulnérabilité. La sécurité métier est présentée dans [`../services-organisationnels/securite-et-confiance.md`](../services-organisationnels/securite-et-confiance.md).

## 2. Périmètre de la découverte

La phase 03 peut établir et faire valider :

- les actifs métier et informations dont la compromission aurait un impact sur les participants, les organisateurs ou Eventix ;
- les objectifs de sécurité à préserver : confidentialité, intégrité, disponibilité, authenticité et traçabilité, selon leur pertinence métier ;
- les conséquences redoutées d'une divulgation, altération, indisponibilité, fraude ou prise de contrôle ;
- les acteurs, responsabilités, contraintes réglementaires et dépendances externes pertinentes ;
- les hypothèses, risques et questions à faire arbitrer avant de spécifier les exigences.

Les menaces et risques doivent rester qualifiés comme hypothèses tant qu'ils ne sont pas étayés. Cette phase ne fixe pas les technologies, contrôles, mesures de sécurité ou choix de stockage.

### Actifs à examiner

Les actifs ci-dessous sont des pistes dérivées des capacités déjà décrites, et non une classification validée ni un inventaire exhaustif :

- comptes, identités et droits des participants, organisateurs et équipes ;
- données personnelles et informations de contact ;
- configuration des événements, capacité et attribution des places ;
- commandes, paiements, remboursements, commissions, soldes et retraits ;
- billets, droits d'accès, QR codes et faits de contrôle ;
- liens et codes de promotion, ainsi que les données d'attribution associées ;
- informations d'accès au direct et à la VOD fournies par un prestataire éventuel ;
- disponibilité et intégrité des services Eventix nécessaires aux ventes et contrôles.

Pour chaque actif, les parties prenantes devront préciser la valeur métier, les conséquences d'une atteinte, les obligations de protection et le propriétaire responsable. Ne pas inclure de données de carte ou d'identifiants de paiement sensibles dans l'inventaire Eventix sans décision et analyse spécifiques.

### Risques à valider

Les scénarios suivants servent à guider l'analyse et ne signifient pas qu'une vulnérabilité existe :

- prise de contrôle d'un compte ou usage abusif de privilèges ;
- divulgation ou modification non autorisée de données ou de configuration ;
- falsification, copie ou rejeu d'un billet ou d'un droit d'accès ;
- perturbation de la vente ou du contrôle lors d'une période critique ;
- compromission d'un fournisseur ou d'une intégration ;
- fuite d'un accès vidéo ou mauvaise attribution d'un droit en ligne.

La fraude à l'événement, les faux organisateurs et les décisions de modération restent suivis par la sécurité et confiance métier ; un scénario peut relever des deux domaines s'il implique aussi une atteinte au système.

## 3. Approches défensive et offensive

### Défensive

La démarche défensive vise à prévenir, détecter, contenir et traiter les atteintes à la sécurité du système et des données. En phase de découverte, on recense les objectifs, les impacts, les responsabilités et les capacités organisationnelles attendues ; les contrôles techniques seront définis dans les phases de conception.

### Offensive autorisée

Les évaluations offensives servent à éprouver la résistance du système et à vérifier l'efficacité des mesures défensives. Elles ne sont envisagées que sur des actifs explicitement autorisés, avec un périmètre approuvé, des règles d'engagement, des conditions d'arrêt et un processus de restitution/remédiation définis.

Ce document ne donne pas de procédure d'intrusion et n'autorise aucune activité contre des systèmes tiers. Les tests techniques et leur planification relèvent des phases de conception, de réalisation et de validation adaptées.

### Gouvernance et capacités organisationnelles à établir

La gouvernance devra déterminer qui est responsable de la sécurité, qui peut autoriser un test, qui reçoit les alertes, qui décide du confinement et qui coordonne les corrections et communications. La capacité à conserver des preuves et à tirer des enseignements d'un incident doit être prise en compte. Ces responsabilités sont à arbitrer, et ne sont pas attribuées par la présente fiche.

Questions ouvertes associées : **QMO-053 à QMO-058** dans [`../questions-metier-ouvertes.md`](../questions-metier-ouvertes.md). Les priorités de détection et profils d'accès du tableau de bord restent à préciser via **QMO-057–QMO-058**.

## 4. Passage aux phases suivantes

| Phase | Résultat attendu à partir des enjeux découverts |
|---|---|
| **04 — Analyse des besoins** | Exigences non fonctionnelles de sécurité formulées de manière vérifiable, avec priorité et critères d'acceptation dans les [exigences non fonctionnelles](../../04-analyse-des-besoins/exigences-non-fonctionnelles.md). |
| **05 — DDD** | Concepts, responsabilités et invariants métier liés aux identités, droits ou données, sans imposer de mécanisme technique ([phase 05](../../05-domain-driven-design/README.md)). |
| **06 — UML** | Acteurs, frontières et scénarios métier utiles à la compréhension des interactions et abus à considérer ([phase 06](../../06-modelisation-uml/README.md)). |
| **07 — Architecture logique** | Responsabilités, frontières de confiance et flux à protéger, au niveau logique ([phase 07](../../07-architecture-logique/README.md)). |
| **08 — Architecture des données** | Classification, accès, intégrité, conservation, sauvegarde et cohérence des données ([phase 08](../../08-architecture-des-donnees/README.md)). |
| **09–10 — Architecture technique et infrastructure** | Mécanismes techniques, déploiement, identités système, secrets et protections d'infrastructure ([phase 09](../../09-architecture-technique/README.md), [phase 10](../../10-infrastructure/README.md)). |
| **Validation et exploitation** | Vérifications défensives, évaluations offensives autorisées, gestion des constats et suivi des corrections. |

Le point de passage vers l'analyse comprend **B38**, **RM40**, les exigences **EF-145–EF-147** et les exigences non fonctionnelles transversales **ENF-016 à ENF-021**. Ces artefacts comprennent des enjeux de cybersécurité et des enjeux de sécurité métier ; il faut conserver cette distinction lors de leur évolution.

## 5. Traçabilité et gouvernance

Les questions non tranchées sont consignées dans [`../questions-metier-ouvertes.md`](../questions-metier-ouvertes.md). Une décision de sécurité doit indiquer son propriétaire, sa justification, son statut et les phases/documentations qu'elle impacte.

Avant toute évaluation offensive, confirmer formellement l'autorisation, les actifs et environnements concernés, les limites d'impact, les contacts d'escalade et le traitement des résultats. Aucun test ne doit être déduit de la seule présence de ce dossier.
