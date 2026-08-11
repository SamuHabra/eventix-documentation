# Dépendances fichier-à-fichier — Documentation Eventix



## 01 — Organisation du projet

| Fichier | Dépend de | Pourquoi |
|---|---|---|
| `objectifs.md` | — | Point de départ absolu du projet de documentation. |
| `gouvernance.md` | [objectifs.md](01-organisation-du-projet/objectifs.md) | La gouvernance (qui décide) se définit par rapport aux objectifs fixés. |
| `equipe-et-responsabilites.md` | [gouvernance.md](01-organisation-du-projet/gouvernance.md) | Les rôles et responsabilités découlent du modèle de gouvernance choisi. |
| `processus-de-decision.md` | [gouvernance.md](01-organisation-du-projet/gouvernance.md), [equipe-et-responsabilites.md](01-organisation-du-projet/equipe-et-responsabilites.md) | Le processus de décision s'appuie sur qui a autorité et sur quel rôle. |
| `methode-de-travail.md` | [equipe-et-responsabilites.md](01-organisation-du-projet/equipe-et-responsabilites.md), [processus-de-decision.md](01-organisation-du-projet/processus-de-decision.md) | La méthode organise comment l'équipe applique le processus décisionnel au quotidien. |
| `workflow-de-collaboration.md` | [methode-de-travail.md](01-organisation-du-projet/methode-de-travail.md) | Précise concrètement (outils, rituels) comment appliquer la méthode. |
| `workflow-de-validation.md` | [workflow-de-collaboration.md](01-organisation-du-projet/workflow-de-collaboration.md), [processus-de-decision.md](01-organisation-du-projet/processus-de-decision.md) | La validation est une étape du workflow, gouvernée par le processus de décision. |
| `conventions-documentaires.md` | [methode-de-travail.md](01-organisation-du-projet/methode-de-travail.md) | Les conventions d'écriture découlent de la méthode de travail adoptée. |
| `gestion-des-versions.md` | [workflow-de-validation.md](01-organisation-du-projet/workflow-de-validation.md), [conventions-documentaires.md](01-organisation-du-projet/conventions-documentaires.md) | Versionner un document suppose qu'il ait été validé et respecte les conventions. |
| `criteres-de-fin-de-phase.md` | [workflow-de-validation.md](01-organisation-du-projet/workflow-de-validation.md), [processus-de-decision.md](01-organisation-du-projet/processus-de-decision.md) | Définir "quand une phase est finie" s'appuie sur validation/décision. |
| `README.md` | tous les fichiers ci-dessus | Synthèse de la section. |

## 02 — Vision produit

| Fichier | Dépend de | Pourquoi |
|---|---|---|
| `probleme-metier.md` | [01/objectifs.md](01-organisation-du-projet/objectifs.md) | Le problème adressé doit être cohérent avec les objectifs globaux fixés en 01. |
| `marche-cible.md` | [probleme-metier.md](02-vision-produit/probleme-metier.md) | Le marché cible se définit par rapport au problème qu'on résout. |
| `proposition-de-valeur.md` | [probleme-metier.md](02-vision-produit/probleme-metier.md), [marche-cible.md](02-vision-produit/marche-cible.md) | La proposition de valeur répond au problème pour ce marché précis. |
| `vision-eventix.md` | [proposition-de-valeur.md](02-vision-produit/proposition-de-valeur.md) | La vision formalise la proposition de valeur en une ambition à long terme. |
| `vision-long-terme.md` | [vision-eventix.md](02-vision-produit/vision-eventix.md) | Prolonge la vision produit dans le temps. |
| `objectifs-produit.md` | [vision-eventix.md](02-vision-produit/vision-eventix.md), [marche-cible.md](02-vision-produit/marche-cible.md) | Opérationnalise la vision pour le marché ciblé. |
| `perimetre.md` | [objectifs-produit.md](02-vision-produit/objectifs-produit.md) | Le périmètre découpe ce qui sert directement les objectifs produit. |
| `hors-perimetre.md` | [perimetre.md](02-vision-produit/perimetre.md) | Symétrique du périmètre, ne peut être défini qu'après lui. |
| `mvp.md` | [perimetre.md](02-vision-produit/perimetre.md), [objectifs-produit.md](02-vision-produit/objectifs-produit.md) | Le MVP est un sous-ensemble priorisé du périmètre qui sert les objectifs. |
| `indicateurs-kpi.md` | [objectifs-produit.md](02-vision-produit/objectifs-produit.md), [mvp.md](02-vision-produit/mvp.md) | Les KPI mesurent l'atteinte des objectifs, notamment sur le MVP. |
| `README.md` | tous les fichiers ci-dessus | Synthèse de la section. |

## 03 — Découverte du métier

