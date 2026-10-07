# Service de supervision cybersécurité Eventix

| Propriété | Valeur |
|---|---|
| **Nom de travail** | Service de supervision cybersécurité Eventix |
| **Bénéficiaires** | Analyste cybersécurité, responsable humain habilité |
| **Périmètre** | Système Eventix et actifs explicitement autorisés |
| **Cible produit** | MVP |
| **Décision de réponse** | Humaine ; aucune réponse de confinement automatique dans le MVP |
| **Statut** | Cadrage fonctionnel ; sources, responsabilités et mesures techniques à valider |

## 1. Mission

Le service aide les équipes internes à repérer et analyser les signaux d'intrusion ou d'activité technique suspecte affectant Eventix. Il centralise les alertes utiles, permet d'enquêter et de qualifier les incidents, puis conserve la décision et le suivi de la réponse.

Ce service est un outil de supervision et de gestion d'incident. Il n'est ni une garantie de détection exhaustive, ni une équipe de réponse 24/7 présumée, ni une automatisation qui bloque ou sanctionne sur la seule base d'un signal.

## 2. Résultats métier attendus

- Voir qu'un signal suspect a été observé, quand il l'a été et par quel mécanisme ou source connue.
- Prioriser les alertes selon leur sévérité et leur contexte, sans présenter une sévérité automatique comme une conclusion.
- Comprendre quels actifs et activités Eventix sont potentiellement concernés.
- Relier plusieurs alertes à un dossier d'incident et consigner l'analyse humaine.
- Permettre à un responsable habilité de décider, justifier et suivre une réponse.
- Garder un historique permettant l'audit et l'amélioration des protections.

## 3. Rôles et séparation des responsabilités

### Analyste cybersécurité

Consulte les alertes auxquelles son rôle donne accès, examine les informations disponibles, ajoute des observations, qualifie le dossier et l'escalade. Il ne confirme pas automatiquement une mesure métier comme la suspension d'un organisateur.

### Responsable humain habilité

Examine les constats et décide des mesures de réponse selon son mandat. L'identité de la personne, la justification, le périmètre et le résultat doivent être enregistrés. Les rôles exacts et le principe de séparation à appliquer restent à déterminer via QMO-058.

### Administrateur de plateforme

Gère les opérations techniques qui lui sont attribuées. L'accès à ce rôle ne lui donne pas automatiquement accès à tous les contenus d'enquête ni le pouvoir de valider toutes les mesures de sécurité.

Les personnes peuvent cumuler des rôles uniquement si la gouvernance le permet et que les décisions sensibles restent contrôlables.

## 4. Cycle de vie d'une alerte

```text
Signal observé
      ↓
Règle / analyse de détection
      ↓
Alerte à examiner
      ↓
Triage humain
      ├── Faux positif / non concluant → justification et clôture ou observation
      ├── À investiguer → collecte et analyse autorisées
      └── Incident confirmé → dossier d'incident
                                  ↓
                        Décision humaine habilitée
                                  ↓
                        Mesure exécutée par le rôle autorisé
                                  ↓
                       Vérification du résultat
                                  ↓
                         Clôture et retour d'expérience
```

États de travail proposés pour le dossier : `NEW`, `TRIAGE`, `INVESTIGATING`, `CONFIRMED`, `FALSE_POSITIVE`, `NON_CONCLUSIVE`, `CONTAINMENT_DECIDED`, `RESOLVED`, `CLOSED`. Cette liste est une proposition de modélisation de processus à valider, pas un choix de stockage ni une nomenclature d'API.

## 5. Tableau de bord MVP

Le tableau de bord doit fournir une vue opérationnelle filtrable des éléments effectivement disponibles :

- alertes nouvelles, ouvertes et en cours d'analyse ;
- catégorie de signal, date/heure, sévérité estimée et état ;
- actif ou service Eventix potentiellement touché ;
- source de détection et période de couverture connue ;
- résumé des éléments de contexte autorisés ;
- dossier d'incident associé, responsable de suivi et dernière mise à jour ;
- décisions humaines, état des mesures et résultat déclaré ;
- limites ou indisponibilités des sources qui influencent la visibilité.

