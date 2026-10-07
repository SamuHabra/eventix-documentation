# 05 — Domain-Driven Design

| Champ | Valeur |
|---|---|
| **Phase** | 05 — Domain-Driven Design |
| **Projet** | Eventix |
| **Périmètre** | MVP |
| **Marché initial** | Cameroun |
| **Statut** | À valider par l'équipe |

---

## 1. Rôle de la section

Cette section établit le modèle de domaine d'Eventix : elle descend graduellement du métier vers la structure, sans prendre aucune décision technique.

Elle consomme les livrables des phases amont — les règles métier validées (phase 03) et les exigences fonctionnelles (phase 04) — et les organise en domaines, sous-domaines, agrégats et événements. Elle constitue la référence à partir de laquelle les phases aval (conception) pourront définir politiques de cohérence et mécanismes de communication inter-contextes.

---

## 2. Inventaire des documents

| Document | Rôle | Sources |
|---|---|---|
| `vue-d-ensemble-du-domaine.md` | Vue transversale du domaine : définitions, états métier et invariants de référence ; inclut la cybersécurité opérationnelle | Phases 03 et 04 |
| `domaines.md` | Treize domaines métier et leurs frontières, dont le domaine cyber interne ajouté au MVP | Vue d'ensemble |
| `sous-domaines.md` | Décomposition des treize domaines en sous-domaines | `domaines.md` |
| `core-domain.md` | Classification cœur / soutien / générique des sous-domaines au regard de la proposition de valeur | `sous-domaines.md`, proposition de valeur |
| `entites.md` | Entités, relations et invariants portés par les entités | Vue d'ensemble |
| `objets-valeur.md` | Objets de valeur rattachés aux entités | Vue d'ensemble |
| `agregats.md` | Regroupement des entités en frontières de cohérence des bounded contexts, dont BC-13 | `entites.md`, `objets-valeur.md` |
| `services-de-domaine.md` | Coordinations inter-agrégats ne pouvant appartenir à une seule frontière | `agregats.md` |
| `evenements-de-domaine.md` | Événements produits par les transitions d'état et les services, avec consommateurs | `agregats.md`, `services-de-domaine.md` |
| `regles-du-domaine.md` | Traduction formelle de chaque règle métier en mécanisme du modèle | Règles métier (phase 03), `agregats.md`, `evenements-de-domaine.md` |
| `questions-metier-ouvertes.md` | Questions métier non tranchées, référencées par les documents de la section | Phases amont |

---

## 3. Ordre de lecture recommandé

```text
vue-d-ensemble-du-domaine.md
        ↓
domaines.md → sous-domaines.md → core-domaines.md
        ↓
entites.md → objets-valeur.md
        ↓
agregats.md
        ↓
services-de-domaine.md
        ↓
evenements-de-domaine.md
        ↓
regles-du-domaine.md
```

`questions-metier-ouvertes.md` se consulte ponctuellement ; aucun document de la section ne tranche les questions qu'il recense.

---

## 4. Conventions de la section

Tous les documents de la section partagent les mêmes conventions :

- **Cohérence par référence** : aucun document ne redéfinit ce qu'un document amont a déjà établi ; il s'y réfère par identifiant ou par nom exact. Une comparaison entre documents ne doit jamais faire apparaître deux formulations concurrentes d'une même règle ou d'une même définition.
- **Langage ubiquitaire** : les noms d'agrégats, d'événements et de sous-domaines sont des termes métier au passé composé pour les événements.
- **Préfixes d'identifiants** : `DOM-XX` domaines · `SD-XX-Y` sous-domaines · `BC-XX` bounded contexts · `ENT-...` entités · `RMXX` règles métier (phase 03) · `EF-XXX` exigences fonctionnelles (phase 04).
- **Aucune décision technique** : aucun document n'impose ou ne suggère de choix d'infrastructure, de framework ou de pattern d'implémentation.
- **Statut homogène** : chaque document porte le même bloc de statut ; la validation se fait document par document, puis en cohérence d'ensemble.

---

## 5. Points de vigilance transverses

Deux constats, établis dans les documents concernés, conditionnent la validation de l'ensemble de la section :

- **Pilier interaction non couvert** (`core-domaines.md`) : la proposition de valeur établit un pilier « interaction avec les participants » qui ne correspond à aucun domaine du découpage — à trancher : hors MVP assumé, promesse au-delà du MVP, ou manquement à corriger.
- **Vente physique non modélisée** (`regles-du-domaine.md`) : quatre règles métier (RM08, RM10, RM11, RM32) partagent une dimension « point physique / agent / espèces » absente des entités, agrégats et événements ; une passe de modélisation dédiée est requise en amont.

Tant que ces deux points ne sont pas arbitrés, la section est **complete dans sa structure mais partielle dans sa couverture** — l'ordre de lecture ci-dessus n'en est pas affecté.

---

## 6. Statut de la section

| Propriété | État |
|---|---|
| Documents de structure (domaines → événements) | ✅ RÉDIGÉS |
| Traduction des règles métier | ✅ RÉDIGÉE — 4 écarts signalés |
| Arbitrage des points de vigilance | ⏳ EN ATTENTE ÉQUIPE |
| Décisions techniques | ⏳ NON PRÉJUGÉES |

Prochaine étape logique de la phase : arbitrage des deux points de vigilance, puis validation d'ensemble de la section avant ouverture de la phase de conception.