# Matrice de traçabilité — Eventix

> **Phase :** 04 — Analyse des besoins  
> **Projet :** Eventix  
> **Périmètre :** MVP — Billetterie  
> **Marché :** Cameroun  
> **Version :** 1.0  
> **Statut :** Référence de traçabilité — à valider

---

# 1. Objectif

La matrice de traçabilité constitue la **vue centrale des relations entre les artefacts du projet**.

Elle permet de répondre notamment aux questions suivantes :

- D'où vient ce besoin ?
- Quelle User Story répond à ce besoin ?
- Quelle règle métier contraint cette User Story ?
- Quel Use Case décrit son comportement ?
- Quels critères d'acceptation permettent de le vérifier ?
- Une exigence est-elle couverte ?
- Une User Story est-elle orpheline ?
- Un critère est-il rattaché à une User Story ?
- Existe-t-il une rupture de traçabilité ?

La matrice ne remplace aucun des documents sources.

Elle référence les relations entre eux.

---

# 2. Principe fondamental

La matrice repose sur le modèle général :

```text
Objectif produit
       ↓
Besoin métier
       ↓
Exigence
       ↓
User Story
       ↓
Use Case
       ↓
Critère d'acceptation
       ↓
Test
```

Des relations supplémentaires existent lorsqu'elles sont réellement justifiées :

```text
Contrainte
       ↓
contraint
       ↓
User Story

Règle métier
       ↓
contraint
       ↓
User Story

Règle métier
       ↓
influence
       ↓
Use Case

Exigence non fonctionnelle
       ↓
contraint
       ↓
User Story / Use Case
```

Cette chaîne n'est **pas obligatoire dans son intégralité**.

La matrice représente les relations réellement établies et ne crée jamais de relations artificielles.

---

# 3. Principes de traçabilité

La matrice respecte les principes suivants :

1. La matrice est la vue centrale des relations entre artefacts.
2. Une ligne représente une relation de traçabilité.
3. Les relations utilisent un vocabulaire contrôlé.
4. Une relation n'est enregistrée qu'une seule fois.
5. La direction de la relation est canonique.
6. La relation inverse peut être déduite.
7. Les artefacts possèdent des identifiants stables.
8. Les relations non justifiées ne sont pas créées.
9. Les ruptures de traçabilité sont signalées.
10. Les artefacts orphelins sont signalés.
11. Les exigences fonctionnelles et non fonctionnelles peuvent être tracées.
12. Les questions ouvertes ne deviennent pas automatiquement des décisions.
13. La matrice représente l'état courant.
14. L'historique des modifications est assuré par Git.
15. Chaque contributeur maintient les relations liées aux artefacts qu'il modifie.
16. La cohérence globale est validée collectivement.
17. Aucun détail technique n'est introduit uniquement pour compléter la matrice.
18. Le faible couplage documentaire doit être préservé.

---

# 4. Périmètre

La matrice couvre les artefacts participant au cycle de définition, justification, réalisation ou vérification du besoin.

## 4.1 Artefacts inclus

```text
Objectifs produit
Besoins métier
Contraintes
Règles métier
Exigences fonctionnelles
Exigences non fonctionnelles
User Stories
Use Cases
Critères d'acceptation
Tests
```

## 4.2 Artefacts exclus

Les documents purement organisationnels ou techniques ne sont pas intégrés automatiquement :

```text
README
Guide Git
Guide de contribution
Notes internes
Décisions techniques
Documentation d'implémentation
Configuration infrastructure
Code source
```

Ils pourront disposer de leurs propres mécanismes de traçabilité lorsque cela sera nécessaire.

---

# 5. Convention des identifiants

Chaque artefact possède un identifiant global et stable.

| Type | Convention | Exemple |
|---|---|---|
| Objectif | `OBJ-XXX` | `OBJ-001` |
| Besoin métier | `BM-XXX` | `BM-001` |
| Contrainte | `C-XXX` | `C-001` |
| Règle métier | `RM-XXX` | `RM-001` |
| Exigence fonctionnelle | `REQ-F-XXX` | `REQ-F-001` |
| Exigence non fonctionnelle | `REQ-NF-XXX` | `REQ-NF-001` |
| User Story | `US-XXX` | `US-001` |
| Use Case | `UC-XXX` | `UC-001` |
| Critère d'acceptation | `AC-XXX` | `AC-001` |
| Test | `TEST-XXX` | `TEST-001` |

Les identifiants ne dépendent pas du titre ou du chemin du fichier.

Ils restent stables lorsque l'organisation documentaire évolue.

---

# 6. Vocabulaire contrôlé

La matrice utilise un nombre limité de relations normalisées.

