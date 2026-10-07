# Service organisationnel — Opérations événementielles

| Propriété | Définition |
|---|---|
| **Bénéficiaire principal** | Organisateur |
| **Bénéficiaires associés** | Participant, agent de vente, équipe d'entrée |
| **Objet** | Outiller la préparation, l'exploitation métier et le suivi numérique de la billetterie d'un événement |
| **Statut** | Service numérique dans le périmètre Eventix ; prestations physiques exclues |

## 1. Besoin métier

L'organisateur a besoin d'un parcours cohérent pour préparer son événement, proposer des billets, gérer les ventes et les accès, puis consulter les résultats. Eventix centralise les opérations couvertes par la plateforme, sans se substituer à l'organisateur ou aux prestataires événementiels.

## 2. Cycle de service

### Avant l'événement

- Créer, configurer, publier et mettre en vente l'événement.
- Définir les capacités, espaces, zones, catégories et périodes de vente.
- Préparer l'offre de billets : gratuité ou paiement, modes d'accès et, lorsqu'ils sont prévus, passes multi-jours et dons facultatifs.
- Configurer un plan interactif pour les places numérotées.
- Organiser les canaux de vente et les équipes autorisées.

### Pendant l'événement

- Suivre les ventes, billets, participants et disponibilités.
- Contrôler les billets aux points d'entrée affectés.
- Consulter les informations nécessaires à la gestion numérique des accès.
- Pour un billet hybride, communiquer à son détenteur éligible les informations d'accès en ligne selon les modalités retenues.

Chaque scan d'entrée accepté d'un pass consomme une entrée ; les sorties ne sont pas scannées. Cette règle est définie par **RM34** et n'est pas une fonction de sortie/retour distincte.

### Après l'événement

- Suivre les statistiques et les présences enregistrées.
- Préparer la clôture et le règlement suivant le cycle financier de référence.
- Conserver l'historique nécessaire à la traçabilité et aux analyses permises.

## 3. Valeur pour les acteurs

- **Organisateur** : réduit la fragmentation entre configuration, vente, gestion des billets, accès et suivi.
- **Équipe de vente et d'entrée** : agit sur les événements et points qui lui sont attribués, dans les limites de ses autorisations.
- **Participant** : retrouve un lien cohérent entre l'offre, la commande, le billet et les droits d'accès.
- **Eventix** : maintient une vue de billetterie exploitable et traçable pour les parcours pris en charge.

## 4. Capacités documentées et dépendances

Les besoins métier couvrent notamment la gestion des événements et espaces (**B01–B04**, **B08–B09**), les ventes et canaux (**B10–B13**), le contrôle et le suivi des participants (**B21–B25**), la billetterie hybride (**B35**) et la sélection d'une place (**B37**).

Les capacités MVP documentées incluent également les passes multi-jours et dons facultatifs. Le suivi marketing relève de [Marketing](./marketing.md), et la clôture et le retrait de [Finance](./finance.md) : cette page décrit leur place dans le cycle événementiel, sans redéfinir leurs responsabilités.

## 5. Frontières du service

Eventix fournit un outillage numérique. Cette définition ne promet pas :

- la location du lieu, la production du spectacle, le personnel d'accueil ou de sécurité physique ;
- une assistance opérationnelle humaine sur place ;
- l'hébergement ou la diffusion vidéo du direct/VOD ;
- des fonctionnalités non documentées comme les applications NFC, la reconnaissance faciale ou les bornes autonomes ;
- la surveillance technique de la plateforme Eventix, qui relève de son exploitation système et de sa cybersécurité.

Les paramètres détaillés du plan de salle (**QMO-052**), de l'accès vidéo (**QMO-049**), des passes après report (**QMO-048**) et des dons (**QMO-045 à QMO-047**) restent à arbitrer.

## 6. Traçabilité

- Besoins : [B01–B04](../besoins-metier.md#2-besoins-liés-à-lorganisateur), [B08–B09](../besoins-metier.md#4-besoins-liés-à-la-configuration-des-espaces), [B10–B13](../besoins-metier.md#5-besoins-liés-à-la-vente), [B21–B25](../besoins-metier.md) et [B35 — Billetterie hybride](../besoins-metier.md#11-besoins-complémentaires-du-mvp) / [B37 — Place assise](../besoins-metier.md#11-besoins-complémentaires-du-mvp). B36 est détaillé dans le service Marketing.
- Processus de référence : [processus métier](../processus-metier.md).
- Règles : [RM34–RM39](../regles-metier.md).
- Exigences : configuration et accès **EF-008 à EF-024**, **EF-057 à EF-071**, **EF-130 à EF-139** et **EF-142 à EF-143** dans [les exigences fonctionnelles](../../04-analyse-des-besoins/exigences-fonctionnelles.md). Les fonctions promotionnelles EF-140/141 sont détaillées dans le service Marketing ; les règles financières correspondantes sont précisées dans Finance.
