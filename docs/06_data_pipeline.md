# 06 - Data Pipeline Architecture

## Objectif

Le pipeline connecte les systèmes OLTP, OLAP et NoSQL. Il doit permettre à la fois le temps réel et le batch.

Son rôle est de faire circuler la donnée sans créer de dépendance directe entre tous les systèmes. Les paiements restent traités dans l'OLTP, tandis que les analyses, contrôles et modèles ML sont alimentés par des flux dédiés.

## Architecture simple

```text
OLTP PostgreSQL
  -> CDC Debezium
  -> Kafka
  -> Schema Registry / Dead Letter Queue
  -> NoSQL MongoDB
  -> Data Lake
  -> Airflow
  -> Transformations / Data Quality
  -> OLAP Warehouse
  -> BI / ML / Reporting
```

## Composants nécessaires

Pour faire fonctionner le pipeline de bout en bout, l'architecture doit inclure :

| Composant | Rôle |
|---|---|
| PostgreSQL OLTP | Source transactionnelle principale |
| Debezium CDC | Capture des changements depuis l'OLTP |
| Kafka | Transport des événements en temps réel |
| Schema Registry | Versionnement et validation des schémas d'événements |
| Dead Letter Queue | Stockage des messages invalides ou en erreur |
| MongoDB NoSQL | Stockage des logs, sessions, feedbacks et scores ML |
| Data Lake | Stockage brut, historique et rejouable |
| Airflow Scheduler | Planification des DAGs |
| Airflow Webserver | Suivi des exécutions et diagnostic |
| Airflow Workers | Exécution des tâches batch |
| Airflow Metadata DB | État des DAGs, runs, tâches et retries |
| Transformations | Nettoyage, jointures, enrichissement et agrégations |
| Data Quality Checks | Validation des volumes, schémas, doublons et valeurs nulles |
| OLAP Warehouse | Tables de faits, dimensions, agrégats et vues matérialisées |
| Monitoring et alerting | Suivi des retards, échecs et performances |
| Secrets Manager | Gestion des identifiants et clés de connexion |

## Flux temps réel

| Étape | Description |
|---|---|
| 1 | Une transaction est créée dans l'OLTP |
| 2 | Debezium capture le changement |
| 3 | Kafka publie l'événement |
| 4 | Le service de fraude consomme l'événement Kafka et calcule un score |
| 5 | Le score est stocké dans NoSQL |

Ce flux sert surtout à la détection de fraude et aux alertes rapides.

## Flux batch

| Étape | Description |
|---|---|
| 1 | Les données brutes sont stockées dans le data lake |
| 2 | Airflow déclenche les DAGs planifiés |
| 3 | Les contrôles de qualité vérifient les schémas, doublons et volumes |
| 4 | Les données sont nettoyées, transformées et enrichies |
| 5 | Le data warehouse est alimenté |
| 6 | Les agrégations et vues matérialisées sont mises à jour |
| 7 | Les datasets ML et rapports BI sont rafraîchis |

Ce flux sert au reporting, aux analyses historiques et à l'entraînement ML.

## Rôle d'Airflow

Airflow pilote les workflows batch sans traiter directement les transactions temps réel. Les DAGs principaux peuvent être :

- `load_oltp_to_lake` pour charger les exports ou snapshots dans le data lake ;
- `validate_raw_data` pour contrôler les fichiers et événements entrants ;
- `build_warehouse_dimensions` pour mettre à jour les dimensions OLAP ;
- `build_fact_transactions` pour construire la table de faits ;
- `refresh_aggregates` pour mettre à jour les tables agrégées ;
- `prepare_ml_features` pour préparer les features de fraude et segmentation ;
- `compliance_reporting` pour produire les exports de conformité.

## Gestion des erreurs

- Rejeu possible des messages Kafka.
- Retries automatiques dans Airflow.
- Deduplication avec `transaction_id`.
- Monitoring du retard CDC/Kafka.
- Dead Letter Queue pour les événements invalides.
- Alertes Airflow en cas de tâche échouée ou trop lente.
- Alertes en cas d'échec de pipeline.

## Synthèse

Le pipeline garde les systèmes synchronisés sans faire porter toute la charge à la base transactionnelle.

Il apporte aussi une meilleure capacité de reprise : en cas d'échec, les messages peuvent être rejoués et les traitements Airflow relancés.
