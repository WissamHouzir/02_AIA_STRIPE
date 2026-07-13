# 06 - Data Pipeline Architecture

## Objectif

Le pipeline connecte les systemes OLTP, OLAP et NoSQL. Il doit permettre a la fois le temps reel et le batch.

## Architecture simple

```text
OLTP PostgreSQL
  -> CDC Debezium
  -> Kafka
  -> NoSQL MongoDB
  -> Data Lake
  -> OLAP Warehouse
  -> BI / ML / Reporting
```

## Flux temps reel

| Etape | Description |
|---|---|
| 1 | Une transaction est creee dans l'OLTP |
| 2 | Debezium capture le changement |
| 3 | Kafka publie l'evenement |
| 4 | Le service ML calcule un score de fraude |
| 5 | Le score est stocke dans NoSQL |

Ce flux sert surtout a la detection de fraude et aux alertes rapides.

## Flux batch

| Etape | Description |
|---|---|
| 1 | Les donnees brutes sont stockees dans le data lake |
| 2 | Airflow orchestre les traitements |
| 3 | Les donnees sont nettoyees et transformees |
| 4 | Le data warehouse est alimente |
| 5 | Les agregations sont mises a jour |

Ce flux sert au reporting, aux analyses historiques et a l'entrainement ML.

## Gestion des erreurs

- Rejeu possible des messages Kafka.
- Retries automatiques dans Airflow.
- Deduplication avec `transaction_id`.
- Monitoring du retard CDC/Kafka.
- Alertes en cas d'echec de pipeline.

## Synthese

Le pipeline garde les systemes synchronises sans faire porter toute la charge a la base transactionnelle.
