# Audit de sécurité et traçabilité des incidents

## 1. Objectif

Définir comment les alertes, investigations et décisions de cybersécurité peuvent être reconstituées et vérifiées, sans confondre les journaux d'audit avec les signaux de détection ou les statistiques.

## 2. Éléments à tracer

Sous réserve de minimisation et de validation des durées de conservation, le service doit pouvoir relier :

- l'identifiant du signal ou de l'alerte et sa provenance connue ;
- la création, l'association et les changements d'état du dossier ;
- les qualifications, annotations et escalades réalisées par un analyste ;
- la personne qui décide, la justification, le périmètre et le moment de la décision ;
- la mesure décidée, la personne ou l'équipe qui la met en œuvre et son résultat ;
- l'accès aux informations sensibles du dossier lorsque cet accès doit être audité.

Les événements d'audit doivent être protégés contre les modifications non autorisées et accessibles uniquement aux rôles qui en ont besoin. Les méthodes d'intégrité, stockage et conservation sont renvoyées aux phases de données et d'architecture.

## 3. Séparation des données

- Ne pas conserver des secrets, mots de passe ou contenus complets de paiement dans les événements d'audit.
- Ne collecter que les éléments nécessaires à la détection, l'analyse, la réponse et l'audit.
- Référencer les objets métier Eventix par des identifiants contrôlés ; limiter les données personnelles affichées au minimum nécessaire.
- Séparer les preuves d'incident, les signaux bruts, les notes d'analyse et les décisions métier Trust & Safety, même lorsqu'ils sont reliés.
- Restreindre et tracer les exports ou consultations de dossiers sensibles selon les règles à valider.

## 4. Décisions ouvertes

Les durées de conservation, les données masquées, les rôles ayant accès aux preuves, les exigences réglementaires, l'intégrité des traces et les procédures d'export restent à préciser. Voir QMO-041, QMO-055, QMO-057 et QMO-058.

## 5. Traçabilité

Cette définition soutient [EF-146 et EF-147](../04-analyse-des-besoins/exigences-fonctionnelles.md), [RM40](../03-decouverte-du-metier/regles-metier.md) et [le service de supervision cybersécurité](./supervision-cybersecurite.md). Elle ne spécifie pas de technologie de journalisation.
