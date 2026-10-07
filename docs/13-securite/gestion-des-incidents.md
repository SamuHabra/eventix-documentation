# Gestion des incidents de cybersécurité

## 1. Objectif

Organiser le traitement humain des alertes et incidents affectant la confidentialité, l'intégrité ou la disponibilité du système Eventix, de ses données ou de ses services.

## 2. Processus de traitement

1. **Détection** — une source configurée rend un signal ou une alerte disponible ; sa provenance et ses limites sont conservées.
2. **Triage** — un analyste habilité vérifie le contexte, la sévérité estimée et les actifs potentiellement touchés.
3. **Qualification** — l'analyste classe le dossier comme à investiguer, confirmé, faux positif ou non concluant, avec justification.
4. **Escalade** — le dossier est transmis au responsable habilité lorsque l'impact ou l'urgence le justifie selon une politique à établir.
5. **Décision** — le responsable humain décide de surveiller, d'investiguer davantage ou d'autoriser une réponse proportionnée.
6. **Exécution et suivi** — la personne responsable de la mise en œuvre consigne l'action et son résultat.
7. **Résolution** — l'analyste vérifie les informations disponibles, clôt le dossier ou consigne pourquoi il reste ouvert.
8. **Retour d'expérience** — les constats pertinents alimentent les corrections et l'amélioration de la détection.

## 3. Règle d'autorisation du MVP

Une alerte ou un résultat automatique ne déclenche pas à lui seul une mesure de confinement, une suspension, une sanction ou une modification du système. Toute mesure ayant un impact doit être décidée et autorisée par une personne compétente. La décision, sa justification, son périmètre, son auteur, son exécution et son résultat sont traçables.

Les mesures d'urgence, leur niveau d'approbation et les personnes de garde ne sont pas définis ici. Il faut les établir avant la mise en production ; aucun SLA ou fonctionnement 24/7 n'est présumé.

## 4. Coordination métier

Si un incident cyber touche un organisateur, un billet, un paiement ou un événement, l'équipe cyber transmet les faits minimaux nécessaires au responsable métier concerné. L'équipe Trust & Safety conserve l'autorité sur les décisions de vérification, de restriction ou de sanction métier ; le SOC ne déduit pas une fraude métier de la seule alerte cyber.

## 5. Communication et notification

Les responsables, canaux, délais, obligations de notification et messages destinés aux parties concernées doivent être définis conformément au cadre légal et contractuel applicable. Ne pas notifier automatiquement un utilisateur ou un tiers sur la base d'une alerte non qualifiée.

## 6. Traçabilité et décisions à prendre

Les données de traitement relèvent d'[audit.md](./audit.md). Les rôles, priorités, critères d'escalade, obligations de notification, conservation et cadre d'autorisation sont à confirmer par QMO-054 à QMO-058.

Le processus soutient [EF-145–EF-147](../04-analyse-des-besoins/exigences-fonctionnelles.md), [RM40](../03-decouverte-du-metier/regles-metier.md) et [UC-037](../04-analyse-des-besoins/use-cases.md).