| Fichier | Dépend de | Pourquoi |
|---|---|---|
| `ecosysteme-eventix.md` | [02/marche-cible.md](02-vision-produit/marche-cible.md), [02/vision-eventix.md](02-vision-produit/vision-eventix.md) | Cartographie l'écosystème autour du marché ciblé et de la vision produit. |
| `acteurs.md` | [ecosysteme-eventix.md](03-decouverte-du-metier/ecosysteme-eventix.md) | Les acteurs sont extraits de la cartographie de l'écosystème. |
| `personas.md` | [acteurs.md](03-decouverte-du-metier/acteurs.md), [02/marche-cible.md](02-vision-produit/marche-cible.md) | Les personas détaillent des acteurs types dans le marché cible. |
| `points-de-contact.md` | [acteurs.md](03-decouverte-du-metier/acteurs.md), [personas.md](03-decouverte-du-metier/personas.md) | Décrit où/comment les acteurs interagissent avec Eventix. |
| `processus-metier.md` | [acteurs.md](03-decouverte-du-metier/acteurs.md), [points-de-contact.md](03-decouverte-du-metier/points-de-contact.md) | Relie acteurs et points de contact en séquences d'actions. |
| `besoins-metier.md` | [personas.md](03-decouverte-du-metier/personas.md), [processus-metier.md](03-decouverte-du-metier/processus-metier.md), [02/probleme-metier.md](02-vision-produit/probleme-metier.md) | Les besoins découlent des personas confrontés aux processus, en réponse au problème initial. |
| `regles-metier.md` | [processus-metier.md](03-decouverte-du-metier/processus-metier.md), [besoins-metier.md](03-decouverte-du-metier/besoins-metier.md) | Les règles encadrent les processus identifiés pour satisfaire les besoins. |
| `contraintes-metier.md` | [regles-metier.md](03-decouverte-du-metier/regles-metier.md), [ecosysteme-eventix.md](03-decouverte-du-metier/ecosysteme-eventix.md) | Les contraintes (légales, organisationnelles) s'ajoutent aux règles et à l'écosystème. |
| `glossaire-metier.md` | [acteurs.md](03-decouverte-du-metier/acteurs.md), [processus-metier.md](03-decouverte-du-metier/processus-metier.md), [regles-metier.md](03-decouverte-du-metier/regles-metier.md) | Consolide le vocabulaire utilisé dans les fichiers précédents. |
| `user-journeys.md` | [personas.md](03-decouverte-du-metier/personas.md), [processus-metier.md](03-decouverte-du-metier/processus-metier.md), [points-de-contact.md](03-decouverte-du-metier/points-de-contact.md) | Combine persona + processus + points de contact. |
| `questions-metier-ouvertes.md` | tous les fichiers ci-dessus de 03 | Capitalise les zones d'ombre identifiées en rédigeant le reste de la section. |
| `README.md` | tous les fichiers ci-dessus | Synthèse de la section. |

## 04 — Analyse des besoins

| Fichier | Dépend de | Pourquoi |
|---|---|---|
| `hypotheses.md` | [02/objectifs-produit.md](02-vision-produit/objectifs-produit.md), [03/questions-metier-ouvertes.md](03-decouverte-du-metier/questions-metier-ouvertes.md) | Formalise les hypothèses de travail, notamment sur les zones encore incertaines de 03. |
| `exigences-fonctionnelles.md` | [03/besoins-metier.md](03-decouverte-du-metier/besoins-metier.md), [03/user-journeys.md](03-decouverte-du-metier/user-journeys.md), [03/regles-metier.md](03-decouverte-du-metier/regles-metier.md) | Traduit besoins/parcours/règles métier en exigences fonctionnelles concrètes. |
| `exigences-non-fonctionnelles.md` | [03/contraintes-metier.md](03-decouverte-du-metier/contraintes-metier.md), [02/objectifs-produit.md](02-vision-produit/objectifs-produit.md) | Dérive des contraintes métier et des objectifs produit (perf, sécurité, dispo). |
| `contraintes.md` | [03/contraintes-metier.md](03-decouverte-du-metier/contraintes-metier.md), [exigences-non-fonctionnelles.md](04-analyse-des-besoins/exigences-non-fonctionnelles.md) | Consolide les contraintes projet au-delà du seul métier. |
| `use-cases.md` | [exigences-fonctionnelles.md](04-analyse-des-besoins/exigences-fonctionnelles.md), [03/acteurs.md](03-decouverte-du-metier/acteurs.md) | Formalise chaque exigence fonctionnelle en cas d'usage porté par un acteur. |
| `user-stories.md` | [use-cases.md](04-analyse-des-besoins/use-cases.md), [03/personas.md](03-decouverte-du-metier/personas.md) | Reformule les cas d'usage du point de vue des personas, format agile. |
| `criteres-d-acceptation.md` | [user-stories.md](04-analyse-des-besoins/user-stories.md) | Chaque user story a besoin de critères vérifiables. |
| `priorisation.md` | [user-stories.md](04-analyse-des-besoins/user-stories.md), [02/mvp.md](02-vision-produit/mvp.md), [hypotheses.md](04-analyse-des-besoins/hypotheses.md) | Priorise selon le MVP visé et les hypothèses de valeur/risque. |
| `matrice-de-tracabilite.md` | [exigences-fonctionnelles.md](04-analyse-des-besoins/exigences-fonctionnelles.md), [exigences-non-fonctionnelles.md](04-analyse-des-besoins/exigences-non-fonctionnelles.md), [use-cases.md](04-analyse-des-besoins/use-cases.md), [user-stories.md](04-analyse-des-besoins/user-stories.md), [03/besoins-metier.md](03-decouverte-du-metier/besoins-metier.md) | Trace chaque exigence/story jusqu'à son origine métier — nécessite que tout le reste existe. |
| `README.md` | tous les fichiers ci-dessus | Synthèse de la section. |

## 05 — Domain-Driven Design

