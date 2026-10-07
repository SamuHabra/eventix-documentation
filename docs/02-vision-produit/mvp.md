
# MVP — Minimum Viable Product

## 1. Définition

Le MVP d'Eventix constitue la première version fonctionnelle permettant de valider la proposition de valeur principale de la plateforme auprès des organisateurs et des participants.

Il doit permettre de couvrir un cycle complet :

> Découvrir → Créer → Publier → Acheter → Accéder → Analyser

Le MVP ne cherche pas à intégrer toutes les fonctionnalités prévues à long terme.

Son objectif est de fournir une expérience complète, cohérente et réellement utile avec un périmètre maîtrisé.

---

## 2. Objectif du MVP

Le MVP doit permettre de valider les hypothèses principales suivantes :

- Les participants sont prêts à utiliser une plateforme dédiée pour découvrir des événements.
- Les organisateurs sont prêts à utiliser une plateforme centralisée pour gérer leurs événements.
- Les participants peuvent acheter et gérer leurs billets via Eventix.
- Les organisateurs peuvent suivre leurs ventes et leurs participants depuis Eventix.
- Le contrôle numérique des billets apporte une meilleure traçabilité et une meilleure sécurité.
- Les données collectées pendant le cycle de vie de l'événement apportent une réelle valeur à l'organisateur.

---

## 3. Fonctionnalités incluses

### 3.1. Gestion des utilisateurs

Le MVP permet :

- La création d'un compte.
- L'authentification.
- La gestion du profil.
- La distinction entre les différents rôles nécessaires au fonctionnement du MVP.

---

### 3.2. Création et gestion des événements

L'organisateur peut :

- Créer un événement.
- Modifier les informations de l'événement.
- Définir la date et le lieu.
- Définir la capacité.
- Définir les catégories de billets.
- Configurer les informations nécessaires à la vente.
- Publier ou dépublier un événement.

---

### 3.3. Découverte des événements

Le participant peut :

- Consulter les événements disponibles.
- Rechercher des événements.
- Filtrer les événements selon des critères pertinents.
- Consulter la page détaillée d'un événement.

Cette fonctionnalité constitue une première étape vers l'ambition d'Eventix de devenir une référence de découverte des événements.

---

### 3.4. Billetterie numérique

Le participant peut :

- Sélectionner un type de billet.
- Acheter un billet.
- Effectuer un paiement via les moyens de paiement intégrés.
- Recevoir son billet.
- Consulter ses billets.

L'organisateur peut :

- Définir les types de billets.
- Définir les prix.
- Proposer pour un même événement des billets d'accès sur place et des billets d'accès à un direct ou à un contenu VOD.
- Créer des codes promotionnels et des liens de suivi pour ses partenaires ou influenceurs.
- Configurer un plan de salle interactif pour les événements à places numérotées.
- Configurer des passes valables sur plusieurs jours, avec un nombre maximal d'entrées.
- Activer les dons optionnels et définir des montants suggérés.
- Suivre les ventes.
- Consulter les billets vendus et disponibles.

Lorsqu'un billet comprend un accès en ligne, Eventix communique au participant autorisé les informations nécessaires pour accéder au direct ou à la VOD. L'hébergement et la diffusion vidéo ne sont pas présumés fournis par Eventix ; le prestataire et le mécanisme technique restent à définir.

Les codes promotionnels appliquent les réductions configurées par l'organisateur. Les liens de suivi identifient la source d'une vente réalisée par leur intermédiaire ; ils ne constituent pas eux-mêmes un code de réduction. Les ventes finalisées et leur chiffre d'affaires sont consultables par code ou lien.

Pour les événements à places numérotées, le participant peut consulter le plan interactif, choisir une place disponible et la réserver pendant le parcours d'achat. La place doit être attribuée au plus à une commande finalisée.

Un pass consomme une entrée à chaque scan d'entrée accepté. La sortie n'est pas scannée ; chaque nouvelle entrée validée consomme donc une entrée supplémentaire. Les scans refusés ne consomment pas d'entrée.

Le participant peut ajouter un don facultatif au paiement d'un billet payant ou gratuit, en choisissant un montant suggéré par l'organisateur ou en saisissant un montant libre. Un don sur un billet gratuit nécessite un paiement. Sans don, le participant ne règle rien et le parcours gratuit actuel est conservé.

Le don est enregistré séparément du prix du billet. Il ne crée pas de billet supplémentaire et ne consomme pas de disponibilité.

---

### 3.5. Gestion des participants

L'organisateur peut :

- Consulter la liste des participants.
- Consulter les informations associées aux billets.
- Suivre les billets vendus.
- Suivre les participants associés aux événements.

---

### 3.6. Contrôle des accès

Le MVP permet :

- La vérification d'un billet.
- La validation d'un billet.
- Le suivi des billets utilisés.
- La prévention de la réutilisation d'un billet déjà validé.
- Le contrôle des entrées restantes d'un pass multi-jours.
- L'enregistrement distinct des dons facultatifs.