| Relation | Signification |
|---|---|
| `est décliné en` | Un objectif donne lieu à un besoin |
| `est couvert par` | Un artefact répond à un besoin |
| `contraint` | Une contrainte impose une limite |
| `influence` | Une règle influence un comportement |
| `dérive de` | Un artefact est issu d'un autre |
| `est détaillé par` | Un artefact est développé par un artefact plus détaillé |
| `est réalisé par` | Un comportement est réalisé par un autre artefact |
| `est vérifié par` | Un comportement est vérifié par un critère ou un test |
| `est couvert par` | Une capacité est couverte par un Use Case ou une User Story |
| `est associé à` | Relation documentaire justifiée lorsqu'aucune relation plus précise n'est adaptée |

Les relations doivent utiliser le terme le plus précis possible.

`est associé à` ne doit pas remplacer une relation plus sémantique.

---

# 7. Direction canonique

Une relation est enregistrée une seule fois.

Exemple :

```text
BM-006
   │
   └── est couvert par ──→ US-004
```

On ne stocke pas simultanément :

```text
BM-006 → US-004
US-004 → BM-006
```

La relation inverse est déduite lors d'une consultation.

---

# 8. Structure d'une relation

Chaque relation possède au minimum :

| Champ | Description |
|---|---|
| `ID` | Identifiant de la relation |
| `Source` | Artefact source |
| `Relation` | Type de relation |
| `Cible` | Artefact cible |
| `Statut` | État de la relation |
| `Justification` | Pourquoi la relation existe |
| `Source documentaire` | Document permettant de la vérifier |

Convention :

```text
TR-001
TR-002
TR-003
...
```

---

# 9. Statuts de relation

Les relations utilisent les statuts suivants :

| Statut | Signification |
|---|---|
| `VALIDÉE` | Relation confirmée |
| `À VALIDER` | Relation identifiée mais non encore validée |
| `À PRÉCISER` | Information métier insuffisante |
| `OBSERVÉE` | Relation présente dans une source mais pas encore consolidée |
| `OBSOLETE` | Relation supprimée de l'état courant |

Dans la matrice active, les relations `OBSOLETE` ne sont pas conservées.

L'historique reste disponible dans Git.

---

# 10. Registre des artefacts

Le registre indique quels artefacts sont actuellement connus de la matrice.

## 10.1 Besoins métier

Les besoins métier utilisent les identifiants `BXX` dans les documents actuels.

Pour la matrice, ils sont normalisés sous la forme :

```text
BM-001
BM-002
...
```

La correspondance avec les identifiants métier existants doit être conservée sans modifier les identifiants sources.

Exemple :

| Identifiant source | Identifiant matrice | Statut |
|---|---|---|
| `B01` | `BM-001` | VALIDÉE |
| `B02` | `BM-002` | VALIDÉE |
| `B03` | `BM-003` | VALIDÉE |
| `B04` | `BM-004` | VALIDÉE |
| `B05` | `BM-005` | VALIDÉE |
| `B06` | `BM-006` | VALIDÉE |
| `B07` | `BM-007` | VALIDÉE |
| `B08` | `BM-008` | VALIDÉE |
| `B14` | `BM-014` | VALIDÉE |
| `B16` | `BM-016` | VALIDÉE |
| `B17` | `BM-017` | VALIDÉE |
| `B18` | `BM-018` | VALIDÉE |
| `B19` | `BM-019` | VALIDÉE |
| `B20` | `BM-020` | VALIDÉE |
| `B21` | `BM-021` | VALIDÉE |
| `B22` | `BM-022` | VALIDÉE |
| `B23` | `BM-023` | VALIDÉE |
| `B24` | `BM-024` | VALIDÉE |
| `B25` | `BM-025` | VALIDÉE |
| `B26` | `BM-026` | VALIDÉE |
| `B27` | `BM-027` | VALIDÉE |
| `B28` | `BM-028` | VALIDÉE |
| `B29` | `BM-029` | VALIDÉE |
| `B30` | `BM-030` | VALIDÉE |
| `B31` | `BM-031` | VALIDÉE |
| `B32` | `BM-032` | VALIDÉE |
| `B33` | `BM-033` | VALIDÉE |

Les besoins non explicitement présents dans les artefacts actuels ne doivent pas être inventés.

---

# 11. User Stories couvertes

Les User Stories actuelles utilisent la convention stable `US-XXX`.

Le registre principal comprend :

```text
US-001 → US-040
```

avec les exceptions suivantes :

```text
US-009 → À PRÉCISER
US-021 → À PRÉCISER
US-024 → conditions financières à préciser
US-039 → statut MVP à préciser
US-040 → statut MVP à préciser
```

Les sources indiquent notamment que US-039 et US-040 restent à préciser concernant le périmètre de la vente physique. 

Les User Stories hors MVP ne doivent pas être artificiellement transformées en capacités MVP.

---

# 12. Use Cases couverts

Les Use Cases utilisent la convention :

```text
UC-001
UC-002
UC-003
...
```

Le modèle actuel couvre notamment :