| Fichier | Dépend de | Pourquoi |
|---|---|---|
| `vue-d-ensemble-du-domaine.md` | [03/ecosysteme-eventix.md](03-decouverte-du-metier/ecosysteme-eventix.md), [04/exigences-fonctionnelles.md](04-analyse-des-besoins/exigences-fonctionnelles.md) | Pose une vue globale du domaine à partir de l'écosystème et des exigences déjà cadrées. |
| `domaines.md` | [vue-d-ensemble-du-domaine.md](05-domain-driven-design/vue-d-ensemble-du-domaine.md) | Découpe la vue d'ensemble en domaines distincts. |
| `sous-domaines.md` | [domaines.md](05-domain-driven-design/domaines.md) | Affine chaque domaine en sous-domaines. |
| `core-domain.md` | [sous-domaines.md](05-domain-driven-design/sous-domaines.md), [02/proposition-de-valeur.md](02-vision-produit/proposition-de-valeur.md) | Identifie le sous-domaine différenciant, en lien avec la proposition de valeur. |
| `bounded-contexts.md` | [sous-domaines.md](05-domain-driven-design/sous-domaines.md), [core-domain.md](05-domain-driven-design/core-domain.md) | Délimite les frontières logiques autour des sous-domaines et du core domain. |
| `context-map.md` | [bounded-contexts.md](05-domain-driven-design/bounded-contexts.md) | Cartographie les relations entre les bounded contexts définis. |
| `langage-ubiquitaire.md` | [03/glossaire-metier.md](03-decouverte-du-metier/glossaire-metier.md), [bounded-contexts.md](05-domain-driven-design/bounded-contexts.md) | Précise, par bounded context, le vocabulaire partagé issu du glossaire métier. |
| `entites.md` | [bounded-contexts.md](05-domain-driven-design/bounded-contexts.md), [langage-ubiquitaire.md](05-domain-driven-design/langage-ubiquitaire.md) | Identifie les entités du domaine avec le vocabulaire validé. |
| `objets-valeur.md` | [entites.md](05-domain-driven-design/entites.md), [langage-ubiquitaire.md](05-domain-driven-design/langage-ubiquitaire.md) | Complète les entités par les objets sans identité propre. |
| `agregats.md` | [entites.md](05-domain-driven-design/entites.md), [objets-valeur.md](05-domain-driven-design/objets-valeur.md) | Regroupe entités/objets-valeur en agrégats cohérents. |
| `services-de-domaine.md` | [agregats.md](05-domain-driven-design/agregats.md), [03/processus-metier.md](03-decouverte-du-metier/processus-metier.md) | Modélise les opérations métier qui n'appartiennent à aucun agrégat seul. |
| `evenements-de-domaine.md` | [agregats.md](05-domain-driven-design/agregats.md), [services-de-domaine.md](05-domain-driven-design/services-de-domaine.md) | Décrit les événements produits par les changements d'état des agrégats/services. |
| `regles-du-domaine.md` | [03/regles-metier.md](03-decouverte-du-metier/regles-metier.md), [agregats.md](05-domain-driven-design/agregats.md), [evenements-de-domaine.md](05-domain-driven-design/evenements-de-domaine.md) | Traduit formellement les règles métier au niveau du modèle de domaine. |
| `README.md` | tous les fichiers ci-dessus | Synthèse de la section. |

## 06 — Modélisation UML

| Fichier | Dépend de | Pourquoi |
|---|---|---|
| `diagramme-de-contexte.md` | [05/context-map.md](05-domain-driven-design/context-map.md) | Visualise le système et ses acteurs externes à partir de la carte des contextes. |
| `diagrammes-de-cas-d-utilisation.md` | [04/use-cases.md](04-analyse-des-besoins/use-cases.md), [diagramme-de-contexte.md](06-modelisation-uml/diagramme-de-contexte.md) | Formalise en UML les cas d'usage déjà rédigés en texte. |
| `diagrammes-de-classes.md` | [05/entites.md](05-domain-driven-design/entites.md), [05/objets-valeur.md](05-domain-driven-design/objets-valeur.md), [05/agregats.md](05-domain-driven-design/agregats.md) | Transpose directement le modèle DDD en diagramme de classes. |
| `diagrammes-de-sequence.md` | [diagrammes-de-cas-d-utilisation.md](06-modelisation-uml/diagrammes-de-cas-d-utilisation.md), [05/services-de-domaine.md](05-domain-driven-design/services-de-domaine.md) | Détaille l'enchaînement d'appels pour chaque cas d'usage/service. |
| `diagrammes-d-activite.md` | [diagrammes-de-sequence.md](06-modelisation-uml/diagrammes-de-sequence.md), [03/processus-metier.md](03-decouverte-du-metier/processus-metier.md) | Modélise les flux de contrôle des processus déjà décrits. |
| `diagrammes-d-etat.md` | [05/agregats.md](05-domain-driven-design/agregats.md), [05/evenements-de-domaine.md](05-domain-driven-design/evenements-de-domaine.md) | Montre les transitions d'état des agrégats déclenchées par les événements. |
| `diagrammes-de-composants.md` | [diagrammes-de-classes.md](06-modelisation-uml/diagrammes-de-classes.md), [05/bounded-contexts.md](05-domain-driven-design/bounded-contexts.md) | Regroupe les classes en composants alignés sur les bounded contexts. |
| `diagrammes-de-deploiement.md` | [diagrammes-de-composants.md](06-modelisation-uml/diagrammes-de-composants.md) | Place les composants sur une infrastructure (encore préliminaire à ce stade). |
| `README.md` | tous les fichiers ci-dessus | Synthèse de la section. |

## 07 — Architecture logique

| Fichier | Dépend de | Pourquoi |
|---|---|---|
| `principes-architecturaux.md` | [02/objectifs-produit.md](02-vision-produit/objectifs-produit.md), [04/exigences-non-fonctionnelles.md](04-analyse-des-besoins/exigences-non-fonctionnelles.md) | Fixe les principes directeurs en cohérence avec objectifs et exigences non-fonctionnelles. |
| `decomposition-fonctionnelle.md` | [principes-architecturaux.md](07-architecture-logique/principes-architecturaux.md), [05/bounded-contexts.md](05-domain-driven-design/bounded-contexts.md) | Découpe le système en blocs fonctionnels alignés sur les bounded contexts. |
| `modules.md` | [decomposition-fonctionnelle.md](07-architecture-logique/decomposition-fonctionnelle.md), [06/diagrammes-de-composants.md](06-modelisation-uml/diagrammes-de-composants.md) | Concrétise la décomposition fonctionnelle en modules logiciels. |
| `responsabilites.md` | [modules.md](07-architecture-logique/modules.md), [05/services-de-domaine.md](05-domain-driven-design/services-de-domaine.md) | Attribue une responsabilité claire à chaque module. |
| `interfaces.md` | [modules.md](07-architecture-logique/modules.md), [responsabilites.md](07-architecture-logique/responsabilites.md) | Définit les contrats d'échange entre modules selon leurs responsabilités. |
| `communication.md` | [interfaces.md](07-architecture-logique/interfaces.md) | Précise les modes de communication (sync/async) qui implémentent les interfaces. |
| `flux-metier.md` | [communication.md](07-architecture-logique/communication.md), [06/diagrammes-de-sequence.md](06-modelisation-uml/diagrammes-de-sequence.md) | Décrit les flux bout-en-bout à partir des communications inter-modules et des séquences UML. |
| `dependances.md` | [modules.md](07-architecture-logique/modules.md), [interfaces.md](07-architecture-logique/interfaces.md), [flux-metier.md](07-architecture-logique/flux-metier.md) | Formalise le graphe de dépendances entre modules une fois interfaces et flux connus. |
| `decisions-architecturales.md` | [principes-architecturaux.md](07-architecture-logique/principes-architecturaux.md), [dependances.md](07-architecture-logique/dependances.md) | Documente les choix/arbitrages faits en construisant la section. |
| `README.md` | tous les fichiers ci-dessus | Synthèse de la section. |

