# Service organisationnel — Finance

| Propriété | Définition |
|---|---|
| **Bénéficiaire principal** | Organisateur |
| **Bénéficiaires associés** | Participant, point physique partenaire, équipe financière Eventix |
| **Objet** | Suivre les flux financiers issus de la billetterie et le règlement de l'organisateur |
| **Statut** | Cycle métier documenté ; conditions contractuelles et financières détaillées à arbitrer |

## 1. Besoin métier

L'organisateur doit pouvoir comprendre les ventes générées par son événement, les ajustements qui affectent les recettes et le moment où un montant devient disponible au règlement. Eventix doit distinguer la confirmation d'un paiement, la clôture financière et le retrait du solde : ce sont des étapes différentes.

Ce service aide au suivi financier de la billetterie. Il ne remplace ni la comptabilité de l'organisateur ni les conseils fiscaux, juridiques ou financiers.

## 2. Parcours financier de référence

```text
Commande
   ↓
Paiement confirmé ou obtention gratuite
   ↓
Enregistrement des ventes et ajustements
   ↓
Fin de l'événement
   ↓
Réconciliation et clôture financière
   ↓
Solde organisateur disponible
   ↓
Demande et traitement du retrait
```

Les remboursements, échecs de retrait et autres ajustements doivent être pris en compte selon leur cycle propre. Un paiement confirmé ne signifie pas que les fonds sont immédiatement disponibles pour l'organisateur.

## 3. Capacités documentées

- Suivre les ventes par événement, catégorie et canal, selon les informations disponibles.
- Distinguer le montant des dons du prix des billets.
- Prendre en compte les frais, remboursements et commissions applicables pour déterminer le montant net.
- Réconcilier les opérations nécessaires avant la clôture.
- Rendre consultable le solde disponible et gérer les demandes de retrait.
- Représenter la commission d'un point physique selon la convention applicable.

Les besoins **B13**, **B33** et **B34** ainsi que **RM26** établissent ces sujets métier. Les exigences correspondantes incluent notamment les domaines de paiement, de dons, de clôture financière et de retraits.

## 4. Valeur et responsabilités

- **Organisateur** : comprend l'origine et l'état de ses recettes, le montant disponible et le statut d'un retrait.
- **Point physique partenaire** : sa commission peut être prise en compte lorsque la convention de vente est définie.
- **Participant** : reçoit les informations de paiement et de remboursement prévues par son parcours.
- **Eventix** : conserve les distinctions nécessaires entre paiement, don, frais, remboursement, solde et retrait, sans les confondre.

## 5. Limites et décisions encore ouvertes

Les délais et conditions exacts de versement, les frais, les commissions, le partage de frais, le traitement fiscal, le bénéficiaire et le remboursement des dons ne sont pas fixés ici. Un schéma de ventilation illustre des catégories possibles, mais ne constitue pas un barème ni un engagement contractuel.

Questions de référence : **QMO-001 à QMO-003** (paiement et frais), **QMO-043** (politique commerciale) et **QMO-045 à QMO-047** (traitement financier, remboursement et fiscalité des dons).

## 6. Traçabilité

- Besoins : [B13 — Suivre les ventes](../besoins-metier.md#5-besoins-liés-à-la-vente), [B33–B34 — Règlement et commissions](../besoins-metier.md#14-besoins-liés-au-règlement-financier).
- Règle : [RM26 — Le règlement de l'organisateur est différé](../regles-metier.md).
- Exigences : notamment **EF-035 à EF-041**, **EF-105 à EF-114**, **EF-135 à EF-137** dans [les exigences fonctionnelles](../../04-analyse-des-besoins/exigences-fonctionnelles.md).
- Questions ouvertes : [questions métier](../questions-metier-ouvertes.md).