| Use Case | User Stories principales |
|---|---|
| `UC-001` | `US-001`, `US-002` |
| `UC-002` | `US-003` |
| `UC-003` | `US-004`, `US-005`, `US-006`, `US-007` |
| `UC-004` | `US-007` |
| `UC-005` | `US-011`, `US-012`, `US-016` |
| `UC-006` | `US-013` |
| `UC-007` | `US-014`, `US-015` |
| `UC-008` | `US-017` |
| `UC-009` | `US-019` |
| `UC-010` | `US-020`, `US-021` |
| `UC-011` | `US-022` |
| `UC-012` | `US-023` |
| `UC-013` | `US-024` |
| `UC-014` | `US-025`, `US-026`, `US-027`, `US-028` |
| `UC-015` | `US-029`, `US-030` |
| `UC-016` | `US-031` |
| `UC-017` | `US-010`, `US-032`, `US-033` |
| `UC-018` | `US-034` |
| `UC-019` | `US-035` |
| `UC-020` | `US-036` |
| `UC-021` | `US-037` |
| `UC-022` | `US-038` |
| `UC-023` | `US-038` |
| `UC-024` | `US-005` |
| `UC-025` | `US-007` |
| `UC-026` | `US-008`, `US-025` |

Cette relation n'est pas nécessairement 1:1.

Une User Story peut être couverte par plusieurs Use Cases et un Use Case peut couvrir plusieurs User Stories.

---

# 13. Relations Besoins métier → User Stories

Les relations actuellement établies sont :

| Source | Relation | Cible |
|---|---|---|
| `B05` | est couvert par | `US-001` |
| `B05` | est couvert par | `US-002` |
| `B06` | est couvert par | `US-003` |
| `B06` | est couvert par | `US-004` |
| `B14` | est couvert par | `US-005` |
| `B06/B16` | est couvert par | `US-006` |
| `B07` | est couvert par | `US-007` |
| `B21/B22` | est couvert par | `US-008` |
| `B27` | est couvert par | `US-009` |
| `B30` | est couvert par | `US-010` |
| `B01` | est couvert par | `US-011` |
| `B02` | est couvert par | `US-012` |
| `B03` | est couvert par | `US-013` |
| `B04` | est couvert par | `US-014` |
| `B04` | est couvert par | `US-015` |
| `B08` | est couvert par | `US-016` |
| `B20` | est couvert par | `US-017` |
| `B25` | est couvert par | `US-018` |
| `B26` | est couvert par | `US-019` |
| `B31` | est couvert par | `US-020` |
| `B32` | est couvert par | `US-021` |
| `B19` | est couvert par | `US-022` |
| `B18` | est couvert par | `US-023` |
| `B33` | est couvert par | `US-024` |
| `B21` | est couvert par | `US-025` |
| `B22` | est couvert par | `US-026` |
| `B23` | est couvert par | `US-027` |
| `B24` | est couvert par | `US-028` |
| — | est couvert par | `US-029` |
| — | est couvert par | `US-030` |
| `B28` | est couvert par | `US-031` |
| `B29` | est couvert par | `US-032` |
| `B30` | est couvert par | `US-033` |
| `B29` | est couvert par | `US-034` |
| `B29/B30` | est couvert par | `US-035` |
| `B17` | est couvert par | `US-036` |
| `B16` | est couvert par | `US-037` |
| `B33` | est couvert par | `US-038` |
| `B27` | est couvert par | `US-009` |
| — | à préciser | `US-039` |
| — | à préciser | `US-040` |

Les relations non explicitement établies dans les sources restent ouvertes.

---

# 14. Relations User Stories → Use Cases

| Source | Relation | Cible |
|---|---|---|
| `US-001` | est réalisé par | `UC-001` |
| `US-002` | est réalisé par | `UC-001` |
| `US-003` | est réalisé par | `UC-002` |
| `US-004` | est réalisé par | `UC-003` |
| `US-005` | est réalisé par | `UC-003` |
| `US-005` | est réalisé par | `UC-024` |
| `US-006` | est réalisé par | `UC-003` |
| `US-007` | est réalisé par | `UC-003` |
| `US-007` | est réalisé par | `UC-004` |
| `US-007` | est réalisé par | `UC-025` |
| `US-008` | est réalisé par | `UC-026` |
| `US-008` | est réalisé par | `UC-014` |
| `US-010` | est réalisé par | `UC-017` |
| `US-011` | est réalisé par | `UC-005` |
| `US-012` | est réalisé par | `UC-005` |
| `US-013` | est réalisé par | `UC-006` |
| `US-014` | est réalisé par | `UC-007` |
| `US-015` | est réalisé par | `UC-007` |
| `US-016` | est réalisé par | `UC-005` |
| `US-017` | est réalisé par | `UC-008` |
| `US-019` | est réalisé par | `UC-009` |
| `US-020` | est réalisé par | `UC-010` |
| `US-021` | est réalisé par | `UC-010` |
| `US-022` | est réalisé par | `UC-011` |
| `US-023` | est réalisé par | `UC-012` |
| `US-024` | est réalisé par | `UC-013` |
| `US-025` | est réalisé par | `UC-014` |
| `US-026` | est réalisé par | `UC-014` |
| `US-027` | est réalisé par | `UC-014` |
| `US-028` | est réalisé par | `UC-014` |
| `US-029` | est réalisé par | `UC-015` |
| `US-030` | est réalisé par | `UC-015` |
| `US-031` | est réalisé par | `UC-016` |
| `US-032` | est réalisé par | `UC-017` |
| `US-033` | est réalisé par | `UC-017` |
| `US-034` | est réalisé par | `UC-018` |
| `US-035` | est réalisé par | `UC-019` |
| `US-036` | est réalisé par | `UC-020` |
| `US-037` | est réalisé par | `UC-021` |
| `US-038` | est réalisé par | `UC-022` |
| `US-038` | est réalisé par | `UC-023` |

