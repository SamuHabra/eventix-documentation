# Acteurs


## 1. Définition

Un acteur représente une personne, une organisation, un système ou une entité susceptible d'interagir avec Eventix ou d'avoir une influence sur son fonctionnement.

Les acteurs sont distingués selon leur nature et leur relation avec le système.

Un acteur peut être :

- Un utilisateur direct d'Eventix.
- Un acteur métier indirect.
- Un acteur futur.
- Un système externe.
- Un acteur de menace.

La présence d'un acteur dans ce document ne signifie pas nécessairement qu'il sera pris en charge dans le MVP.

---

# 2. Classification des acteurs

Les acteurs Eventix sont répartis en plusieurs catégories :

`text
ACTEURS EVENTIX
│
├── Acteurs principaux
│   ├── Organisateur
│   └── Participant
│
├── Acteurs opérationnels
│   ├── Agent de vente
│   ├── Agent de contrôle d'accès
│   └── Équipe opérationnelle
│
├── Acteurs de l'écosystème événementiel
│   ├── Artiste / Intervenant
│   ├── Prestataire événementiel
│   ├── Sponsor
│   ├── Lieu / Gestionnaire de lieu
│   └── Partenaire de promotion
│
├── Services externes
│   ├── Service de paiement
│   ├── Réseaux sociaux / Médias
│   └── Services automatisés
│
├── Acteurs institutionnels
│   └── Autorités / Organismes de réglementation
│
└── Acteurs de menace
    └── Acteur malveillant

## 3. Acteurs principaux

### 3.1. Organisateur

L'organisateur est responsable de la conception, de la préparation et de la gestion d'un événement.

Il constitue l'un des principaux utilisateurs d'Eventix.

Il peut notamment :

- Créer un événement.
- Modifier un événement.
- Définir les informations de l'événement.
- Configurer la billetterie.
- Définir les catégories de billets.
- Définir les prix.
- Publier l'événement.
- Gérer les participants.
- Suivre les ventes.
- Gérer les accès.
- Consulter les statistiques.
- Analyser les performances de l'événement.

L'organisateur peut également attribuer des rôles à d'autres personnes intervenant dans la gestion de l'événement.

### 3.2. Participant

Le participant est une personne qui souhaite découvrir ou participer à un événement.

Il peut notamment :

- Découvrir des événements.
- Rechercher des événements.
- Consulter les informations d'un événement.
- Acheter un billet.
- Consulter son billet.
- Présenter son billet lors de l'accès.
- Participer à l'événement.
- Interagir avec certaines fonctionnalités proposées par l'événement.

## 4. Acteurs opérationnels

### 4.1. Agent de vente

L'agent de vente intervient dans la vente physique des billets.

Cet acteur est prévu pour une évolution future d'Eventix et ne fait pas partie du MVP initial.

Il pourra notamment :

- Se connecter à une interface dédiée.
- Rechercher un événement.
- Consulter les billets disponibles.
- Enregistrer une vente.
- Enregistrer le moyen de paiement utilisé.
- Générer ou attribuer un billet.
- Consulter ses ventes.
- Suivre les opérations réalisées.

Le compte de l'agent pourra être créé ou attribué par l'organisateur ou par une structure habilitée.

**Statut :** Acteur futur — hors MVP

### 4.2. Agent de contrôle d'accès

L'agent de contrôle est chargé de vérifier les billets à l'entrée d'un événement.

Son compte est créé ou attribué par l'organisateur.

Il dispose d'une interface dédiée et de permissions limitées à sa mission.

Il peut notamment :

- Se connecter à son interface.
- Scanner un billet.
- Vérifier sa validité.
- Vérifier qu'il n'a pas déjà été utilisé.
- Valider l'accès.
- Refuser un billet invalide.
- Consulter le résultat du contrôle.

L'agent de contrôle ne dispose pas des mêmes droits que l'organisateur.

**Statut :** Acteur du MVP

### 4.3. Équipe opérationnelle

L'équipe opérationnelle regroupe les personnes qui participent à la préparation et au déroulement pratique d'un événement.

Selon l'événement, elle peut comprendre :

- Responsables d'organisation.
- Coordinateurs.
- Personnel d'accueil.
- Personnel technique.
- Personnel de sécurité.
- Personnel logistique.

Certains membres pourront éventuellement utiliser Eventix dans le futur avec des permissions adaptées à leur rôle.

**Statut :** Acteur métier — intégration progressive

## 5. Acteurs de l'écosystème événementiel

### 5.1. Artiste / Intervenant

L'artiste ou l'intervenant contribue au contenu ou à l'animation de l'événement.

Exemples :

- Musicien.
- Humoriste.
- Conférencier.
- Animateur.
- Formateur.
- Invité.
- Speaker.

Cet acteur peut avoir un rôle important dans la visibilité et l'attractivité d'un événement.

**Statut :** Acteur de l'écosystème — fonctionnalités futures possibles

### 5.2. Prestataire événementiel

Le prestataire fournit des services nécessaires à l'organisation ou au déroulement d'un événement.

Exemples :

- Sonorisation.
- Éclairage.
- Photographie.
- Vidéographie.
- Décoration.
- Restauration.
- Transport.
- Sécurité.
- Location de matériel.
- Services techniques.

Les prestataires ne sont pas intégrés au MVP.

À long terme, Eventix pourra éventuellement proposer une marketplace permettant aux organisateurs de rechercher et sélectionner des prestataires.

**Statut :** Acteur de l'écosystème — marketplace future

### 5.3. Sponsor

Le sponsor apporte un soutien financier, matériel ou stratégique à un événement.

En contrepartie, il peut rechercher :

- De la visibilité.
- Une présence auprès du public.
- Une association à l'événement.
- Des résultats ou statistiques sur l'événement.

Le sponsor n'est pas un utilisateur direct du MVP.

Des fonctionnalités spécifiques pourront être envisagées à long terme.

**Statut :** Acteur futur / indirect

### 5.4. Lieu / Gestionnaire de lieu

Le lieu correspond à l'espace dans lequel se déroule l'événement.

Exemples :

- Salle.
- Centre de conférence.
- Espace extérieur.
- Complexe sportif.
- Stade.
- Centre culturel.

Le gestionnaire du lieu peut être amené à fournir ou gérer :

- La disponibilité du lieu.
- La capacité.
- Les informations pratiques.
- Les conditions d'utilisation.
- Les contraintes liées au lieu.

**Statut :** Acteur de l'écosystème — intégration future possible

### 5.5. Partenaire de promotion

Les partenaires de promotion contribuent à augmenter la visibilité d'un événement.

Ils peuvent être :

- Médias.
- Influenceurs.
- Communautés.
- Associations.
- Entreprises partenaires.
- Pages ou plateformes spécialisées.

Ils peuvent contribuer à orienter des participants vers un événement.

**Statut :** Acteur indirect

## 6. Services externes

### 6.1. Service de paiement

Les services de paiement permettent de réaliser les transactions liées à l'achat des billets.

Eventix pourra intégrer différents moyens de paiement.

Exemples :

- Mobile Money.
- Cartes bancaires.
- Autres moyens de paiement disponibles selon les marchés.

Eventix ne devient pas lui-même un opérateur de paiement.

**Statut :** Service externe — nécessaire au fonctionnement de la billetterie

### 6.2. Réseaux sociaux et médias

Les réseaux sociaux et médias constituent des canaux externes de visibilité et de promotion.

Ils peuvent notamment permettre :

- La diffusion d'informations sur un événement.
- Le partage d'un événement.
- L'acquisition de participants.

Eventix pourra éventuellement proposer des mécanismes de partage ou d'intégration avec ces plateformes.

**Statut :** Acteurs / services externes

### 6.3. Services automatisés

Les services automatisés représentent les composants logiciels capables d'exécuter certaines opérations sans intervention humaine directe.

Ils peuvent notamment intervenir dans :

- Les notifications.
- Les rappels.
- Le traitement de certaines opérations.
- La synchronisation de données.
- Le traitement d'événements système.
- La détection d'activités inhabituelles.
- Certaines tâches automatisées.

Les responsabilités précises seront définies lors de l'analyse des besoins et de la conception technique.

**Statut :** Acteurs techniques

## 7. Acteurs institutionnels

### 7.1. Autorités et organismes de réglementation

Selon la nature de l'événement, certaines autorités ou administrations peuvent intervenir.

Elles peuvent imposer :

- Des autorisations.
- Des règles de sécurité.
- Des restrictions.
- Des obligations réglementaires.
- Des conditions particulières d'organisation.

Ces acteurs peuvent ne pas utiliser Eventix directement, mais leurs exigences peuvent influencer le produit et les processus métier.

**Statut :** Acteurs indirects

## 8. Acteurs de menace

### 8.1. Acteur malveillant

L'acteur malveillant représente toute personne ou entité cherchant à exploiter Eventix de manière frauduleuse ou nuisible.

Il peut notamment tenter de :

- Falsifier un billet.
- Réutiliser un billet.
- Contourner le contrôle d'accès.
- Compromettre un compte.
- Accéder à des données non autorisées.
- Manipuler des informations.
- Exploiter une vulnérabilité.
- Effectuer des transactions frauduleuses.

L'acteur malveillant n'est pas un utilisateur fonctionnel.

Il est néanmoins identifié dans le modèle métier afin que ses menaces soient prises en compte dans :

- Les exigences de sécurité.
- Les règles métier.
- Les cas d'utilisation.
- L'architecture.
- Les mécanismes de contrôle.

## 9. Synthèse des acteurs

| Acteur | Nature | MVP | Évolution prévue |
|---|---|---|---|
| Organisateur | Principal | Oui | Oui |
| Participant | Principal | Oui | Oui |
| Agent de contrôle | Opérationnel | Oui | Oui |
| Agent de vente | Opérationnel | Non | Oui |
| Équipe opérationnelle | Opérationnel | Partiel | Oui |
| Artiste / Intervenant | Écosystème | Non | Possible |
| Prestataire | Écosystème | Non | Oui |
| Sponsor | Écosystème | Non | Possible |
| Lieu / Gestionnaire de lieu | Écosystème | Non | Possible |
| Partenaire de promotion | Écosystème | Non | Possible |
| Service de paiement | Externe | Oui | Oui |
| Réseaux sociaux / Médias | Externe | Indirect | Oui |
| Services automatisés | Technique | Oui | Oui |
| Autorités / Régulateurs | Institutionnel | Indirect | Oui |
| Acteur malveillant | Menace | À prendre en compte | Oui |

## 10. Principe d'évolution

La présence d'un acteur dans ce document ne signifie pas qu'une fonctionnalité correspondante doit être développée immédiatement.

Eventix doit distinguer :

> **Comprendre aujourd'hui → Préparer l'évolution → Développer au bon moment.**

Les acteurs futurs sont donc documentés afin de préserver une compréhension complète du métier et de faciliter les futures évolutions du produit, sans élargir artificiellement le périmètre du MVP.

---