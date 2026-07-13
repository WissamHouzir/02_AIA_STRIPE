# 04 - OLAP Data Model

## Objectif

Le modele OLAP sert a analyser les donnees Stripe : revenus, transactions, fraude, remboursements, performances marchands et segmentation client.

## Type de schema

Le choix retenu est un **schema en etoile** simple. Il facilite les jointures et les agregations.

## Table de faits principale

| Table | Grain | Mesures |
|---|---|---|
| `fact_transactions` | 1 ligne = 1 transaction | montant, montant USD, frais Stripe, score fraude, indicateur remboursement |

## Dimensions

| Dimension | Role |
|---|---|
| `dim_date` | Analyse par jour, mois, trimestre, annee |
| `dim_customer` | Segment, pays, type de client |
| `dim_merchant` | Marchand, categorie, pays |
| `dim_payment_method` | Type de paiement et marque |
| `dim_currency` | Devise et taux de conversion |
| `dim_location` | Pays, region, ville |

## Tables agregees

| Table | Utilite |
|---|---|
| `agg_daily_revenue` | Revenu quotidien par marchand et devise |
| `agg_monthly_revenue` | Revenu mensuel global |
| `agg_monthly_fraud` | Suivi mensuel des transactions suspectes |

## Optimisations

- Partitionnement par date.
- Index ou clustering par marchand, pays et devise.
- Vues materialisees pour les rapports frequents.
- Conversion des montants en USD pour comparer les devises.

## Synthese

L'OLAP n'est pas utilise pour traiter les paiements. Il sert uniquement a analyser les donnees deja collectees, avec un modele simple et rapide a interroger.
