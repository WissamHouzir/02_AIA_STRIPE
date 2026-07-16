# 04 - OLAP Data Model

## Objectif

Le modèle OLAP sert à analyser les données Stripe : revenus, transactions, fraude, remboursements, performances marchands et segmentation client.

Il répond aux questions métier qui ne doivent pas être exécutées directement sur l'OLTP, afin de ne pas ralentir le traitement des paiements.

## Type de schéma

Le choix retenu est un **schéma en étoile** simple. Il facilite les jointures et les agrégations.

## Table de faits principale

| Table | Grain | Mesures |
|---|---|---|
| `fact_transactions` | 1 ligne = 1 transaction | montant, montant USD, frais Stripe, score fraude, indicateur remboursement |

## Dimensions

| Dimension | Role |
|---|---|
| `dim_date` | Analyse par jour, mois, trimestre, année |
| `dim_customer` | Segment, pays, type de client |
| `dim_merchant` | Marchand, catégorie, pays |
| `dim_payment_method` | Type de paiement et marque |
| `dim_currency` | Devise et taux de conversion |
| `dim_location` | Pays, région, ville |

## Tables agrégées

| Table | Utilité |
|---|---|
| `agg_daily_revenue` | Revenu quotidien par marchand et devise |
| `agg_monthly_revenue` | Revenu mensuel global |
| `agg_monthly_fraud` | Suivi mensuel des transactions suspectes |

## Optimisations

- Partitionnement par date.
- Index ou clustering par marchand, pays et devise.
- Vues matérialisées pour les rapports fréquents.
- Conversion des montants en USD pour comparer les devises.

## Synthèse

L'OLAP n'est pas utilisé pour traiter les paiements. Il sert uniquement à analyser les données déjà collectées, avec un modèle simple et rapide à interroger.

Cette séparation permet aux équipes métier d'explorer les données sans risquer de perturber les systèmes transactionnels.
