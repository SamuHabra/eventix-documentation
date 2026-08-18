# Questions Métier Ouvertes — Eventix

> **Statut :** En cours  
> **Phase :** Business Discovery  
> **Objectif :** Centraliser les décisions métier qui restent à prendre avant de passer à l'analyse détaillée des besoins.

---

# 1. Objectif du document

Ce document centralise les questions métier auxquelles Eventix n'a pas encore apporté de réponse définitive.

Une question doit être ajoutée ici lorsqu'elle :

- influence le fonctionnement métier d'Eventix ;
- nécessite une décision des parties prenantes ;
- peut modifier un processus métier ;
- peut modifier une règle métier ;
- peut avoir un impact financier, juridique, opérationnel ou utilisateur ;
- ne doit pas être résolue arbitrairement par l'équipe technique.

## Principe

> **Une question ouverte ne doit pas être transformée en décision technique sans décision métier préalable.**

---

# 2. Statuts

Chaque question possède un statut.

| Statut | Signification |
|---|---|
| `OPEN` | Question non traitée |
| `IN_DISCUSSION` | Question actuellement discutée |
| `DECIDED` | Décision prise |
| `BLOCKED` | Décision impossible pour le moment |
| `CANCELLED` | Question devenue sans objet |

---

# 3. Priorités

| Priorité | Signification |
|---|---|
| `CRITICAL` | Bloque potentiellement la conception ou le fonctionnement du MVP |
| `HIGH` | Peut avoir un impact important |
| `MEDIUM` | Important mais non bloquant |
| `LOW` | Peut être traité ultérieurement |

---

# 4. Paiement

## QMO-001 — Quels moyens de paiement seront disponibles dans le MVP ?

**Statut :** `OPEN`  
**Priorité :** `CRITICAL`

Questions à déterminer :

- Quels moyens de paiement sont acceptés ?
- Mobile Money ?
- Carte bancaire ?
- Autres moyens locaux ?
- Un seul fournisseur ou plusieurs ?
- Eventix doit-il pouvoir changer de fournisseur ultérieurement ?

**Pourquoi cette question est importante :**

Le choix des moyens de paiement influence directement le parcours d'achat et le fonctionnement financier.

---

## QMO-002 — Quelle est la politique exacte concernant les frais de paiement ?

**Statut :** `OPEN`  
**Priorité :** `HIGH`

À déterminer :

- Qui supporte les frais ?
- Participant ?
- Organisateur ?
- Eventix ?
- Partage des frais ?
- Les frais sont-ils affichés avant paiement ?

---

## QMO-003 — Quelle commission Eventix applique-t-il ?

**Statut :** `OPEN`  
**Priorité :** `CRITICAL`

À déterminer :

- pourcentage ;
- montant fixe éventuel ;
- combinaison fixe + pourcentage ;
- commission différente selon le type d'événement ;
- commission supportée par l'organisateur ou le participant.

---

# 5. Réservation

## QMO-004 — Le délai de réservation est-il toujours de 5 minutes ?

**Statut :** `DECIDED`

**Décision actuelle :**

> Une réservation `PENDING` expire après 5 minutes si elle n'est pas finalisée.

---

## QMO-005 — Que se passe-t-il si le paiement arrive après expiration ?

**Statut :** `DECIDED`

**Décision actuelle :**

Une réconciliation est déclenchée.

