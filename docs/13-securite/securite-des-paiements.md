# Sécurité des paiements — Eventix

| Propriété | Valeur |
|---|---|
| **Périmètre produit actuel** | Paiements Mobile Money via un prestataire externe ; marché initial : Cameroun |
| **Statut** | Cadrage des responsabilités et exigences ; intégration et contrôles à valider |
| **Sources** | EF-035 à EF-041, EF-135, EF-136, EF-144 ; MOD-05, MOD-08, MOD-09 ; D-ARCH-06 |

## 1. Objectif et frontière

Ce document définit les responsabilités et risques à traiter dans le cycle financier d'Eventix. Il ne choisit pas de prestataire, protocole, bibliothèque, mécanisme cryptographique, méthode de stockage ni exigence de conformité externe non établie.

À ce stade, le moyen de paiement documenté est Mobile Money. L'éventuelle collecte ou conservation de données de carte n'est pas incluse dans le périmètre défini ici. Si les cartes ou un nouveau moyen de paiement sont ajoutés, la frontière de données et les obligations applicables devront faire l'objet d'une analyse spécifique avant activation. Aucune revendication de conformité PCI n'est formulée par ce document.

Le prestataire Mobile Money reste un système externe et une autorité sur ses propres opérations. Eventix conserve son autorité sur l'état métier du paiement, la commande, le billet, le remboursement et le solde organisateur selon le module propriétaire concerné.

## 2. Responsabilités par module

| Module | Responsabilité de sécurité financière | Ne possède pas |
|---|---|---|
| **MOD-05 — Payment Processing** | Initier/suivre le paiement, vérifier et traiter les confirmations, assurer l'idempotence et la réconciliation des paiements tardifs | Le billet émis (MOD-06), l'obligation de remboursement (MOD-09), le solde organisateur (MOD-08) |
| **MOD-09 — Refund Management** | Déterminer l'obligation, calculer et suivre le remboursement, éviter les doublons | L'état du paiement d'origine ou le solde organisateur |
| **MOD-08 — Financial Settlement** | Calculer la clôture et le solde retirable, tracer les retraits et restitutions d'échec | Le paiement participant ou l'obligation de remboursement |
| **MOD-13 — Cybersecurity Operations** | Recevoir des signaux financiers minimisés, ouvrir et suivre une investigation, tracer une décision humaine | Toute opération de paiement/remboursement/retrait ou transition détenue par les modules financiers |
| **MOD-11 — Analytics & Observability** | Restituer des statistiques métier agrégées et faits autorisés | Décision financière, approbation de transaction ou preuve d'autorisation |

L'arbitrage d'un incident cyber ne modifie pas les soldes et n'annule pas de paiement. Une réponse ayant un effet sur une opération financière doit être décidée humainement et exécutée par le module propriétaire, avec ses contrôles métier et sa trace propre.

## 3. Principes à préserver

1. **Autorité distincte** : les notifications d'un prestataire ne sont pas, à elles seules, des écritures autoritaires dans l'état métier Eventix.
2. **Vérification** : toute confirmation entrante doit être associée à une opération Eventix attendue et vérifiée selon le contrat du prestataire avant de produire un effet métier.
3. **Idempotence** : une confirmation, un remboursement ou un retrait livré plusieurs fois ne peut produire plusieurs effets pour une même opération/cause.
4. **Séparation des états** : réservation, paiement, billet, remboursement et règlement organisateur restent des cycles distincts.
5. **Montants calculés côté Eventix** : un montant, une devise, un bénéficiaire ou une référence reçus du client ne sont pas acceptés sans rapprochement avec la commande et les règles Eventix.
6. **Minimisation** : ne pas collecter ou journaliser de secret d'authentification du prestataire, code de validation, PIN, contenu de portefeuille ni donnée personnelle/financière non nécessaire.
7. **Traçabilité** : toute transition financière critique conserve l'identifiant de corrélation, la source, l'heure, le résultat et le motif requis sans enregistrer les secrets.
8. **Aucune décision du tableau de bord** : un indicateur ou une anomalie ne confirme pas une fraude et ne déclenche pas automatiquement un remboursement, blocage ou bannissement.

## 4. Risques à couvrir dans la conception

Les scénarios sont des risques à analyser, pas des vulnérabilités constatées :

- confirmation falsifiée, mal associée ou reçue par un canal inattendu ;
- rejeu ou duplication d'une confirmation, d'une demande de remboursement ou d'un retrait ;
- altération du montant, de la devise, de la commande ou du bénéficiaire ;
- paiement tardif après expiration ou changement d'état de la réservation ;
- écart entre statut du prestataire et état métier Eventix ;
- abus d'accès à l'interface organisateur ou aux fonctions de remboursement/retrait ;
- exposition de secrets, identifiants de transaction ou informations personnelles dans les journaux, exports ou tableaux de bord ;
- compromission ou indisponibilité du prestataire ou d'une intégration ;
- erreurs de rapprochement et opérations financières impossibles à expliquer après incident.

## 5. Signaux vers la supervision cyber

MOD-05, MOD-08 et MOD-09 peuvent publier vers MOD-13 des signaux filtrés nécessaires à la détection ou à l'investigation, par exemple un échec inhabituel, une incohérence de corrélation ou un volume anormal de reprises. Le contrat doit éviter les données de paiement brutes et secrets ; les références de paiement sont opaques et les détails restent consultés auprès du propriétaire uniquement si l'accès est autorisé et nécessaire.

MOD-13 peut qualifier un incident et consigner une décision humaine. Il n'interroge pas les bases financières directement, ne suspend pas un paiement de sa propre initiative, ne modifie pas les écritures et ne transforme pas un signal en conclusion automatique de fraude.

## 6. Points de contrôle à valider avant production

- Le prestataire et son contrat d'intégration, ses responsabilités et ses garanties sont identifiés.
- Les opérations entrantes et sortantes (paiement, remboursement, retrait) sont inventoriées séparément.
- La vérification des accusés, les états de transaction, l'idempotence et la réconciliation sont testés contre les répétitions, retards, altérations et interruptions.
- Les permissions d'initiation, remboursement, retrait et consultation sont définies selon le moindre privilège et séparées lorsque requis.
- La gestion, rotation et révocation des secrets du prestataire est définie dans le cadre général de gestion des secrets.
- Les données conservées, durées, accès, exports, journaux d'audit et procédure de réponse à incident sont approuvés.
- Les environnements et comptes de test sont séparés des opérations réelles ; les essais offensifs restent explicitement autorisés et limités.
- Les cas de litige et rapprochement manuel disposent d'une piste d'audit exploitable sans divulguer de secret.

## 7. Décisions encore ouvertes

- Quel prestataire Mobile Money et quel mode d'intégration seront retenus ?
- Quels moyens additionnels, s'il y en a, seront inclus après le MVP ?
- Les remboursements et retraits utilisent-ils le même prestataire ou d'autres canaux (question C9 de `modules.md`) ?
- Quelles données transactionnelles et personnelles sont nécessaires, où sont-elles traitées et combien de temps sont-elles conservées ?
- Quelles habilitations peuvent consulter, initier, approuver ou exécuter les opérations financières ?
- Quels délais, responsabilités de rapprochement et processus d'escalade s'appliquent en cas d'écart ?

La décision d'architecture D-ARCH-06 fixe uniquement les propriétaires logiques (MOD-05, MOD-09, MOD-08) et leur séparation avec MOD-13 ; elle ne tranche aucune de ces questions.
