# 09 - SQL and NoSQL Queries

## Objectif

Ces requêtes montrent comment répondre à quelques questions métier avec les modèles OLTP, OLAP et NoSQL.

## 1. Revenu mensuel par pays

```sql
SELECT
  d.year,
  d.month,
  l.country,
  SUM(f.amount_usd) AS revenue_usd,
  COUNT(*) AS transaction_count
FROM fact_transactions f
JOIN dim_date d ON d.date_key = f.date_key
JOIN dim_location l ON l.location_key = f.location_key
WHERE f.status = 'successful'
GROUP BY d.year, d.month, l.country
ORDER BY d.year, d.month, revenue_usd DESC;
```

## 2. Marchands avec le plus grand volume

```sql
SELECT
  m.merchant_name,
  COUNT(*) AS transaction_count,
  SUM(f.amount_usd) AS total_volume_usd
FROM fact_transactions f
JOIN dim_merchant m ON m.merchant_key = f.merchant_key
GROUP BY m.merchant_name
ORDER BY total_volume_usd DESC
LIMIT 10;
```

## 3. Taux de remboursement par moyen de paiement

```sql
SELECT
  p.payment_method_type,
  COUNT(*) AS total_transactions,
  SUM(CASE WHEN f.is_refunded THEN 1 ELSE 0 END) AS refunded_transactions,
  ROUND(SUM(CASE WHEN f.is_refunded THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS refund_rate_pct
FROM fact_transactions f
JOIN dim_payment_method p ON p.payment_method_key = f.payment_method_key
GROUP BY p.payment_method_type
ORDER BY refund_rate_pct DESC;
```

## 4. Transactions à risque élevé

```javascript
db.fraud_features.find(
  {
    "model_output.risk_level": { $in: ["high", "critical"] }
  },
  {
    transaction_id: 1,
    customer_id: 1,
    merchant_id: 1,
    scored_at: 1,
    "model_output.anomaly_score": 1,
    "model_output.risk_level": 1
  }
).sort({ "model_output.anomaly_score": -1 })
```

## 5. Sessions avec tentative de paiement

```javascript
db.sessions.find(
  {
    "events.event_type": "payment_attempt"
  },
  {
    customer_id: 1,
    started_at: 1,
    device: 1,
    events: 1
  }
).sort({ started_at: -1 }).limit(50)
```

## Synthèse

Les requêtes SQL servent surtout aux analyses structurées dans l'OLAP. Les requêtes NoSQL servent aux données flexibles comme fraude, sessions et logs.