## 08 — Architecture des données

| Fichier | Dépend de | Pourquoi |
|---|---|---|
| `modele-conceptuel.md` | [05/entites.md](05-domain-driven-design/entites.md), [05/objets-valeur.md](05-domain-driven-design/objets-valeur.md) | Transpose les entités/objets-valeur du domaine en modèle conceptuel de données. |
| `modele-logique.md` | [modele-conceptuel.md](08-architecture-des-donnees/modele-conceptuel.md), [05/agregats.md](05-domain-driven-design/agregats.md) | Structure le modèle conceptuel en respectant les frontières d'agrégats. |
| `entites-et-relations.md` | [modele-logique.md](08-architecture-des-donnees/modele-logique.md) | Détaille les relations (cardinalités, clés) du modèle logique. |
| `modele-physique.md` | [entites-et-relations.md](08-architecture-des-donnees/entites-et-relations.md), [09/choix-technologiques.md](09-architecture-technique/choix-technologiques.md) | Implémente le modèle logique dans un SGBD concret — *dépendance croisée : peut nécessiter un aller-retour avec 09 si le choix de SGBD n'est pas encore arrêté.* |
| `indexation.md` | [modele-physique.md](08-architecture-des-donnees/modele-physique.md) | Les index se définissent sur les tables/colonnes du modèle physique. |
| `partitionnement.md` | [modele-physique.md](08-architecture-des-donnees/modele-physique.md), [entites-et-relations.md](08-architecture-des-donnees/entites-et-relations.md) | Le partitionnement s'appuie sur le volume/relations identifiés. |
| `replication.md` | [modele-physique.md](08-architecture-des-donnees/modele-physique.md) | Stratégie de réplication définie sur le schéma physique. |
| `transactions.md` | [entites-et-relations.md](08-architecture-des-donnees/entites-et-relations.md), [modele-physique.md](08-architecture-des-donnees/modele-physique.md) | Les frontières transactionnelles suivent les relations et agrégats physiques. |
| `integrite-et-consistance.md` | [transactions.md](08-architecture-des-donnees/transactions.md), [replication.md](08-architecture-des-donnees/replication.md) | Les règles d'intégrité doivent tenir compte des transactions et de la réplication choisie. |
| `strategie-de-stockage.md` | [modele-physique.md](08-architecture-des-donnees/modele-physique.md), [partitionnement.md](08-architecture-des-donnees/partitionnement.md) | Choisit le type de stockage adapté au modèle physique et à son partitionnement. |
| `sauvegarde-et-retention.md` | [strategie-de-stockage.md](08-architecture-des-donnees/strategie-de-stockage.md), [integrite-et-consistance.md](08-architecture-des-donnees/integrite-et-consistance.md) | La politique de sauvegarde dépend du stockage choisi et des exigences d'intégrité. |
| `README.md` | tous les fichiers ci-dessus | Synthèse de la section. |

## 09 — Architecture technique

| Fichier | Dépend de | Pourquoi |
|---|---|---|
| `vue-d-ensemble.md` | [07/decomposition-fonctionnelle.md](07-architecture-logique/decomposition-fonctionnelle.md), [07/modules.md](07-architecture-logique/modules.md) | Pose la vue technique globale à partir du découpage fonctionnel déjà arrêté. |
| `choix-technologiques.md` | [vue-d-ensemble.md](09-architecture-technique/vue-d-ensemble.md), [04/exigences-non-fonctionnelles.md](04-analyse-des-besoins/exigences-non-fonctionnelles.md) | Les choix techno doivent satisfaire les exigences non-fonctionnelles dans le cadre de la vue d'ensemble. |
| `architecture-des-api.md` | [choix-technologiques.md](09-architecture-technique/choix-technologiques.md), [07/interfaces.md](07-architecture-logique/interfaces.md) | Implémente concrètement les interfaces logiques définies en 07. |
| `protocoles.md` | [architecture-des-api.md](09-architecture-technique/architecture-des-api.md), [07/communication.md](07-architecture-logique/communication.md) | Précise les protocoles réseau qui portent les communications définies en 07. |
| `authentification.md` | [architecture-des-api.md](09-architecture-technique/architecture-des-api.md) | Sécurise l'accès aux API définies. |
| `autorisation.md` | [authentification.md](09-architecture-technique/authentification.md), [05/agregats.md](05-domain-driven-design/agregats.md) | Les droits d'accès s'appliquent aux agrégats/ressources métier, après authentification. |
| `cache.md` | [architecture-des-api.md](09-architecture-technique/architecture-des-api.md), [08/modele-logique.md](08-architecture-des-donnees/modele-logique.md) | Le cache s'appuie sur les données/API à accélérer. |
| `messagerie.md` | [protocoles.md](09-architecture-technique/protocoles.md), [07/flux-metier.md](07-architecture-logique/flux-metier.md) | Implémente les flux métier asynchrones identifiés en 07. |
| `traitements-asynchrones.md` | [messagerie.md](09-architecture-technique/messagerie.md) | Décrit le traitement des messages produits par la messagerie. |
| `stockage.md` | [08/strategie-de-stockage.md](08-architecture-des-donnees/strategie-de-stockage.md), [choix-technologiques.md](09-architecture-technique/choix-technologiques.md) | Implémentation technique concrète de la stratégie de stockage choisie en 08. |
| `decisions-architecturales-adr.md` | tous les fichiers ci-dessus de 09 | Documente sous forme d'ADR les choix faits dans toute la section. |
| `README.md` | tous les fichiers ci-dessus | Synthèse de la section. |

