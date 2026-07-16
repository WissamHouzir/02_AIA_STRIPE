# Projet AIA Stripe — Architecture Data

## Présentation du projet

Ce projet présente la conception d'une architecture data complète pour Stripe, une plateforme de paiement en ligne. L'objectif est de proposer une solution capable de gérer des transactions financières en temps réel, tout en permettant l'analyse des données, la détection de fraude, l'exploitation de données semi-structurées et le respect des exigences de sécurité.

Le projet montre comment séparer les usages data principaux :

- **OLTP** pour enregistrer les paiements, remboursements, abonnements et litiges de manière fiable ;
- **OLAP** pour analyser les revenus, les marchands, les clients, les remboursements et la fraude ;
- **NoSQL** pour stocker les logs, sessions, feedbacks clients et résultats de machine learning ;
- **Kafka et CDC** pour synchroniser les données entre les systèmes ;
- **Airflow** pour orchestrer les traitements batch et les contrôles de qualité ;
- **Machine learning** pour enrichir les données avec des scores de fraude.

L'idée centrale est de ne pas faire porter tous les usages à une seule base de données. Chaque composant a un rôle précis afin de garder une architecture fiable, scalable, maintenable et adaptée à des données financières sensibles.

## Ce que j'ai réalisé

Dans ce projet, j'ai conçu les principaux éléments d'une architecture data pour Stripe :

1. Analyse des besoins métier : paiements fiables, reporting, fraude, conformité et scalabilité.
2. Conception d'une architecture globale reliant OLTP, Kafka, Data Lake, NoSQL, Airflow, OLAP, BI et ML.
3. Modélisation d'un système OLTP normalisé pour les transactions, clients, marchands et paiements.
4. Modélisation d'un schéma OLAP en étoile avec table de faits, dimensions et tables agrégées.
5. Conception d'un modèle NoSQL pour les données flexibles : sessions, logs, feedbacks et scores de fraude.
6. Définition d'un pipeline data combinant temps réel et batch avec Debezium, Kafka et Airflow.
7. Ajout d'un plan sécurité et conformité autour du chiffrement, RBAC, audit logs, GDPR et PCI-DSS.
8. Proposition d'une stratégie machine learning pour la détection de fraude et la segmentation client.
9. Rédaction de requêtes SQL et NoSQL pour montrer comment exploiter les modèles proposés.
10. Création d'un glossaire adapté au projet pour expliquer les termes techniques importants.

## Architecture proposée

Le flux général de l'architecture est le suivant :

```text
Applications Stripe
  -> Base OLTP
  -> CDC / Kafka
  -> NoSQL et Data Lake
  -> Airflow
  -> Data Warehouse OLAP
  -> BI / Reporting / Compliance / Machine Learning
```

Cette architecture permet de traiter les paiements dans un système transactionnel fiable, puis de diffuser les données vers des systèmes spécialisés pour l'analyse, le reporting et la fraude.

## Structure du dossier

- `docs/01_business_requirements.md` : analyse du besoin métier.
- `docs/02_global_architecture.md` : architecture data globale.
- `docs/03_oltp_model.md` : modèle transactionnel OLTP.
- `docs/04_olap_model.md` : modèle analytique OLAP.
- `docs/05_nosql_model.md` : modèle NoSQL.
- `docs/06_data_pipeline.md` : architecture des pipelines data.
- `docs/07_security_compliance.md` : sécurité et conformité.
- `docs/08_machine_learning.md` : intégration machine learning.
- `docs/09_queries.md` : exemples de requêtes SQL et NoSQL.
- `diagrams/architecture.mmd` : diagramme global d'architecture.
- `diagrams/oltp_dbdiagram.dbml` : modèle OLTP compatible dbdiagram.
- `diagrams/olap_dbdiagram.dbml` : modèle OLAP compatible dbdiagram.
- `resources/glossaire.md` : glossaire des notions utilisées dans le projet.
- `resources/Stripe_case.md` : cas d'étude initial.

## Livrables

Le projet contient les livrables suivants :

1. Diagramme global d'architecture data.
2. ERD du système OLTP.
3. Schéma OLAP détaillé.
4. Modèle NoSQL détaillé.
5. Document d'architecture des pipelines data.
6. Plan sécurité et conformité.
7. Stratégie d'intégration machine learning.
8. Requêtes SQL et NoSQL démonstratives.
9. Glossaire technique adapté au projet.

## Synthèse

Ce projet propose une architecture data cohérente pour un contexte de paiement à grande échelle. L'OLTP garantit la fiabilité des transactions, Kafka assure la circulation des événements, Airflow orchestre les traitements batch, l'OLAP sert les analyses métier, le NoSQL gère les données flexibles et le machine learning permet d'améliorer la détection de fraude.