Les relations concernant les User Stories encore `À PRÉCISER` ne doivent pas être interprétées comme une validation définitive de leur périmètre.

---

# 15. Relations Use Cases → Besoins métier

| Source | Relation | Cible |
|---|---|---|
| `UC-001` | couvre | `B05` |
| `UC-002` | couvre | `B06` |
| `UC-003` | couvre | `B06` |
| `UC-003` | couvre | `B14` |
| `UC-003` | couvre | `B15` |
| `UC-003` | couvre | `B16` |
| `UC-004` | couvre | `B07` |
| `UC-005` | couvre | `B01` |
| `UC-005` | couvre | `B02` |
| `UC-005` | couvre | `B08` |
| `UC-006` | couvre | `B03` |
| `UC-007` | couvre | `B04` |
| `UC-008` | couvre | `B20` |
| `UC-009` | couvre | `B26` |
| `UC-010` | couvre | `B31` |
| `UC-010` | couvre | `B32` |
| `UC-011` | couvre | `B19` |
| `UC-012` | couvre | `B18` |
| `UC-013` | couvre | `B33` |
| `UC-014` | couvre | `B21` |
| `UC-014` | couvre | `B22` |
| `UC-014` | couvre | `B23` |
| `UC-014` | couvre | `B24` |
| `UC-016` | couvre | `B28` |
| `UC-017` | couvre | `B29` |
| `UC-017` | couvre | `B30` |
| `UC-018` | couvre | `B29` |
| `UC-019` | couvre | `B29` |
| `UC-019` | couvre | `B30` |
| `UC-020` | couvre | `B17` |
| `UC-021` | couvre | `B16` |
| `UC-022` | couvre | `B33` |

---

# 16. Relations User Stories → Règles métier

Les règles métier ne sont pas des User Stories.

Elles contraignent les comportements exprimés par certaines User Stories.

## 16.1 Réservation et paiement

```text
RM01 → US-004
RM01 → US-005

RM02 → US-005
RM02 → US-037

RM03 → US-004
RM03 → US-005
RM03 → US-006
RM03 → US-037

RM04 → US-037

RM05 → US-004
RM05 → US-006
RM05 → US-037

RM06 → US-003
RM06 → US-004
RM06 → US-016
RM06 → US-025

RM07 → US-004
RM07 → US-006
RM07 → US-017
```

## 16.2 Billets et contrôle

```text
RM13 → US-008
RM13 → US-025
RM13 → US-026

RM14 → US-008
RM14 → US-025
RM14 → US-026
RM14 → US-029
RM14 → US-030

RM15 → US-028

RM16 → US-027

RM17 → US-008
RM17 → US-018
RM17 → US-025
```

## 16.3 Remboursements

```text
RM19 → US-023
RM19 → US-036

RM20 → US-023
RM20 → US-036

RM21 → US-023
RM21 → US-036
```

## 16.4 Événements

```text
RM22 → US-022

RM23 → US-012
RM23 → US-022

RM24 → US-012
RM24 → US-016

RM25 → US-004
RM25 → US-006
RM25 → US-007
```

## 16.5 Confiance et sécurité

```text
RM28 → US-013
RM28 → US-031

RM29 → US-032
RM29 → US-033
RM29 → US-034

RM30 → US-032
RM30 → US-033
RM30 → US-034
RM30 → US-035

RM31 → US-019
RM31 → US-020
RM31 → US-035
RM31 → US-038

RM32 → US-019
RM32 → US-020

RM33 → US-020
```

Les règles relatives aux ventes physiques restent liées aux User Stories `US-039` et `US-040`, mais ces User Stories demeurent `À PRÉCISER` pour le périmètre MVP.

---

# 17. Relations Use Cases → Règles métier

Les Use Cases référencent les règles qui influencent directement leur comportement.

Exemples consolidés :