## 10 — Infrastructure

| Fichier | Dépend de | Pourquoi |
|---|---|---|
| `cloud-et-hebergement.md` | [09/choix-technologiques.md](09-architecture-technique/choix-technologiques.md) | Le choix du cloud/hébergeur découle de la stack technique retenue. |
| `architecture-physique.md` | [cloud-et-hebergement.md](10-infrastructure/cloud-et-hebergement.md), [09/vue-d-ensemble.md](09-architecture-technique/vue-d-ensemble.md) | Place les composants techniques sur l'infrastructure physique choisie. |
| `reseau.md` | [architecture-physique.md](10-infrastructure/architecture-physique.md) | Conçoit le réseau qui relie les composants physiques. |
| `dns.md` | [reseau.md](10-infrastructure/reseau.md) | La résolution de noms s'appuie sur la topologie réseau définie. |
| `load-balancing.md` | [reseau.md](10-infrastructure/reseau.md), [architecture-physique.md](10-infrastructure/architecture-physique.md) | Répartit la charge entre les instances physiques déjà positionnées. |
| `cdn.md` | [dns.md](10-infrastructure/dns.md), [09/architecture-des-api.md](09-architecture-technique/architecture-des-api.md) | Distribue le contenu/API en s'appuyant sur le DNS et les endpoints définis. |
| `compute.md` | [cloud-et-hebergement.md](10-infrastructure/cloud-et-hebergement.md), [architecture-physique.md](10-infrastructure/architecture-physique.md) | Dimensionne les ressources de calcul sur l'infra choisie. |
| `conteneurs.md` | [compute.md](10-infrastructure/compute.md), [09/choix-technologiques.md](09-architecture-technique/choix-technologiques.md) | La conteneurisation s'appuie sur les ressources de calcul et la stack technique. |
| `environnements.md` | [conteneurs.md](10-infrastructure/conteneurs.md), [load-balancing.md](10-infrastructure/load-balancing.md) | Définit dev/staging/prod à partir des briques d'infra déjà posées. |
| `iam-et-secrets.md` | [environnements.md](10-infrastructure/environnements.md), [09/authentification.md](09-architecture-technique/authentification.md) | Gère les accès/secrets par environnement, en cohérence avec l'auth applicative. |
| `deploiement.md` | [environnements.md](10-infrastructure/environnements.md), [conteneurs.md](10-infrastructure/conteneurs.md), [iam-et-secrets.md](10-infrastructure/iam-et-secrets.md) | Le pipeline de déploiement orchestre tout ce qui précède. |
| `README.md` | tous les fichiers ci-dessus | Synthèse de la section. |

## 11 — Scalabilité

| Fichier | Dépend de | Pourquoi |
|---|---|---|
| `hypotheses-de-charge.md` | [02/indicateurs-kpi.md](02-vision-produit/indicateurs-kpi.md), [04/exigences-non-fonctionnelles.md](04-analyse-des-besoins/exigences-non-fonctionnelles.md) | Pose les hypothèses de volumétrie à partir des KPI et exigences non-fonctionnelles. |
| `modele-de-trafic.md` | [hypotheses-de-charge.md](11-scalabilite/hypotheses-de-charge.md), [03/user-journeys.md](03-decouverte-du-metier/user-journeys.md) | Modélise le trafic réel attendu selon les parcours utilisateurs et hypothèses de charge. |
| `capacite.md` | [modele-de-trafic.md](11-scalabilite/modele-de-trafic.md), [10/compute.md](10-infrastructure/compute.md) | Calcule la capacité nécessaire sur l'infra de calcul déjà définie. |
| `scalabilite-horizontale.md` | [capacite.md](11-scalabilite/capacite.md), [10/load-balancing.md](10-infrastructure/load-balancing.md) | S'appuie sur le load balancing déjà en place. |
| `scalabilite-verticale.md` | [capacite.md](11-scalabilite/capacite.md), [10/compute.md](10-infrastructure/compute.md) | Alternative/complément à l'horizontale, sur les mêmes ressources compute. |
| `partitionnement.md` | [08/partitionnement.md](08-architecture-des-donnees/partitionnement.md), [capacite.md](11-scalabilite/capacite.md) | Étend le partitionnement des données (08) sous l'angle de la montée en charge. |
| `sharding.md` | [partitionnement.md](11-scalabilite/partitionnement.md), [08/modele-physique.md](08-architecture-des-donnees/modele-physique.md) | Forme avancée de partitionnement appliquée au modèle physique. |
| `replication.md` | [08/replication.md](08-architecture-des-donnees/replication.md), [sharding.md](11-scalabilite/sharding.md) | Adapte la réplication des données (08) au contexte de scalabilité/sharding. |
| `cache-distribue.md` | [09/cache.md](09-architecture-technique/cache.md), [scalabilite-horizontale.md](11-scalabilite/scalabilite-horizontale.md) | Distribue le cache applicatif (09) sur une infra scalée horizontalement. |
| `strategie-de-croissance.md` | tous les fichiers ci-dessus de 11 | Synthétise la trajectoire de scalabilité, alimentera la roadmap (17). |
| `README.md` | tous les fichiers ci-dessus | Synthèse de la section. |

## 12 — Performance