L'objectif est de fournir une meilleure traçabilité et de réduire les risques de fraude.

---

### 3.7. Supervision de la cybersécurité Eventix

Le MVP comprend un service interne de supervision cybersécurité distinct de la sécurité métier et du traitement des signalements liés aux événements. Il permet aux personnes habilitées de consulter un tableau de bord d'alertes, d'examiner les éléments disponibles, de qualifier un incident et de tracer la décision humaine prise.

Une alerte constitue un signal à examiner, pas la preuve qu'une attaque a abouti. Le tableau de bord présente les limites et la couverture des sources de détection connues. Aucune mesure de confinement, suspension, sanction, invalidation de billet ou blocage de fonds n'est déclenchée automatiquement par une alerte dans le MVP ; la décision revient à un responsable humain habilité.

Le périmètre fonctionnel détaillé est défini dans [`13-securite/supervision-cybersecurite.md`](../13-securite/supervision-cybersecurite.md). Son propriétaire logique est le contexte BC-13 / module MOD-13 ; les contrats, sources et mécanismes d'intégration restent à spécifier dans les phases d'architecture.

---

### 3.8. Statistiques

Le MVP fournit à l'organisateur des informations permettant de suivre la performance de son événement, notamment :

- Nombre de billets vendus.
- Billets disponibles.
- Revenus générés.
- Taux de remplissage.
- Nombre de participants.
- Nombre de billets utilisés.
- Nombre de billets non utilisés.
- Répartition des ventes selon les catégories de billets.
- Montants des dons reçus, présentés séparément des ventes de billets.

Les statistiques constituent une première base d'aide à la décision pour les organisateurs.

---

## 4. Plus-value du MVP

Le MVP ne doit pas être une simple plateforme de vente de billets.

Sa valeur repose sur la combinaison de plusieurs éléments :

### Pour les participants

> Découvrir facilement des événements, obtenir leur billet simplement et disposer d'un accès numérique centralisé.

### Pour les organisateurs

> Gérer l'ensemble du parcours principal de leur événement depuis une plateforme unique et disposer d'une vision claire des ventes, des participants et des accès.

### Pour Eventix
Le MVP établit les premières bases d'un écosystème où les événements sont :

> découverts → gérés → vécus → analysés.

---

## 5. Fonctionnalités volontairement exclues du MVP

Les fonctionnalités suivantes ne font pas partie du MVP :

- Quiz en direct.
- Votes en temps réel.
- Sondages et interactions avancées.
- Points de vente physiques.
- Reels et contenus vidéo courts.
- Marketplace de prestataires.
- Gestion des événements dans les stades à très grande capacité.
- Abonnements et paiements récurrents.
- Services logistiques physiques.
- Comptabilité complète.
- Gestion complète de la communication externe.

Ces fonctionnalités pourront être ajoutées dans des versions futures.

---

## 6. Évolution prévue du produit

Les évolutions suivantes sont envisagées à titre indicatif :

### V1 — MVP

- Découverte.
- Création et publication.
- Billetterie numérique.
- Pass multi-jours à quota d'entrées.
- Dons optionnels.
- Paiement intégré.
- Gestion des participants.
- Contrôle des accès.
- Statistiques.

### V2 — Expérience interactive

- Quiz live.
- Votes.
- Sondages.
- Interactions avec les participants.
- Autres fonctionnalités d'animation.

### V3 — Accessibilité physique

- Réseau de points de vente physiques.
- Gestion des ventes réalisées hors ligne.
- Synchronisation avec la plateforme numérique.

### V4 — Contenu événementiel

- Reels.
- Contenus courts.
- Mise en avant des événements.
- Contenus générés autour des événements.

### V5 et suivantes — Écosystème

- Marketplace de prestataires.
- Gestion d'événements de très grande capacité.
- Nouvelles fonctionnalités destinées aux organisateurs et participants.
- Expansion vers de nouveaux marchés africains.

Cette roadmap est indicative et pourra évoluer selon les résultats du MVP, les besoins des utilisateurs et les priorités stratégiques.

---

## 7. Principe d'évolutivité

Le MVP doit être conçu de manière à permettre l'intégration progressive des fonctionnalités futures sans nécessiter une remise en cause majeure du cœur du système.

Les décisions d'architecture, de modélisation des données et d'organisation du code doivent donc tenir compte des évolutions potentielles du produit.

Cependant, les fonctionnalités futures ne doivent pas être développées prématurément uniquement parce qu'elles sont prévues dans la roadmap.

Le principe retenu est :

> Préparer l'architecture à l'évolution sans construire prématurément les fonctionnalités futures.

---

## 8. Critère de réussite du MVP

Le MVP sera considéré comme pertinent lorsqu'il démontrera qu'Eventix peut apporter une valeur réelle aux deux faces de son marché :

Participant :

> Découvrir → acheter → accéder à un événement.

Organisateur :

> Créer → publier → vendre → contrôler → analyser.

La validation du MVP permettra ensuite de déterminer les fonctionnalités à prioriser dans les versions suivantes.