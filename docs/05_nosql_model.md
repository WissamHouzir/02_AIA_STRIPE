# 05 - NoSQL Data Model

## Objectif

Le NoSQL sert à stocker les données flexibles ou semi-structurées : logs, sessions, feedbacks clients et résultats de machine learning.

Ces données évoluent plus vite que les tables transactionnelles. Le NoSQL permet donc d'ajouter de nouveaux champs ou signaux de fraude sans modifier en permanence le modèle OLTP.

## Collections principales

| Collection | Contenu |
|---|---|
| `fraud_features` | Features et scores de fraude par transaction |
| `sessions` | Sessions web/mobile et événements clickstream |
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

## Stratégie de modélisation

- Embedding pour les petits objets liés au document, comme `features`.
- Referencing avec `transaction_id`, `customer_id` et `merchant_id` pour relier OLTP, OLAP et NoSQL.
- Index sur `transaction_id`, `customer_id`, `merchant_id`, `scored_at` et `model_output.risk_level`.

## Synthèse

Le NoSQL complète le relationnel. Il évite de surcharger l'OLTP avec des données variables comme les logs, sessions et résultats ML.

Il joue aussi un rôle important pour relier les événements utilisateurs aux scores de risque et aux analyses de comportement.
