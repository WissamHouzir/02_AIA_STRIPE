# 05 - NoSQL Data Model

## Objectif

Le NoSQL sert a stocker les donnees flexibles ou semi-structurees : logs, sessions, feedbacks clients et resultats de machine learning.

## Collections principales

| Collection | Contenu |
|---|---|
| `fraud_features` | Features et scores de fraude par transaction |
| `sessions` | Sessions web/mobile et evenements clickstream |
| `customer_feedback` | Notes, commentaires et sentiment client |
| `application_logs` | Logs techniques des services |
| `audit_trail` | Historique des actions sensibles |

## Exemple de document fraude

```json
{
  "transaction_id": "trx_1001",
  "customer_id": "cus_501",
  "merchant_id": "mer_101",
  "scored_at": "2026-07-11T10:15:00Z",
  "features": {
    "amount": 249.90,
    "currency": "EUR",
    "device_type": "mobile",
    "is_new_device": true,
    "ip_country": "FR"
  },
  "model_output": {
    "anomaly_score": 0.91,
    "risk_level": "high",
    "manual_review_required": true
  }
}
```

## Strategie de modelisation

- Embedding pour les petits objets lies au document, comme `features`.
- Referencing avec `transaction_id`, `customer_id` et `merchant_id` pour relier OLTP, OLAP et NoSQL.
- Index sur `transaction_id`, `customer_id`, `merchant_id`, `scored_at` et `model_output.risk_level`.

## Synthese

Le NoSQL complete le relationnel. Il evite de surcharger l'OLTP avec des donnees variables comme les logs, sessions et resultats ML.
