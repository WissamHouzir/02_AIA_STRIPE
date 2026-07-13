# 03 - OLTP Data Model

## Objectif

Le modele OLTP sert a stocker les transactions Stripe de maniere fiable, rapide et coherente. Il doit respecter les proprietes ACID et eviter les redondances inutiles.

## Tables principales

| Table | Role |
|---|---|
| `customers` | Clients finaux qui effectuent des paiements |
| `merchants` | Entreprises utilisant Stripe |
| `payment_methods` | Moyens de paiement tokenises |
| `transactions` | Paiements effectues via Stripe |
| `payments` | Execution concrete du paiement liee a une transaction |
| `refunds` | Remboursements lies aux transactions |
| `chargebacks` | Litiges ou contestations bancaires |
| `subscriptions` | Abonnements geres via Stripe Billing |
| `fraud_indicators` | Score ou niveau de risque associe a une transaction |
| `currencies` | Devises supportees |
| `countries` | Pays et regions |

## Relations principales

- Un client peut avoir plusieurs moyens de paiement.
- Un marchand peut recevoir plusieurs transactions.
- Une transaction appartient a un client, un marchand et un moyen de paiement.
- Une transaction peut avoir un paiement associe avec son statut de traitement.
- Une transaction peut avoir zero ou plusieurs remboursements.
- Une transaction peut avoir zero ou un chargeback.
- Une transaction peut avoir un score de risque ou un indicateur de fraude.
- Un client peut avoir plusieurs abonnements.

## Exemple de structure

```text
customers(customer_id, email, country_code, created_at)
merchants(merchant_id, merchant_name, country_code, category, created_at)
payment_methods(payment_method_id, customer_id, type, brand, last4, created_at)
transactions(transaction_id, customer_id, merchant_id, payment_method_id, amount, currency, status, created_at)
payments(payment_id, transaction_id, payment_method_id, status, processed_at)
refunds(refund_id, transaction_id, amount, reason, status, created_at)
chargebacks(chargeback_id, transaction_id, amount, reason, status, created_at)
subscriptions(subscription_id, customer_id, merchant_id, status, start_date, end_date)
fraud_indicators(fraud_indicator_id, transaction_id, anomaly_score, risk_level, evaluated_at)
```

## Choix de conception

- Modele normalise pour limiter les doublons.
- Cles primaires sur chaque table.
- Cles etrangeres pour garantir les relations.
- Index sur `customer_id`, `merchant_id`, `transaction_id`, `created_at` et `status`.
- Replication et failover pour garantir la disponibilite.

## Synthese

L'OLTP est la source de verite pour les paiements. Il doit rester simple, fiable et optimise pour les ecritures rapides.