Les vues de synthèse peuvent présenter volumes et tendances, mais ne doivent pas masquer une source muette ou transformer « aucun signal reçu » en « aucune attaque ». Les cibles de fraîcheur, périodes de conservation, filtres, seuils et indicateurs de couverture restent à déterminer.

## 6. Sources de signaux à étudier

Les sources ci-dessous sont des catégories possibles, pas des intégrations déjà choisies :

- événements d'authentification et d'autorisation ;
- changements sensibles de compte, rôle ou configuration ;
- événements d'accès et erreurs des interfaces Eventix ;
- anomalies autour des commandes, paiements et opérations financières ;
- alertes issues d'infrastructure ou de fournisseurs explicitement intégrés ;
- incidents signalés par les équipes ou révélés lors d'évaluations autorisées.

Avant toute collecte, déterminer la nécessité, la fiabilité, la sensibilité, la provenance, la durée de conservation et les personnes autorisées à consulter chaque type de signal. Éviter de collecter des secrets, contenus de paiement ou données personnelles non nécessaires.

## 7. Relation à la sécurité métier

La supervision cyber et Trust & Safety demeurent deux responsabilités différentes :

- Une alerte cyber peut indiquer la compromission d'un compte organisateur ; elle ne confirme pas à elle seule une fraude de cet organisateur.
- Un signalement de faux événement relève du processus de confiance métier ; il ne prouve pas une intrusion technique.
- Si un incident cyber a des conséquences sur un billet, un événement, un paiement ou une organisation, le dossier cyber peut référencer le dossier métier concerné avec le minimum de données nécessaires. Chaque responsable prend les décisions de son domaine.
- Le dashboard ne fusionne pas les sévérités cyber et métier en un score unique sans règle validée.

La responsabilité existante de `MOD-10 trust-safety` couvre la confiance et les mesures métier ; `MOD-11 analytics-observability` collecte et restitue des faits selon l'architecture actuelle. Aucun de ces modules n'est désigné par défaut comme propriétaire du traitement d'incidents cyber. La frontière logicielle sera tranchée en phase 05–09 avant implémentation.

## 8. Décision de réponse et absence d'automatisation

Dans le MVP, une détection peut créer et mettre à jour une alerte ; elle ne peut pas, à elle seule :

- désactiver ou suspendre un compte ;
- suspendre un organisateur ou un événement ;
- annuler une commande, bloquer des fonds ou initier un remboursement ;
- invalider des billets ;
- arrêter un service ou changer sa configuration.

Ces effets nécessitent une décision humaine autorisée. La fiche consigne le décideur, le motif, les actifs concernés, l'action décidée, qui l'a exécutée, l'heure et le résultat. Si la réponse est urgente, le processus d'escalade et le niveau d'autorisation doivent être définis avant mise en production ; ce document n'invente pas de politique d'urgence.

## 9. Évaluations défensives et offensives

La démarche défensive doit permettre d'examiner la couverture et l'état des sources, qualifier les alertes, coordonner les corrections et réviser les règles après les incidents.

Les évaluations offensives servent à valider les protections et la détection dans un périmètre expressément autorisé. Avant un test, approuver les actifs et environnements visés, les règles d'engagement, les limites d'impact, les contacts d'arrêt, la confidentialité des résultats et le processus de correction. Ne jamais déduire une autorisation d'un objectif de test ou d'une entrée du backlog.

## 10. Traçabilité et validation

- Découverte : [B38 — Superviser la cybersécurité](../03-decouverte-du-metier/besoins-metier.md), [RM40](../03-decouverte-du-metier/regles-metier.md), QMO-053 à QMO-058.
- Exigences : [EF-145–EF-147](../04-analyse-des-besoins/exigences-fonctionnelles.md), [ENF-016–ENF-021](../04-analyse-des-besoins/exigences-non-fonctionnelles.md), critères AC-115–AC-119.
- Scénario métier : [UC-037](../04-analyse-des-besoins/use-cases.md).
- Règles d'audit détaillées : [audit.md](./audit.md).
- Processus d'incident détaillé : [gestion-des-incidents.md](./gestion-des-incidents.md).

La couverture des menaces, les sources concrètes, les seuils et délais d'analyse, les habilitations, la conservation, les mesures d'urgence et les critères de vérification doivent faire l'objet de décisions et de validations avant le lancement du service.
