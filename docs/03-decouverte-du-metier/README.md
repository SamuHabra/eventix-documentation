# Découverte du métier

## 1. Objectif du dossier

Ce dossier permet à l'équipe de comprendre en profondeur le fonctionnement du domaine de l'événementiel avant de définir précisément les fonctionnalités et les exigences techniques d'Eventix.

L'objectif est de comprendre :

- Comment fonctionne l'organisation d'un événement.
- Qui sont les différents acteurs impliqués.
- Quels sont leurs rôles et leurs responsabilités.
- Quels sont leurs besoins et leurs difficultés.
- Comment se déroulent les principales activités liées à un événement.
- Quelles règles métier doivent être respectées.
- Quelles contraintes existent dans le domaine.
- Comment les différents acteurs interagissent avec Eventix.

---

## 2. Pourquoi cette étape est importante

Cette phase permet d'éviter de concevoir Eventix uniquement à partir d'idées ou de suppositions techniques.

Avant de décider ce que le système doit faire, l'équipe doit comprendre comment le métier fonctionne réellement.

La découverte du métier sert donc de base aux étapes suivantes :

> Comprendre le métier → Identifier les besoins → Définir les exigences → Concevoir la solution

---

## 3. Contenu du dossier

### ecosysteme-eventix.md

Présente l'écosystème global dans lequel Eventix évolue.

Il permet d'identifier les différentes catégories d'acteurs, services et relations autour de l'événementiel.

### acteurs.md

Identifie les différents acteurs susceptibles d'interagir directement ou indirectement avec Eventix et précise leur rôle dans le système.

### personas.md

Présente des profils représentatifs des principaux utilisateurs d'Eventix afin de mieux comprendre leurs objectifs, comportements, besoins et difficultés.

### besoins-metier.md

Documente les besoins réels des différents acteurs du métier, indépendamment des solutions techniques.

### processus-metier.md

Décrit les processus liés à l'organisation, la gestion et la participation à un événement.

Les processus sont étudiés notamment avant, pendant et après l'événement.

### regles-metier.md

Documente les règles qui doivent être respectées dans le fonctionnement des activités liées aux événements et à Eventix.

### user-journeys.md

Décrit les parcours vécus par les différents utilisateurs lorsqu'ils accomplissent leurs objectifs.

### points-de-contact.md

Identifie les différents moments et canaux par lesquels les utilisateurs interagissent avec Eventix ou avec les acteurs de l'écosystème.

### glossaire-metier.md

Définit les termes utilisés dans le domaine et dans le projet afin que toute l'équipe utilise un vocabulaire commun.

### questions-metier-ouvertes.md

Centralise les questions dont les réponses ne sont pas encore connues ou validées.

Ces questions doivent être étudiées avant de prendre certaines décisions importantes.

### contraintes-metier.md

Documente les contraintes qui peuvent influencer le fonctionnement du produit ou les décisions futures.

### `services-organisationnels/`

Présente les familles de services qu'Eventix rend aux organisateurs par l'intermédiaire de la plateforme. Ces documents exposent la valeur métier, les bénéficiaires et les limites de l'offre ; ils complètent les besoins métier sans recopier les exigences ni promettre une prestation humaine ou opérationnelle qui n'a pas été décidée.

Consulter le [guide des services organisationnels](./services-organisationnels/README.md), puis les pages consacrées au [marketing](./services-organisationnels/marketing.md), à la [finance](./services-organisationnels/finance.md), aux [opérations événementielles](./services-organisationnels/operations-evenementielles.md) et à la [sécurité et confiance métier](./services-organisationnels/securite-et-confiance.md).

### `cybersecurite-eventix/`

Recense les enjeux de cybersécurité du système Eventix, en distinguant les démarches défensives et les évaluations offensives autorisées. Cette découverte identifie les actifs, impacts, risques et décisions à approfondir ; elle ne choisit pas les mécanismes techniques, qui relèvent des phases de conception ultérieures.

Voir le [cadre de découverte cybersécurité](./cybersecurite-eventix/README.md).

---

## 4. Principe de travail

La découverte du métier doit partir autant que possible de situations réelles, d'observations, d'entretiens, de recherches et d'informations vérifiables.

L'équipe doit distinguer :

- Faits : informations observées ou confirmées.
- Hypothèses : suppositions qui doivent encore être vérifiées.
- Décisions : choix validés par l'équipe.
- Questions ouvertes : informations encore inconnues.

Une hypothèse ne doit pas être considérée comme une règle métier tant qu'elle n'a pas été validée.

Les documents de services et de cybersécurité distinguent également les capacités déjà documentées, les hypothèses à valider et les décisions reportées aux phases suivantes. La présence d'un sujet dans ce dossier ne signifie pas à elle seule qu'une capacité est incluse au MVP.

---

## 5. Relation avec les autres dossiers

Ce dossier se situe entre la vision produit et l'analyse détaillée des besoins.

`text
02-vision-produit
        ↓
Comprendre le métier
        ↓
03-decouverte-du-metier
        ↓
Comprendre les acteurs, besoins,
processus et règles
        ↓
04-analyse-des-besoins
        ↓
Définir les exigences et comportements
du système