| Fichier | Dépend de | Pourquoi |
|---|---|---|
| `objectifs-de-performance.md` | [04/exigences-non-fonctionnelles.md](04-analyse-des-besoins/exigences-non-fonctionnelles.md), [02/indicateurs-kpi.md](02-vision-produit/indicateurs-kpi.md) | Fixe des cibles chiffrées à partir des exigences non-fonctionnelles et des KPI produit. |
| `latence.md` | [objectifs-de-performance.md](12-performance/objectifs-de-performance.md), [09/architecture-des-api.md](09-architecture-technique/architecture-des-api.md) | Analyse la latence des API par rapport aux cibles fixées. |
| `debit-throughput.md` | [objectifs-de-performance.md](12-performance/objectifs-de-performance.md), [11/capacite.md](11-scalabilite/capacite.md) | Évalue le débit soutenable compte tenu de la capacité dimensionnée. |
| `bottlenecks.md` | [latence.md](12-performance/latence.md), [debit-throughput.md](12-performance/debit-throughput.md) | Identifie les goulots d'étranglement à partir des mesures de latence/débit. |
| `capacity-planning.md` | [bottlenecks.md](12-performance/bottlenecks.md), [11/strategie-de-croissance.md](11-scalabilite/strategie-de-croissance.md) | Planifie la capacité future en tenant compte des goulots et de la trajectoire de croissance. |
| `load-testing.md` | [objectifs-de-performance.md](12-performance/objectifs-de-performance.md), [capacity-planning.md](12-performance/capacity-planning.md) | Définit les scénarios de test pour valider les cibles et le plan de capacité. |
| `stress-testing.md` | [load-testing.md](12-performance/load-testing.md) | Pousse au-delà des scénarios de charge nominale déjà définis. |
| `optimisation.md` | [bottlenecks.md](12-performance/bottlenecks.md), [stress-testing.md](12-performance/stress-testing.md) | Propose des optimisations à partir des goulots et des limites observées en tests. |
| `README.md` | tous les fichiers ci-dessus | Synthèse de la section. |

## 13 — Sécurité

| Fichier | Dépend de | Pourquoi |
|---|---|---|
| `modele-de-menaces.md` | [07/decomposition-fonctionnelle.md](07-architecture-logique/decomposition-fonctionnelle.md), [09/vue-d-ensemble.md](09-architecture-technique/vue-d-ensemble.md) | Identifie les menaces sur la surface d'attaque définie par l'architecture. |
| `authentification.md` | [modele-de-menaces.md](13-securite/modele-de-menaces.md), [09/authentification.md](09-architecture-technique/authentification.md) | Précise la politique de sécurité par-dessus le mécanisme technique déjà posé en 09. |
| `autorisation.md` | [authentification.md](13-securite/authentification.md), [09/autorisation.md](09-architecture-technique/autorisation.md) | Politique de sécurité sur le mécanisme technique d'autorisation. |
| `chiffrement.md` | [modele-de-menaces.md](13-securite/modele-de-menaces.md), [08/strategie-de-stockage.md](08-architecture-des-donnees/strategie-de-stockage.md) | Définit le chiffrement au repos/en transit selon les menaces et le stockage choisi. |
| `protection-des-donnees.md` | [chiffrement.md](13-securite/chiffrement.md), [08/entites-et-relations.md](08-architecture-des-donnees/entites-et-relations.md) | Applique la protection aux données identifiées dans le modèle. |
| `securite-api.md` | [09/architecture-des-api.md](09-architecture-technique/architecture-des-api.md), [autorisation.md](13-securite/autorisation.md) | Sécurise les API définies techniquement, selon les règles d'autorisation. |
| `securite-des-paiements.md` | [securite-api.md](13-securite/securite-api.md), [protection-des-donnees.md](13-securite/protection-des-donnees.md) | Cas particulier sensible des API et données, avec exigences propres (PCI-DSS etc.). |
| `gestion-des-secrets.md` | [10/iam-et-secrets.md](10-infrastructure/iam-et-secrets.md), [chiffrement.md](13-securite/chiffrement.md) | Politique de gestion des secrets par-dessus l'implémentation IAM déjà posée en 10. |
| `audit.md` | [authentification.md](13-securite/authentification.md), [autorisation.md](13-securite/autorisation.md), [gestion-des-secrets.md](13-securite/gestion-des-secrets.md) | L'audit trace les accès/actions couverts par ces mécanismes. |
| `gestion-des-incidents.md` | [modele-de-menaces.md](13-securite/modele-de-menaces.md), [audit.md](13-securite/audit.md) | Le plan de réponse s'appuie sur les menaces anticipées et les capacités d'audit. |
| `README.md` | tous les fichiers ci-dessus | Synthèse de la section. |

## 14 — Observabilité

| Fichier | Dépend de | Pourquoi |
|---|---|---|
| `metriques.md` | [09/vue-d-ensemble.md](09-architecture-technique/vue-d-ensemble.md), [12/objectifs-de-performance.md](12-performance/objectifs-de-performance.md) | Définit quoi mesurer à partir de l'architecture et des cibles de performance. |
| `journalisation-logs.md` | [09/vue-d-ensemble.md](09-architecture-technique/vue-d-ensemble.md) | Définit la stratégie de logs sur les composants de l'architecture technique. |
| `traces-distribuees.md` | [07/flux-metier.md](07-architecture-logique/flux-metier.md), [09/messagerie.md](09-architecture-technique/messagerie.md) | Trace les flux distribués identifiés en architecture logique/technique. |
| `health-checks.md` | [10/deploiement.md](10-infrastructure/deploiement.md) | S'intègre au pipeline/orchestration de déploiement. |
| `sli.md` | [metriques.md](14-observabilite/metriques.md) | Les indicateurs de niveau de service sont construits à partir des métriques collectées. |
| `slo.md` | [sli.md](14-observabilite/sli.md), [12/objectifs-de-performance.md](12-performance/objectifs-de-performance.md) | Fixe des objectifs cibles sur les SLI, alignés sur les objectifs de performance. |
| `sla.md` | [slo.md](14-observabilite/slo.md) | Engagement contractuel/externe basé sur les SLO internes. |
| `alerting.md` | [slo.md](14-observabilite/slo.md), [sli.md](14-observabilite/sli.md) | Déclenche des alertes quand les SLI s'écartent des SLO. |
| `tableaux-de-bord.md` | [metriques.md](14-observabilite/metriques.md), [sli.md](14-observabilite/sli.md), [slo.md](14-observabilite/slo.md) | Visualise l'ensemble des indicateurs et objectifs définis. |
| `README.md` | tous les fichiers ci-dessus | Synthèse de la section. |