| Use Case | Règles |
|---|---|
| `UC-002` | `RM06`, `RM12` |
| `UC-003` | `RM01`, `RM02`, `RM03`, `RM05`, `RM06`, `RM07`, `RM25` |
| `UC-005` | `RM23`, `RM24` |
| `UC-006` | `RM28` |
| `UC-008` | `RM07` |
| `UC-009` | `RM31`, `RM32` |
| `UC-010` | `RM31`, `RM32`, `RM33` |
| `UC-011` | `RM22`, `RM23` |
| `UC-012` | `RM19`, `RM20`, `RM21` |
| `UC-014` | `RM13`, `RM14`, `RM15`, `RM16`, `RM17` |
| `UC-015` | `RM14` |
| `UC-016` | `RM28` |
| `UC-017` | `RM29`, `RM30` |
| `UC-018` | `RM29`, `RM30` |
| `UC-019` | `RM30`, `RM31` |
| `UC-020` | `RM19`, `RM20`, `RM21` |
| `UC-021` | `RM02`, `RM03`, `RM04`, `RM05`, `RM06` |
| `UC-024` | `RM01`, `RM02`, `RM03` |
| `UC-025` | `RM03`, `RM05`, `RM06`, `RM25` |
| `UC-026` | `RM13`, `RM14`, `RM16`, `RM17` |

---

# 18. Relations User Stories → Critères d'acceptation

Les critères d'acceptation utilisent la convention `AC-XXX`.

La relation canonique est :

```text
US
 ↓
est vérifiée par
 ↓
AC
```

La couverture actuelle est :

| User Story | Critères |
|---|---|
| `US-001` | `AC-001 → AC-002` |
| `US-002` | `AC-003` |
| `US-003` | `AC-004 → AC-006` |
| `US-004` | `AC-007 → AC-009` |
| `US-005` | `AC-010 → AC-012` |
| `US-006` | `AC-013 → AC-015` |
| `US-007` | `AC-016 → AC-018` |
| `US-008` | `AC-019 → AC-021` |
| `US-009` | À préciser |
| `US-010` | `AC-023 → AC-024` |
| `US-011` | `AC-025` |
| `US-012` | `AC-026 → AC-028` |
| `US-013` | `AC-029 → AC-030` |
| `US-014` | `AC-031` |
| `US-015` | `AC-032 → AC-033` |
| `US-016` | `AC-034 → AC-035` |
| `US-017` | `AC-036 → AC-038` |
| `US-018` | `AC-039 → AC-040` |
| `US-019` | `AC-041 → AC-042` |
| `US-020` | `AC-043 → AC-045` |
| `US-021` | À préciser |
| `US-022` | `AC-047 → AC-049` |
| `US-023` | `AC-050 → AC-053` |
| `US-024` | `AC-054 → AC-056` |
| `US-025` | `AC-057 → AC-060` |
| `US-026` | `AC-061 → AC-062` |
| `US-027` | `AC-063` |
| `US-028` | `AC-064 → AC-065` |
| `US-029` | `AC-066 → AC-067` |
| `US-030` | `AC-068 → AC-069` |
| `US-031` | `AC-070 → AC-071` |
| `US-032` | `AC-072 → AC-073` |
| `US-033` | `AC-074 → AC-076` |
| `US-034` | `AC-077 → AC-078` |
| `US-035` | `AC-079 → AC-080` |
| `US-036` | `AC-081 → AC-083` |
| `US-037` | `AC-084 → AC-086` |
| `US-038` | `AC-087 → AC-090` |
| `US-039` | À préciser |
| `US-040` | À préciser |

---

# 19. Relations Use Cases → Critères d'acceptation

La relation suit :

```text
Use Case
    ↓
comportement détaillé
    ↓
Critère d'acceptation
```

Cette relation doit être utilisée lorsque le critère vérifie directement un comportement décrit par le Use Case.

Exemples :

```text
UC-002 → AC-004
UC-002 → AC-005
UC-002 → AC-006

UC-003 → AC-007
UC-003 → AC-008
UC-003 → AC-009
UC-003 → AC-010
UC-003 → AC-012

UC-004 → AC-016
UC-004 → AC-017
UC-004 → AC-018

UC-006 → AC-029
UC-006 → AC-030

UC-008 → AC-036
UC-008 → AC-037
UC-008 → AC-038

UC-011 → AC-047
UC-011 → AC-048
UC-011 → AC-049

UC-012 → AC-050
UC-012 → AC-051
UC-012 → AC-052
UC-012 → AC-053

UC-014 → AC-057
UC-014 → AC-058
UC-014 → AC-059
UC-014 → AC-060
UC-014 → AC-061
UC-014 → AC-063
UC-014 → AC-064
UC-014 → AC-065

UC-015 → AC-066
UC-015 → AC-067
UC-015 → AC-068
UC-015 → AC-069

UC-017 → AC-074
UC-017 → AC-075
UC-017 → AC-076

UC-018 → AC-077
UC-018 → AC-078

UC-019 → AC-079
UC-019 → AC-080

UC-020 → AC-081
UC-020 → AC-082
UC-020 → AC-083

UC-021 → AC-084
UC-021 → AC-085
UC-021 → AC-086

UC-022 → AC-087
UC-022 → AC-088
UC-022 → AC-089
UC-022 → AC-090
```