```text
Paiement tardif
      ↓
Réconciliation
      ↓
Billet disponible ?
   ┌──┴──┐
   ↓     ↓
 Oui    Non
 ↓       ↓
Billet  Remboursement

# 6. Billets

## QMO-006 — Un participant peut-il transférer son billet à une autre personne ?

**Statut :** `OPEN`
**Priorité :** `MEDIUM`

À déterminer :

* transfert autorisé ou non ;
* conditions ;
* changement du propriétaire ;
* historique du transfert ;
* impact sur le contrôle.

---

## QMO-007 — La revente d'un billet est-elle autorisée ?

**Statut :** `OPEN`
**Priorité :** `LOW`

Cette fonctionnalité est considérée comme potentiellement future.

À déterminer ultérieurement :

* revente libre ;
* revente contrôlée par Eventix ;
* prix maximum ;
* commission ;
* lutte contre la spéculation.

---

## QMO-008 — Un billet peut-il être utilisé après modification de certaines informations de l'événement ?

**Statut :** `OPEN`
**Priorité :** `HIGH`

Exemples :

* changement d'heure ;
* changement de lieu ;
* changement de catégorie ;
* changement de siège.

---

# 7. Annulation et report

## QMO-009 — Dans quelles conditions un organisateur peut-il annuler un événement ?

**Statut :** `OPEN`
**Priorité :** `HIGH`

À déterminer :

* annulation libre ;
* délai minimum ;
* justification obligatoire ;
* intervention d'Eventix ;
* conséquences financières.

---

## QMO-010 — Dans quelles conditions un événement peut-il être reporté ?

**Statut :** `OPEN`
**Priorité :** `HIGH`

À déterminer :

* nombre maximal de reports ;
* délai acceptable ;
* changement de lieu ;
* changement d'artiste ;
* changement de catégorie de billet.

---

## QMO-011 — Le participant peut-il demander un remboursement lorsqu'un événement est reporté ?

**Statut :** `OPEN`
**Priorité :** `CRITICAL`

La règle actuelle établit que le billet peut être conservé pour la nouvelle date.

Il reste à définir les conditions permettant éventuellement un remboursement.

---

# 8. Remboursements

## QMO-012 — Dans quelles situations un remboursement est-il obligatoire ?

**Statut :** `OPEN`
**Priorité :** `CRITICAL`

Cas à préciser :

* événement annulé ;
* événement reporté ;
* paiement tardif ;
* double paiement ;
* erreur Eventix ;
* fraude ;
* autre situation.

---

## QMO-013 — Qui supporte financièrement le remboursement ?

**Statut :** `OPEN`
**Priorité :** `CRITICAL`

À déterminer selon la cause :

* Eventix ;
* organisateur ;
* fournisseur de paiement ;
* partage.

---

## QMO-014 — Quel est le délai maximum de traitement d'un remboursement ?

**Statut :** `OPEN`
**Priorité :** `HIGH`

À déterminer :

* délai de déclenchement par Eventix ;
* délai de traitement du fournisseur ;
* délai communiqué au participant.

---

# 9. Contrôle des billets

## QMO-015 — Combien de scanners peuvent être utilisés simultanément ?

**Statut :** `DECIDED`

**Décision actuelle :**

Eventix privilégie la fiabilité du contrôle.

Si la synchronisation entre plusieurs scanners ne peut pas être garantie, le système doit privilégier **un seul scanner actif** plutôt que de risquer l'acceptation d'un même billet plusieurs fois.

### Trade-off

```text
Rapidité ↓
Fiabilité ↑
```

---

## QMO-016 — Que doit faire l'agent lorsqu'un billet est refusé ?

**Statut :** `OPEN`
**Priorité :** `HIGH`

À déterminer :

* afficher uniquement le motif ;
* permettre une vérification supplémentaire ;
* contacter l'organisateur ;
* contacter Eventix ;
* procédure particulière en cas de litige.

---

## QMO-017 — Qui peut autoriser manuellement l'accès à un participant ?

**Statut :** `OPEN`
**Priorité :** `HIGH`

À déterminer :

* agent ;
* organisateur ;
* administrateur Eventix ;
* personne habilitée spécifique.

---

# 10. Vérification des organisateurs

## QMO-018 — Quelles informations sont nécessaires pour vérifier un organisateur ?

**Statut :** `OPEN`
**Priorité :** `CRITICAL`

À déterminer :

* identité ;
* téléphone ;
* email ;
* documents ;
* informations de paiement ;
* informations sur l'organisation ;
* autres justificatifs.

---

## QMO-019 — Tous les organisateurs doivent-ils être vérifiés avant de publier ?

**Statut :** `OPEN`
**Priorité :** `CRITICAL`

Possibilités :

* vérification obligatoire ;
* vérification progressive ;
* vérification selon le risque ;
* vérification manuelle.

---

## QMO-020 — Un événement peut-il être publié avant validation complète de l'organisateur ?

**Statut :** `OPEN`
**Priorité :** `HIGH`

Cette décision influence directement la stratégie de confiance d'Eventix.

---

# 11. Signalements

## QMO-021 — Qui peut signaler un événement ?

**Statut :** `OPEN`
**Priorité :** `MEDIUM`

À déterminer :

* participant uniquement ;
* utilisateur connecté ;
* utilisateur non connecté ;
* organisateur ;
* agent.

---

## QMO-022 — Combien de signalements sont nécessaires pour déclencher une vérification renforcée ?

**Statut :** `OPEN`
**Priorité :** `HIGH`

À déterminer :

> Un nombre fixe de signalements ne doit pas nécessairement être considéré comme une preuve de fraude.

Il faut déterminer la règle d'escalade.

---

## QMO-023 — Qui décide qu'une fraude est confirmée ?

**Statut :** `OPEN`
**Priorité :** `CRITICAL`

Possibilités :

* administrateur ;
* équipe de confiance et sécurité ;
* système automatique ;
* combinaison automatique + humaine.

---

# 12. Bannissement

## QMO-024 — Dans quelles situations une organisation est-elle bannie ?

**Statut :** `OPEN`
**Priorité :** `CRITICAL`

Exemples à étudier :

* fraude confirmée ;
* fausse identité ;
* escroquerie ;
* utilisation répétée de faux événements ;
* contournement des règles ;
* autre violation grave.

---

## QMO-025 — Le bannissement est-il temporaire ou définitif ?

**Statut :** `OPEN`
**Priorité :** `HIGH`

À déterminer :

* durée ;
* conditions de levée ;
* possibilité d'appel ;
* réexamen.

---

## QMO-026 — Une organisation bannie peut-elle faire appel ?

**Statut :** `OPEN`
**Priorité :** `MEDIUM`

À déterminer :

* procédure ;
* délai ;
* preuves ;
* personne chargée de la décision.

---

# 13. Gestion du compte participant

## QMO-027 — Quelles informations sont obligatoires pour créer un compte participant ?

**Statut :** `OPEN`
**Priorité :** `HIGH`

À déterminer :

* nom ;
* prénom ;
* email ;
* téléphone ;
* mot de passe ;
* autres informations.

---

## QMO-028 — Un participant peut-il acheter sans compte ?

**Statut :** `OPEN`
**Priorité :** `HIGH`

À déterminer :

* achat obligatoire avec compte ;
* achat invité ;
* création automatique de compte.

---

## QMO-029 — Dans quelles situations un participant peut-il être banni ?

**Statut :** `OPEN`
**Priorité :** `MEDIUM`

Exemples :

* fraude ;
* tentative de fraude ;
* abus ;
* comportement malveillant ;
* utilisation répétée de faux billets.

---

# 14. Gestion du compte organisateur

## QMO-030 — Quand un utilisateur devient-il officiellement organisateur ?

**Statut :** `OPEN`
**Priorité :** `HIGH`

À déterminer :

* simple activation d'un rôle ;
* validation ;
* vérification d'identité ;
* validation d'une organisation.

---

## QMO-031 — Un organisateur peut-il gérer plusieurs organisations ?

**Statut :** `OPEN`
**Priorité :** `MEDIUM`

À déterminer :

* une personne → une organisation ;
* une personne → plusieurs organisations ;
* plusieurs personnes → une organisation.

---

## QMO-032 — Plusieurs utilisateurs peuvent-ils gérer une même organisation ?

**Statut :** `OPEN`
**Priorité :** `HIGH`

Si oui, il faudra définir les rôles :

* propriétaire ;
* administrateur ;
* gestionnaire ;
* finance ;
* contrôle ;
* etc.

---

# 15. Clôture financière

## QMO-033 — Quand exactement les fonds deviennent-ils disponibles ?

**Statut :** `DECIDED`

**Décision actuelle :**

> L'organisateur ne peut retirer les fonds qu'après la fin de l'événement et la clôture financière.

---

## QMO-034 — Comment calculer exactement le montant disponible pour l'organisateur ?

**Statut :** `OPEN`
**Priorité :** `CRITICAL`

À déterminer :

```text
Ventes
- remboursements
- commissions
- frais
+/- ajustements
= solde disponible
```

---

## QMO-035 — Un organisateur peut-il retirer seulement une partie de son solde ?

**Statut :** `DECIDED`

**Décision actuelle :**

Oui.

Exemple :

```text
Solde = 500 000 FCFA
Retrait = 300 000 FCFA
Nouveau solde = 200 000 FCFA
```

---

## QMO-036 — Que se passe-t-il lorsqu'un retrait échoue ?

**Statut :** `DECIDED`

**Décision actuelle :**

Le montant est restitué au solde disponible et l'organisateur peut effectuer une nouvelle tentative.

---

# 16. Siège et disponibilité

## QMO-037 — Les événements peuvent-ils avoir des sièges numérotés ?

**Statut :** `DECIDED`

**Décision actuelle :**

Oui.

L'organisateur peut configurer les billets selon les sièges lorsque l'événement le nécessite.

---

## QMO-038 — Comment gérer les conflits de réservation sur un même siège ?

**Statut :** `OPEN`
**Priorité :** `CRITICAL`

À déterminer au niveau métier :

> Un même siège ne doit pas être vendu deux fois.

Les mécanismes techniques permettant de garantir cette règle seront déterminés ultérieurement.

---

# 17. Communication

## QMO-039 — Quels événements doivent générer une notification ?

**Statut :** `OPEN`
**Priorité :** `MEDIUM`

Exemples :

* réservation ;
* paiement ;
* billet émis ;
* événement annulé ;
* événement reporté ;
* remboursement ;
* retrait ;
* signalement.

---

## QMO-040 — Quels canaux de communication sont prioritaires ?

**Statut :** `OPEN`
**Priorité :** `MEDIUM`

Possibilités :

* email ;
* SMS ;
* WhatsApp ;
* notification dans l'application.

### MVP actuellement envisagé

* email ;
* notification dans Eventix.

---

# 18. Données et historique métier

## QMO-041 — Combien de temps les données métier doivent-elles être conservées ?

**Statut :** `OPEN`
**Priorité :** `HIGH`

À déterminer notamment pour :

* transactions ;
* billets ;
* remboursements ;
* signalements ;
* bannissements ;
* historiques de contrôle.

---

## QMO-042 — Quelles actions doivent être historisées ?

**Statut :** `OPEN`
**Priorité :** `HIGH`

Exemples :

* création ;
* modification ;
* publication ;
* annulation ;
* report ;
* paiement ;
* remboursement ;
* contrôle ;
* bannissement ;
* retrait.

---

# 19. Questions stratégiques

## QMO-043 — Quelle est la politique commerciale d'Eventix ?

**Statut :** `OPEN`
**Priorité :** `HIGH`

À déterminer :

* commission ;
* abonnement éventuel ;
* services supplémentaires ;
* frais ;
* offres promotionnelles.

---

## QMO-044 — Eventix garantit-il l'authenticité de tous les événements publiés ?

**Statut :** `OPEN`
**Priorité :** `CRITICAL`

Cette question est fondamentale pour la proposition de valeur d'Eventix.

Il faut définir précisément ce que signifie :

> « Événement vérifié »

et ce qu'Eventix garantit réellement.