## 15 — Résilience

| Fichier | Dépend de | Pourquoi |
|---|---|---|
| `modes-de-defaillance.md` | [09/vue-d-ensemble.md](09-architecture-technique/vue-d-ensemble.md), [10/architecture-physique.md](10-infrastructure/architecture-physique.md) | Identifie ce qui peut casser dans l'architecture technique et physique. |
| `timeouts.md` | [modes-de-defaillance.md](15-resilience/modes-de-defaillance.md), [09/protocoles.md](09-architecture-technique/protocoles.md) | Se règlent en fonction des modes de défaillance et des protocoles réseau utilisés. |
| `retry.md` | [timeouts.md](15-resilience/timeouts.md) | La stratégie de réessai s'articule avec les timeouts déjà définis. |
| `circuit-breaker.md` | [retry.md](15-resilience/retry.md), [modes-de-defaillance.md](15-resilience/modes-de-defaillance.md) | Complète les retries pour éviter les cascades de pannes. |
| `idempotence.md` | [retry.md](15-resilience/retry.md), [09/traitements-asynchrones.md](09-architecture-technique/traitements-asynchrones.md) | Nécessaire pour que les retries soient sûrs, en particulier sur les traitements asynchrones. |
| `failover.md` | [circuit-breaker.md](15-resilience/circuit-breaker.md), [11/replication.md](11-scalabilite/replication.md) | Bascule vers une instance de secours, en s'appuyant sur la réplication déjà en place. |
| `sauvegardes.md` | [08/sauvegarde-et-retention.md](08-architecture-des-donnees/sauvegarde-et-retention.md), [modes-de-defaillance.md](15-resilience/modes-de-defaillance.md) | Politique opérationnelle par-dessus la stratégie de rétention déjà posée en 08. |
| `disaster-recovery.md` | [failover.md](15-resilience/failover.md), [sauvegardes.md](15-resilience/sauvegardes.md) | Combine bascule et sauvegardes pour un plan de reprise complet. |
| `rpo-rto.md` | [disaster-recovery.md](15-resilience/disaster-recovery.md) | Fixe des objectifs chiffrés (perte de données/temps d'arrêt) sur le plan défini. |
| `tolerance-aux-pannes.md` | tous les fichiers ci-dessus de 15 | Synthèse de la posture globale de tolérance aux pannes. |
| `README.md` | tous les fichiers ci-dessus | Synthèse de la section. |

## 16 — Analyse des coûts

| Fichier | Dépend de | Pourquoi |
|---|---|---|
| `hypotheses-financieres.md` | [11/hypotheses-de-charge.md](11-scalabilite/hypotheses-de-charge.md), [02/marche-cible.md](02-vision-produit/marche-cible.md) | Pose les hypothèses de volumétrie/pricing à partir de la charge attendue et du marché visé. |
| `cout-infrastructure.md` | [10/cloud-et-hebergement.md](10-infrastructure/cloud-et-hebergement.md), [10/compute.md](10-infrastructure/compute.md), [hypotheses-financieres.md](16-analyse-des-couts/hypotheses-financieres.md) | Chiffre l'infra choisie selon les hypothèses financières. |
| `cout-base-de-donnees.md` | [08/modele-physique.md](08-architecture-des-donnees/modele-physique.md), [hypotheses-financieres.md](16-analyse-des-couts/hypotheses-financieres.md) | Chiffre le coût du SGBD/stockage de données choisi. |
| `cout-stockage.md` | [08/strategie-de-stockage.md](08-architecture-des-donnees/strategie-de-stockage.md), [hypotheses-financieres.md](16-analyse-des-couts/hypotheses-financieres.md) | Chiffre le stockage de fichiers/objets selon la stratégie retenue. |
| `cout-reseau.md` | [10/reseau.md](10-infrastructure/reseau.md), [hypotheses-financieres.md](16-analyse-des-couts/hypotheses-financieres.md) | Chiffre le trafic réseau selon la topologie définie. |
| `cout-cdn.md` | [10/cdn.md](10-infrastructure/cdn.md), [hypotheses-financieres.md](16-analyse-des-couts/hypotheses-financieres.md) | Chiffre l'usage du CDN. |
| `cout-paiements.md` | [13/securite-des-paiements.md](13-securite/securite-des-paiements.md), [hypotheses-financieres.md](16-analyse-des-couts/hypotheses-financieres.md) | Chiffre les frais liés au traitement des paiements. |
| `cout-notifications.md` | [09/messagerie.md](09-architecture-technique/messagerie.md), [hypotheses-financieres.md](16-analyse-des-couts/hypotheses-financieres.md) | Chiffre l'envoi de notifications via la messagerie définie. |
| `cout-par-transaction.md` | [cout-infrastructure.md](16-analyse-des-couts/cout-infrastructure.md), [cout-base-de-donnees.md](16-analyse-des-couts/cout-base-de-donnees.md), [cout-paiements.md](16-analyse-des-couts/cout-paiements.md) | Agrège les coûts unitaires par transaction. |
| `cout-par-utilisateur.md` | [cout-par-transaction.md](16-analyse-des-couts/cout-par-transaction.md), [11/modele-de-trafic.md](11-scalabilite/modele-de-trafic.md) | Ramène les coûts agrégés à un coût par utilisateur selon le modèle de trafic. |
| `optimisation-des-couts.md` | tous les fichiers `cout-*` ci-dessus | Identifie les leviers d'optimisation une fois tous les postes de coût chiffrés. |
| `README.md` | tous les fichiers ci-dessus | Synthèse de la section. |

## 17 — Roadmap produit

| Fichier | Dépend de | Pourquoi |
|---|---|---|
| `priorites.md` | [04/priorisation.md](04-analyse-des-besoins/priorisation.md), [02/objectifs-produit.md](02-vision-produit/objectifs-produit.md) | Transforme la priorisation des besoins en priorités de roadmap. |
| `mvp.md` | [02/mvp.md](02-vision-produit/mvp.md), [priorites.md](17-roadmap-produit/priorites.md) | Précise le contenu réel du MVP en ajustant le MVP produit initial avec les priorités. |
| `version-1.md` | [mvp.md](17-roadmap-produit/mvp.md), [priorites.md](17-roadmap-produit/priorites.md) | La V1 étend le MVP selon les priorités suivantes. |
| `version-2.md` | [version-1.md](17-roadmap-produit/version-1.md) | Prolonge la trajectoire après la V1. |
| `fonctionnalites-futures.md` | [version-2.md](17-roadmap-produit/version-2.md), [02/hors-perimetre.md](02-vision-produit/hors-perimetre.md) | Reprend ce qui était hors périmètre initial pour l'envisager plus tard. |
| `dette-technique.md` | [09/decisions-architecturales-adr.md](09-architecture-technique/decisions-architecturales-adr.md) | Liste la dette issue des choix techniques déjà faits (ADR). |
| `risques.md` | [version-1.md](17-roadmap-produit/version-1.md), [dette-technique.md](17-roadmap-produit/dette-technique.md), [16/optimisation-des-couts.md](16-analyse-des-couts/optimisation-des-couts.md) | Consolide les risques produit/technique/financier de la roadmap. |
| `README.md` | tous les fichiers ci-dessus | Synthèse de la section. |

## 18 — Documentation finale

| Fichier | Dépend de | Pourquoi |
|---|---|---|
| `vue-d-ensemble.md` | [02/vision-eventix.md](02-vision-produit/vision-eventix.md), [07/decomposition-fonctionnelle.md](07-architecture-logique/decomposition-fonctionnelle.md) | Synthèse d'ensemble du produit et de l'architecture logique. |
| `architecture-globale.md` | [07/dependances.md](07-architecture-logique/dependances.md), [09/vue-d-ensemble.md](09-architecture-technique/vue-d-ensemble.md), [10/architecture-physique.md](10-infrastructure/architecture-physique.md) | Consolide les architectures logique, technique et physique en une vue unique. |
| `system-design.md` | [architecture-globale.md](18-documentation-finale/architecture-globale.md), [08/modele-physique.md](08-architecture-des-donnees/modele-physique.md) | Document de référence combinant architecture globale et modèle de données. |
| `documentation-technique.md` | [system-design.md](18-documentation-finale/system-design.md), [09/decisions-architecturales-adr.md](09-architecture-technique/decisions-architecturales-adr.md) | Documente en détail l'implémentation technique et ses décisions. |
| `documentation-metier.md` | [03/glossaire-metier.md](03-decouverte-du-metier/glossaire-metier.md), [05/regles-du-domaine.md](05-domain-driven-design/regles-du-domaine.md) | Documente le métier et les règles de domaine pour un public non technique. |
| `documentation-exploitation.md` | [10/deploiement.md](10-infrastructure/deploiement.md), [14/tableaux-de-bord.md](14-observabilite/tableaux-de-bord.md), [15/tolerance-aux-pannes.md](15-resilience/tolerance-aux-pannes.md) | Documente comment exploiter le système en production. |
| `runbooks.md` | [documentation-exploitation.md](18-documentation-finale/documentation-exploitation.md), [13/gestion-des-incidents.md](13-securite/gestion-des-incidents.md) | Procédures opérationnelles précises pour réagir aux incidents. |
| `glossaire-final.md` | [03/glossaire-metier.md](03-decouverte-du-metier/glossaire-metier.md), [05/langage-ubiquitaire.md](05-domain-driven-design/langage-ubiquitaire.md) | Fusionne les glossaires métier et technique en un glossaire unique final. |
| `README.md` | tous les fichiers ci-dessus | Synthèse de la section. |

## 19 — Revue d'architecture

| Fichier | Dépend de | Pourquoi |
|---|---|---|
| `audit-global.md` | [18/architecture-globale.md](18-documentation-finale/architecture-globale.md) | Point de départ de la revue, sur la base de la documentation finale consolidée. |
| `revue-de-securite.md` | [audit-global.md](19-revue-d-architecture/audit-global.md), [13/README.md](13-securite/README.md) | Réexamine la section sécurité à la lumière de l'audit global. |
| `revue-de-performance.md` | [audit-global.md](19-revue-d-architecture/audit-global.md), [12/README.md](12-performance/README.md) | Réexamine la section performance. |
| `revue-de-scalabilite.md` | [audit-global.md](19-revue-d-architecture/audit-global.md), [11/README.md](11-scalabilite/README.md) | Réexamine la section scalabilité. |
| `revue-des-couts.md` | [audit-global.md](19-revue-d-architecture/audit-global.md), [16/README.md](16-analyse-des-couts/README.md) | Réexamine la section coûts. |
| `analyse-des-risques.md` | [revue-de-securite.md](19-revue-d-architecture/revue-de-securite.md), [revue-de-performance.md](19-revue-d-architecture/revue-de-performance.md), [revue-de-scalabilite.md](19-revue-d-architecture/revue-de-scalabilite.md), [revue-des-couts.md](19-revue-d-architecture/revue-des-couts.md) | Consolide les risques identifiés dans les 4 revues thématiques. |
| `decisions-a-revoir.md` | [analyse-des-risques.md](19-revue-d-architecture/analyse-des-risques.md) | Liste les décisions à reconsidérer suite à l'analyse de risques. |
| `validation-finale.md` | [decisions-a-revoir.md](19-revue-d-architecture/decisions-a-revoir.md) | Dernière étape : valide (ou non) le projet une fois les décisions arbitrées. |
| `README.md` | tous les fichiers ci-dessus | Synthèse de la section. |

---

**À noter** : ce fichier doit lui aussi être placé directement dans `docs/`, au même niveau que les 19 dossiers de section, pour que tous les liens relatifs fonctionnent.
