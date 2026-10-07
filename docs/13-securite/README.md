# 13 — Sécurité Eventix

| Propriété | Valeur |
|---|---|
| **Phase** | 13 — Sécurité |
| **Périmètre** | Sécurité du système Eventix et service interne de supervision cybersécurité |
| **Statut** | Cadrage MVP engagé ; mesures techniques à définir et valider |

## 1. Objectif

Cette phase transforme les exigences et décisions des phases amont en politique, responsabilités, protections, mécanismes de surveillance et moyens de vérification de sécurité.

La cybersécurité doit être prise en compte dès la découverte métier et dans toutes les phases de conception. La phase 13 rassemble et approfondit les décisions de sécurité ; elle ne reporte pas à elle seule l'intégration de la sécurité aux phases 04 à 10.

Le MVP comprend un **service interne de supervision cybersécurité** : observer les signaux disponibles, détecter et analyser des activités potentiellement malveillantes, présenter des alertes et suivre les incidents. Une alerte n'est pas une preuve d'attaque. Les mesures ayant un effet sur Eventix sont décidées par un humain habilité ; leur exécution n'est pas déclenchée automatiquement à partir d'une détection.

## 2. Frontière de responsabilité

- **Cybersécurité opérationnelle** : compromission du système, activité technique suspecte, vulnérabilités, incidents, exposition ou altération de données et disponibilité d'Eventix.
- **Sécurité et confiance métier** : fraude événementielle, vérification des organisateurs/événements, signalements métier, règles de billetterie et décisions associées.
- **Zone de collaboration** : un incident cyber peut affecter une activité métier. Le service peut relier les deux dossiers avec des accès maîtrisés, sans fusionner leurs qualifications ni laisser le SOC décider d'une sanction métier.

La frontière logique est définie par BF-13 / BC-13 / MOD-13. La phase 05 en établit le modèle de domaine et la phase 07 ses responsabilités ; les contrats, sources, mécanismes de collecte, règles de conservation et mode de déploiement restent à concevoir. Le service cyber n'est pas absorbé dans `MOD-10 trust-safety` ou `MOD-11 analytics-observability`.

## 3. Documents de la phase

| Document | Responsabilité | État |
|---|---|---|
| [Supervision cybersécurité](./supervision-cybersecurite.md) | Service de supervision, tableau de bord, alertes, qualification humaine et décision de réponse | Cadré pour le MVP |
| [Modèle de menaces](./modele-de-menaces.md) | Actifs, frontières, scénarios de menace et risques | À élaborer et valider |
| [Authentification](./authentification.md) | Protection des identités système et utilisateurs | À concevoir |
| [Autorisation](./autorisation.md) | Accès et séparation des responsabilités | À concevoir |
| [Sécurité API](./securite-api.md) | Exigences de sécurité des interfaces | À concevoir |
| [Protection des données](./protection-des-donnees.md) | Classification, minimisation, conservation et exposition | À valider avec les responsables compétents |
| [Chiffrement](./chiffrement.md) | Besoins de protection cryptographique | À concevoir |
| [Gestion des secrets](./gestion-des-secrets.md) | Cycle de vie des secrets et responsabilités | À concevoir |
| [Sécurité des paiements](./securite-des-paiements.md) | Frontière Eventix/prestataire et protection du cycle financier | Cadrage des responsabilités et risques ; contrôles à valider |
| [Audit](./audit.md) | Enregistrements de sécurité et éléments de preuve | À concevoir |
| [Gestion des incidents](./gestion-des-incidents.md) | Triage, escalade, décision, communication et retour d'expérience | À élaborer |

## 4. Principes directeurs

1. Les alertes sont des signaux à qualifier ; elles ne constituent pas automatiquement la preuve d'une intrusion.
2. Toute mesure impactant comptes, événements, opérations financières ou services doit être décidée par une personne habilitée et être traçable.
3. Le MVP n'applique pas automatiquement de confinement, suspension ou sanction à la suite d'une alerte.
4. Les tableaux de bord exposent uniquement les informations nécessaires aux rôles autorisés.
5. La couverture de détection, les limites et les périodes sans signal doivent être visibles ; absence d'alerte ne signifie pas absence d'attaque.
6. Les évaluations offensives sont limitées aux systèmes et environnements expressément autorisés, avec règles d'engagement approuvées.
7. Les outils, fournisseurs, niveaux de service, seuils et mesures techniques ne sont pas réputés choisis par ce cadrage.

## 5. Dépendances et décisions

- Enjeux et actifs découverts en [phase 03](../03-decouverte-du-metier/cybersecurite-eventix/README.md).
- Besoin et règle : [B38](../03-decouverte-du-metier/besoins-metier.md), RM40 dans [les règles métier](../03-decouverte-du-metier/regles-metier.md).
- Exigences MVP : EF-145 à EF-147, AC-115 à AC-119.
- Questions à résoudre : QMO-053 à QMO-058 dans [les questions métier ouvertes](../03-decouverte-du-metier/questions-metier-ouvertes.md).
- Impacts à réconcilier : propriété des capacités, sources d'événements, droits du tableau de bord, conservation, réponse aux incidents, tests autorisés et frontière du prestataire de paiement.

## 6. Critères de sortie de phase

La phase sécurité ne sera considérée prête pour la mise en production que lorsque les responsabilités et décisions nécessaires sont validées, les mesures convenues sont implémentées, leur efficacité est vérifiée et les constats prioritaires sont traités ou explicitement acceptés par une autorité compétente. La présence de documents ou d'un tableau de bord seul ne prouve pas qu'Eventix est sécurisé.