Cette liste constitue une consolidation de traçabilité et ne remplace pas le contenu des Use Cases ou des critères.

---

# 20. Exigences fonctionnelles

Les User Stories et Use Cases indiquent actuellement que les identifiants exacts des exigences fonctionnelles doivent être consolidés dans la matrice.

La matrice **ne doit donc pas inventer** de `REQ-F-XXX`.

Statut actuel :

```text
REQ-F
  ↓
Identifiants exacts à consolider
  ↓
À VALIDER
```

Une fois les identifiants définitifs disponibles, les relations seront ajoutées selon le modèle :

```text
BM-XXX
   ↓
REQ-F-XXX
   ↓
US-XXX
   ↓
UC-XXX
   ↓
AC-XXX
```

Une User Story peut être liée à plusieurs exigences fonctionnelles.

Une exigence fonctionnelle peut également être partagée par plusieurs User Stories lorsqu'une telle relation est justifiée.

---

# 21. Exigences non fonctionnelles

Les exigences non fonctionnelles sont intégrées à la matrice.

Elles ne sont pas incorporées directement dans les User Stories.

Elles peuvent être reliées lorsqu'elles influencent réellement :

- une User Story ;
- un Use Case ;
- un critère de vérification ;
- éventuellement un test.

Exemple conceptuel :

```text
REQ-NF-PERF-001
        ↓
     contraint
        ↓
US-001
```

ou :

```text
REQ-NF-SEC-001
        ↓
     contraint
        ↓
US-025
```

Les identifiants exacts des exigences non fonctionnelles doivent être repris du document source lorsqu'ils sont validés.

Aucun identifiant n'est inventé ici.

---

# 22. Tests

Les tests ne sont pas encore utilisés comme identifiants normatifs dans la matrice actuelle.

Une fois leur registre défini, la relation sera :

```text
AC-XXX
   ↓
est vérifié par
   ↓
TEST-XXX
```

Exemple :

```text
AC-061
   ↓
TEST-XXX
```

La matrice permettra alors de détecter :

```text
Critère sans test
        ↓
INCOMPLET
```

ou :

```text
Test sans critère
        ↓
ORPHELIN
```

Aucun `TEST-XXX` ne doit être inventé avant la définition du registre de tests.

---

# 23. Couverture

La couverture mesure si les artefacts disposent des relations attendues.

## 23.1 Besoin couvert

```text
Besoin métier
      ↓
User Story ou autre artefact justifié
```

Statut :

```text
COUVERT
```

si une relation pertinente existe.

---

## 23.2 User Story couverte

```text
User Story
    ↓
Use Case
    ↓
Critère d'acceptation
```

Une User Story est considérée comme correctement couverte lorsque les relations pertinentes existent.

---

## 23.3 Critère couvert

```text
Critère
   ↓
User Story
```

Puis, ultérieurement :

```text
Critère
   ↓
Test
```

---

## 23.4 Exigence couverte

```text
Exigence
    ↓
User Story / Use Case
```

Une exigence sans élément de réalisation ou de vérification doit être signalée.

---

# 24. Ruptures de traçabilité

La matrice doit explicitement signaler les ruptures.

## 24.1 Artefact orphelin

```text
Artefact
   ↓
aucune relation pertinente
```

Statut :

```text
ORPHELIN
```

---

## 24.2 Besoin non couvert

```text
BM-XXX
   ↓
aucune User Story
```

Statut :

```text
NON COUVERT
```

---

## 24.3 User Story incomplète

```text
US-XXX
   ↓
aucun Use Case
```

ou :

```text
US-XXX
   ↓
aucun AC
```

Statut :

```text
INCOMPLET
```

---

## 24.4 Critère orphelin

```text
AC-XXX
   ↓
aucune User Story
```

Statut :

```text
ORPHELIN
```

---

## 24.5 Exigence non couverte

```text
REQ-F-XXX
   ↓
aucune User Story
```

Statut :

```text
NON COUVERT
```

---

# 25. Points volontairement ouverts

La matrice ne doit pas transformer les décisions ouvertes en décisions définitives.

Les éléments suivants restent notamment ouverts :

| Élément | Sujet | Statut |
|---|---|---|
| `US-009` | Conditions de transfert | À préciser |
| `US-021` | Niveau des recommandations | À préciser |
| `US-024` | Conditions détaillées du règlement | À préciser |
| `US-039` | Vente physique dans le MVP | À préciser |
| `US-040` | Acteurs et contexte de vente physique | À préciser |
| `REQ-F-*` | Identifiants exacts des EF | À consolider |
| `REQ-NF-*` | Identifiants exacts des ENF | À consolider |
| `TEST-*` | Registre des tests | À définir |

---

