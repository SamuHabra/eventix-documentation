# Interfaces logiques — Eventix

> Ce document fixe les contrats de responsabilité, pas leurs schémas techniques, protocoles ou transports. Les contrats ci-dessous sont des frontières logiques à valider avant implémentation.

## 1. Règles communes

- Un module consomme uniquement les contrats publiés par le propriétaire des données.
- Un signal communiqué à MOD-13 est un fait observable, pas un verdict ni une instruction d'action.
- Les champs sont minimisés : identifiants opaques, horodatage, source, type d'événement et contexte strictement nécessaire. Aucun secret ou contenu complet de paiement ne doit être envoyé.
- Les événements de signalisation sont tolérants aux doublons et ne bloquent pas l'opération métier d'origine.
- Les actions sur un compte, événement, billet, paiement ou solde passent par le module qui en est propriétaire.

## 2. Contrats liés à la supervision cybersécurité

| Contrat logique | Fournisseur | Consommateur | Contenu fonctionnel minimal | Limite |
|---|---|---|---|---|
| `SignalDeSécurité` | MOD-01 à MOD-12, infrastructure ou fournisseur explicitement approuvé | MOD-13 | Identifiant de signal, source, horodatage, catégorie, référence opaque à l'actif, sévérité éventuelle et contexte minimisé | Le signal n'établit pas qu'une attaque a eu lieu ; aucune donnée brute non nécessaire |
| `IdentitéEtHabilitations` | MOD-01 | MOD-13 | Identité de l'acteur interne et habilitations nécessaires à la consultation/qualification/décision | MOD-13 ne crée ni ne modifie les comptes et droits |
| `DécisionDeRéponseCyber` | MOD-13 après décision humaine habilitée | Module propriétaire de l'actif concerné (p. ex. MOD-01 ou MOD-02) | Référence d'incident, décideur, justification, mesure demandée, portée et identifiant d'idempotence | N'est pas une écriture directe ni une exécution automatique ; cible vérifie l'autorité et trace le résultat |
| `RésultatDeRéponseCyber` | Module ayant exécuté ou refusé la mesure | MOD-13 | Référence de décision, résultat, horodatage, exécutant ou motif de refus | Aucune preuve de résultat ne doit être fabriquée par MOD-13 |
| `RéférenceÀUnDossierMétier` | MOD-10 ou module métier concerné, selon autorisation | MOD-13 | Identifiant opaque et état métier strictement nécessaire à la coordination | Ne fusionne pas l'incident cyber avec une fraude ou une sanction métier |

Les contrats d'action sont activables uniquement après définition des profils d'habilitation, des cas d'usage autorisés, du journal d'audit et du processus de révocation/urgence. Le MVP n'autorise pas qu'une alerte déclenche seule `DécisionDeRéponseCyber`.

## 3. Frontière des paiements

- MOD-05 expose les états et références nécessaires au cycle de paiement ; l'adaptateur du prestataire Mobile Money traite les échanges externes.
- MOD-09 possède l'obligation et le traitement du remboursement ; MOD-08 possède le solde, la clôture et les retraits.
- Les contrats n'exposent pas à MOD-13 de données d'identification financière sensibles ou de secrets fournisseur. Les événements d'incident portent des références opaques et le minimum d'éléments utiles à l'enquête.
- Les identifiants de transaction et accusés du prestataire sont conservés seulement selon la nécessité opérationnelle et les règles de conservation à décider.

## 4. À spécifier avant réalisation

Les versions, champs, signatures, authentification inter-modules, contrôles d'intégrité, idempotence, délais, conservation, politique de reprise et mécanisme d'accès doivent être définis dans la conception détaillée. Ce document ne choisit pas de protocole ni de technologie.
