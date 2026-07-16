# 03 - OLTP Data Model

## Objectif

Le modèle OLTP sert à stocker les transactions Stripe de manière fiable, rapide et cohérente. Il doit respecter les propriétés ACID et éviter les redondances inutiles.

Dans ce projet, l'OLTP représente le cœur opérationnel. C'est le système qui doit rester le plus strict, car une erreur sur un paiement, un remboursement ou un litige peut avoir un impact financier direct.

## Tables principales

| Table | Role |
|---|---|
| `customers` | Clients finaux qui effectuent des paiements |
| `merchants` | Entreprises utilisant Stripe |
| `payment_methods` | Moyens de paiement tokenisés |
| `transactions` | Paiements effectués via Stripe |
| `payments` | Exécution concrète du paiement liée à une transaction |
| `refunds` | Remboursements liés aux transactions |
| `chargebacks` | Litiges ou contestations bancaires |
| `subscriptions` | Abonnements gérés via Stripe Billing |
| `fraud_indicators` | Score ou niveau de risque associé à une transaction |
| `currencies` | Devises supportées |
| `countries` | Pays et régions |

## Relations principales

- Un client peut avoir plusieurs moyens de paiement.
- Un marchand peut recevoir plusieurs transactions.
- Une transaction appartient à un client, un marchand et un moyen de paiement.
- Une transaction peut avoir un paiement associé avec son statut de traitement.
- Une transaction peut avoir zéro ou plusieurs remboursements.
- Une transaction peut avoir zéro ou un chargeback.
- Une transaction peut avoir un score de risque ou un indicateur de fraude.
- Un client peut avoir plusieurs abonnements.

## Exemple de structure

```text
customers(customer_id, email, country_code, created_at)
merchants(merchant_id, merchant_name, country_code, category, created_at)
payment_methods(payment_method_id, customer_id, type, brand, last4, created_at)
transactions(transaction_id, customer_id, merchant_id, payment_method_id, amount, currency, status, created_at)
```

## Choix de conception

- Modèle normalisé pour limiter les doublons.
- Clés primaires sur chaque table.
- Clés étrangères pour garantir les relations.
- Index sur `customer_id`, `merchant_id`, `transaction_id`, `created_at` et `status`.
- Réplication et failover pour garantir la disponibilité.

## Synthèse

L'OLTP est la source de vérité pour les paiements. Il doit rester simple, fiable et optimisé pour les écritures rapides.

Les autres systèmes peuvent réutiliser ses données, mais ils ne doivent pas remplacer son rôle de référence transactionnelle.