# 26. Vente physique

Les User Stories `US-039` et `US-040` apparaissent dans les documents actuels mais leur statut MVP reste à préciser.

La matrice ne doit donc pas les considérer comme des capacités définitivement validées du MVP.

```text
US-039
   ↓
À PRÉCISER

US-040
   ↓
À PRÉCISER
```

Les règles métier associées peuvent continuer à exister dans la documentation métier sans que cela constitue une validation de leur intégration au MVP.

---

# 27. Hors périmètre MVP

Les éléments suivants ne doivent pas être transformés en relations MVP :

```text
Marketplace
Quiz Live
QR Cloud photos / vidéos
Reels
Autres services événementiels
Mode offline avancé
Synchronisation distribuée avancée
```

Ils pourront être ajoutés ultérieurement lorsqu'ils entreront officiellement dans le périmètre produit.

---

# 28. Contrôles de cohérence

La matrice doit permettre d'effectuer au minimum les contrôles suivants.

## Contrôle COV-001 — Besoin couvert

```text
Chaque besoin MVP
    ↓
doit avoir une relation pertinente
```

---

## Contrôle COV-002 — User Story justifiée

```text
Chaque User Story MVP
    ↓
doit être reliée à une source métier
```

---

## Contrôle COV-003 — User Story détaillée

```text
Chaque User Story applicable
    ↓
Use Case
```

---

## Contrôle COV-004 — User Story vérifiable

```text
Chaque User Story applicable
    ↓
Critère d'acceptation
```

---

## Contrôle COV-005 — Critère rattaché

```text
Chaque AC
    ↓
User Story
```

---

## Contrôle COV-006 — Règle utilisée

Une règle métier peut être reliée à plusieurs artefacts.

Elle ne doit pas être reliée artificiellement à des capacités qui ne dépendent pas d'elle.

---

## Contrôle COV-007 — Exigence couverte

```text
REQ-F / REQ-NF
       ↓
relation pertinente
```

---

## Contrôle COV-008 — Absence d'artefact orphelin

Tout artefact appartenant au périmètre de traçabilité doit avoir au moins une relation pertinente, sauf lorsqu'il est explicitement marqué comme ouvert ou en cours de consolidation.

---

# 29. Règle contre les relations artificielles

La matrice ne doit jamais être complétée uniquement pour obtenir :

```text
100 % de couverture
```

Une relation doit répondre à la question :

> **Pourquoi cette relation existe-t-elle réellement ?**

Si aucune justification n'existe :

```text
Pas de relation
```

et non :

```text
Relation inventée
```

---

# 30. Faible couplage documentaire

Chaque artefact conserve sa responsabilité.

```text
Besoins métier
    ↓
Pourquoi le métier a besoin de quelque chose

Contraintes
    ↓
Quelles limites doivent être respectées

Règles métier
    ↓
Quels invariants et comportements métier

Exigences fonctionnelles
    ↓
Ce que le système doit permettre

Exigences non fonctionnelles
    ↓
Quelles qualités et contraintes

User Stories
    ↓
Quelle valeur pour l'acteur

Use Cases
    ↓
Comment le comportement métier se déroule

Critères d'acceptation
    ↓
Comment vérifier le comportement

Tests
    ↓
Comment vérifier concrètement

Matrice
    ↓
Comment tous ces artefacts sont reliés
```

La matrice ne doit donc jamais devenir le conteneur du contenu des autres documents.

---

# 31. Une information, une source canonique

La matrice référence les artefacts.

Elle ne recopie pas leur contenu.

Exemple :

```text
RM02
```

est défini dans :

```text
regles-metier.md
```

La matrice indique seulement :

```text
RM02
   ↓
contraint
   ↓
US-005
```

De même :

```text
US-004
```

est défini dans :

```text
user-stories.md
```

La matrice ne recopie pas la User Story.

---

# 32. Maintenance

Lorsqu'un artefact est modifié :

1. identifier son ID ;
2. rechercher toutes ses relations ;
3. vérifier les relations impactées ;
4. ajouter ou supprimer uniquement les relations justifiées ;
5. vérifier les ruptures de couverture ;
6. mettre à jour le statut si nécessaire ;
7. faire valider le changement selon le processus d'équipe.

---

# 33. Gestion des changements

La matrice représente uniquement l'état courant.

Exemple :

```text
État actuel

BM-004
   ↓
US-012
```

Si une décision ultérieure modifie la relation :

```text
BM-004
   ↓
US-018
```

la matrice active contient uniquement :

```text
BM-004
   ↓
US-018
```

L'ancien état reste récupérable grâce à Git.

La matrice n'est donc pas un journal historique.

---

# 34. Responsabilité

La responsabilité de la traçabilité est distribuée.

```text
Contributeur
     ↓
modifie un artefact
     ↓
met à jour ses relations
     ↓
Revue
     ↓
Validation collective
```

Aucun rôle unique ne possède à lui seul toute la traçabilité.

