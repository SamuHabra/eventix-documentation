# Écosystème Eventix

## 1. Définition

L'écosystème Eventix représente l'ensemble des acteurs, organisations, services et environnements qui interviennent directement ou indirectement dans la création, l'organisation, la promotion, la gestion et la participation à un événement.

Eventix s'inscrit dans cet écosystème comme une plateforme numérique permettant de faciliter la découverte, la gestion et l'expérience événementielle.

---

## 2. Vue globale

`text
                         ÉCOSYSTÈME ÉVÉNEMENTIEL

                              SPONSORS
                                 │
                                 ▼
                         ┌──────────────┐
                         │ ÉVÉNEMENT    │
                         └──────────────┘
                           ▲     ▲     ▲
                           │     │     │
             ┌─────────────┘     │     └─────────────┐
             │                   │                   │
        ORGANISATEUR      ARTISTES /            PARTICIPANTS
                          INTERVENANTS
             │
             ▼
       PRESTATAIRES
             │
             ▼
        LOGISTIQUE

                         ┌─────────────────┐
                         │     EVENTIX      │
                         ├─────────────────┤
                         │ Découverte      │
                         │ Gestion         │
                         │ Billetterie     │
                         │ Accès           │
                         │ Statistiques    │
                         └─────────────────┘
                                  │
                                  ▼
                         SERVICES DE PAIEMENT


          MÉDIAS / RÉSEAUX SOCIAUX / PARTENAIRES
                         │
                         ▼
                    VISIBILITÉ


          AUTORITÉS / RÉGLEMENTATION / SÉCURITÉ
                         │
                         ▼
                  CONTRAINTES / CADRE


                  ## 3. Principaux acteurs

### 3.1. Organisateur

L'organisateur est responsable de la conception et de la gestion de l'événement.

Il peut utiliser **Eventix** pour :

- Créer l'événement.
- Publier l'événement.
- Gérer la billetterie.
- Suivre les participants.
- Contrôler les accès.
- Consulter les statistiques.

### 3.2. Participant

Le participant est la personne qui souhaite découvrir et participer à un événement.

Il peut utiliser **Eventix** pour :

- Découvrir des événements.
- Consulter les informations d'un événement.
- Acheter un billet.
- Accéder à son billet.
- Participer à l'événement.

### 3.3. Sponsor

Le sponsor apporte un soutien financier, matériel ou autre à un événement en échange d'une visibilité ou d'autres contreparties.

Le sponsor peut être indirectement concerné par les données et résultats de l'événement.

### 3.4. Artistes et intervenants

Ils contribuent au contenu ou à l'animation de l'événement.

**Exemples :**

- Artistes.
- Conférenciers.
- Humoristes.
- Musiciens.
- Animateurs.
- Intervenants professionnels.

Ils peuvent ne pas utiliser directement **Eventix** dans le MVP.

### 3.5. Prestataires événementiels

Ils fournissent les services nécessaires au déroulement physique de l'événement.

**Exemples :**

- Sonorisation.
- Éclairage.
- Photographie.
- Vidéographie.
- Restauration.
- Décoration.
- Sécurité.
- Transport.

Ils font partie de l'écosystème mais ne sont pas dans le périmètre initial d'**Eventix**.

### 3.6. Services de paiement

Les services de paiement permettent de réaliser les transactions nécessaires à l'achat des billets.

**Eventix** pourra intégrer différents moyens de paiement sans devenir lui-même un opérateur de paiement.

### 3.7. Médias, réseaux sociaux et partenaires de promotion

Ces acteurs contribuent à la visibilité et à la promotion des événements.

Ils peuvent notamment permettre aux organisateurs de toucher leur audience.

**Eventix** peut également devenir progressivement un canal important de découverte des événements.

### 3.8. Lieux et infrastructures

Les événements se déroulent dans des infrastructures adaptées à leur nature.

**Exemples :**

- Salles.
- Centres de conférence.
- Espaces extérieurs.
- Complexes sportifs.
- Stades.

Le lieu influence notamment la capacité, l'organisation et le contrôle des accès.

### 3.9. Autorités et organismes de réglementation

Selon la nature et le lieu de l'événement, différentes autorités ou administrations peuvent intervenir.

Elles peuvent notamment imposer :

- Des autorisations.
- Des règles de sécurité.
- Des restrictions.
- Des obligations réglementaires.

Ces acteurs ne sont pas nécessairement utilisateurs d'**Eventix**, mais leurs exigences peuvent influencer le fonctionnement du produit.

## 4. Positionnement d'Eventix

**Eventix** se positionne comme une couche numérique reliant plusieurs parties de l'écosystème.

```text
Organisateur
     │
     ▼
  EVENTIX
     │
     ├── Découverte ──────► Participant
     │
     ├── Billetterie ─────► Participant
     │
     ├── Paiement ─────────► Service de paiement
     │
     ├── Accès ────────────► Événement
     │
     └── Données ──────────► Organisateur