La traçabilité étant transversale, elle concerne :

- le produit ;
- le métier ;
- l'analyse ;
- la conception ;
- l'architecture ;
- la validation.

---

# 35. Vue synthétique du graphe

La vue conceptuelle actuelle est :

```text
                 OBJECTIFS
                     │
                     ▼
              BESOINS MÉTIER
               │           │
               │           ▼
               │       CONTRAINTES
               │
               ▼
             RÈGLES
               │
               ▼
          EXIGENCES
          │        │
          │        └──────────→ ENF
          ▼
       USER STORIES
          │   │
          │   ├──────────────→ RÈGLES
          │
          ├──────────────→ USE CASES
          │
          └──────────────→ CRITÈRES
                              │
                              ▼
                            TESTS
```

Cette représentation est conceptuelle.

La matrice réelle est constituée de relations individuelles.

---

# 36. Exemple complet

Exemple sur l'achat d'un billet :

```text
B06
 │
 └── est couvert par ──→ US-004
                              │
                              ├── est réalisé par ──→ UC-003
                              │
                              ├── contraint par ←── RM01
                              ├── contraint par ←── RM03
                              ├── contraint par ←── RM05
                              ├── contraint par ←── RM06
                              ├── contraint par ←── RM07
                              │
                              └── est vérifiée par
                                      │
                                      ├── AC-007
                                      ├── AC-008
                                      └── AC-009
```

Une NFR pourra ultérieurement être ajoutée sans modifier cette structure :

```text
REQ-NF-XXX
     │
     └── contraint ──→ US-004
```

Et un test pourra ensuite compléter la chaîne :

```text
AC-007
   │
   └── est vérifié par ──→ TEST-XXX
```

---

# 37. État actuel de la traçabilité

## Couverture disponible

Les relations suivantes sont déjà suffisamment définies pour être exploitées :

```text
Besoins métier
      ↓
User Stories
      ↓
Use Cases
      ↓
Règles métier
      ↓
Critères d'acceptation
```

## Couverture à consolider

```text
Objectifs
      ↓
Besoins métier
```

Les identifiants exacts doivent être vérifiés dans le document source.

```text
Besoins métier
      ↓
Exigences fonctionnelles
```

Les identifiants exacts des exigences fonctionnelles doivent être consolidés.

```text
Exigences non fonctionnelles
      ↓
User Stories / Use Cases
```

Les relations pertinentes doivent être consolidées à partir du document des exigences non fonctionnelles.

```text
Critères
      ↓
Tests
```

Le registre des tests doit encore être défini.

---

# 38. Règle de validation finale

La matrice ne sera considérée comme complètement consolidée que lorsque :

```text
Objectifs
   ↓
Besoins
   ↓
Exigences
   ↓
User Stories
   ↓
Use Cases
   ↓
Critères
   ↓
Tests
```

pourra être parcouru sans rupture injustifiée pour les éléments du périmètre validé.

Les relations ouvertes doivent cependant rester explicitement marquées comme telles.

---

# 39. Sources de référence

La matrice s'appuie notamment sur :

```text
02-vision-produit/objectifs-produit.md

03-decouverte-du-metier/besoins-metier.md
03-decouverte-du-metier/contraintes-metier.md
03-decouverte-du-metier/regles-metier.md
03-decouverte-du-metier/processus-metier.md
03-decouverte-du-metier/user-journeys.md
03-decouverte-du-metier/questions-metier-ouvertes.md

04-analyse-des-besoins/exigences-fonctionnelles.md
04-analyse-des-besoins/exigences-non-fonctionnelles.md
04-analyse-des-besoins/user-stories.md
04-analyse-des-besoins/use-cases.md
04-analyse-des-besoins/criteres-d-acceptation.md
```

---

# 40. Statut du document

**Document :** `matrice-de-tracabilite.md`  
**Version :** 1.0  
**Statut :** Référence de traçabilité — à valider  
**Périmètre :** MVP Eventix — Billetterie  
**Marché :** Cameroun  
**Méthode :** Traçabilité relationnelle  
**Identifiants :** IDs globaux et stables  
**Relations :** vocabulaire contrôlé  
**Historique :** Git  
**Principe transversal :** Faible couplage

---

# 41. Principe directeur

> **La matrice ne crée pas la vérité documentaire : elle rend visible la relation entre les vérités documentaires déjà établies.**

Elle doit donc privilégier :

```text
Exactitude
    >
Exhaustivité artificielle
```

et :

```text
Relation justifiée
    >
Relation supposée
```

Enfin :

```text
État courant
    =
Matrice

Historique
    =
Git
```

La matrice constitue ainsi le **graphe de traçabilité du système de besoins Eventix**, sans devenir le propriétaire du contenu des artefacts qu'elle relie.




https://github.com/SamuHabra/eventix-documentation/tree/2693bf7c1be5e8b7145f3c9df669dcd3ef30b07a/docs