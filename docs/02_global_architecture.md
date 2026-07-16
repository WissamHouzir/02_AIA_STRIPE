# 02 - Global Data Architecture

## Objectif

Ce document présente l'architecture data globale proposée pour Stripe. Elle relie les systèmes OLTP, OLAP et NoSQL afin de traiter les transactions, produire des analyses et exploiter les données semi-structurées.

L'idée générale est de construire une architecture en couches. Chaque couche a un rôle clair : l'OLTP sécurise l'opérationnel, Kafka diffuse les changements, le data lake conserve l'historique, Airflow orchestre les traitements batch et l'OLAP sert les analyses.

## Vue d'ensemble

Flux principal :

```text
Applications Stripe
  -> Base OLTP
  -> CDC / Kafka
  -> Data Lake / NoSQL / Data Warehouse
  -> Airflow / Transformations / Data Quality
  -> BI / Reporting / Machine Learning / Fraud Detection
```

Le diagramme correspondant se trouve dans `diagrams/architecture.mmd`.

Le schéma reste volontairement synthétique pour montrer les flux principaux sans détailler tous les composants internes. Les détails d'exploitation sont décrits dans les sections suivantes.

## Composants principaux

### OLTP

La base OLTP est la source principale des données transactionnelles. Elle stocke les paiements, remboursements, chargebacks, abonnements, clients et marchands.

Elle doit garantir :

- faible latence ;
- propriétés ACID ;
- cohérence des transactions ;
- réplication ;
- failover.

### CDC et Kafka

Le Change Data Capture récupère les changements depuis la base OLTP. Kafka diffuse ensuite ces événements vers les autres systèmes.

Ce mécanisme permet d'alimenter le data warehouse, le NoSQL, le data lake et les services de fraude presque en temps réel.

### Data Warehouse OLAP

Le data warehouse sert aux analyses et rapports. Il contient des tables de faits et des dimensions pour analyser les revenus, transactions, clients, marchands, devises et pays.

Il est optimisé pour :

- les agrégations ;
- les analyses historiques ;
- les tableaux de bord ;
- le reporting de conformité.

### NoSQL

Le système NoSQL stocke les données flexibles :

- logs ;
- sessions utilisateurs ;
- événements clickstream ;
- feedbacks ;
- features machine learning ;
- scores de fraude.

Il permet d'ajouter de nouveaux champs facilement et de traiter des données non structurées.

### Data Lake

Le data lake conserve les données brutes et historiques. Il sert pour l'archivage, les traitements batch et l'entraînement des modèles machine learning.

### Airflow et orchestration

Airflow orchestre les traitements batch et les opérations nécessaires pour faire fonctionner la plateforme data. Il ne remplace pas Kafka pour le temps réel, mais il planifie, supervise et relance les traitements qui préparent les données pour l'OLAP, le ML et le reporting.

Les composants Airflow nécessaires sont :

- `Airflow Scheduler` pour déclencher les DAGs planifiés ;
- `Airflow Webserver` pour suivre les exécutions et diagnostiquer les erreurs ;
- `Airflow Workers` pour exécuter les tâches ;
- `Airflow Metadata DB` pour stocker l'état des DAGs, des runs et des tâches ;
- stockage des DAGs et des logs pour historiser les traitements ;
- connexions sécurisées vers Kafka, le data lake, le warehouse, NoSQL et les services ML.

Airflow orchestre notamment :

- nettoyage ;
- transformation ;
- chargement vers l'OLAP ;
- création de tables agrégées ;
- préparation des datasets machine learning.

### Composants de fonctionnement

Pour que l'architecture soit exploitable en production, elle doit aussi intégrer :

- `Schema Registry` pour versionner les schémas des événements Kafka ;
- `Dead Letter Queue` pour isoler les messages invalides ou non traitables ;
- contrôles de qualité des données avant chargement dans l'OLAP ;
- gestion des secrets pour les mots de passe, clés API et certificats ;
- monitoring et alerting pour suivre Kafka, Airflow, le warehouse et les jobs ML ;
- sauvegardes et restauration pour l'OLTP, l'Airflow Metadata DB et les données critiques ;
- environnement de développement et de staging pour tester les DAGs et les transformations avant production.

## Flux temps réel

1. Une transaction est créée dans l'application Stripe.
2. Elle est enregistrée dans la base OLTP.
3. Le CDC capture le changement.
4. Kafka publie l'événement.
5. Les services NoSQL, OLAP et ML consomment l'événement.
6. Le modèle de fraude peut calculer un score rapidement.

## Flux batch

1. Les données sont stockées dans le data lake.
2. Airflow lance les traitements planifiés.
3. Les tâches exécutent les contrôles de qualité.
4. Les données sont nettoyées et transformées.
5. Le data warehouse est mis à jour.
6. Les tables agrégées et vues matérialisées sont rafraîchies.
7. Les rapports, dashboards et datasets ML utilisent les données préparées.

## Sécurité et conformité

L'architecture doit inclure :

- chiffrement au repos et en transit ;
- contrôle d'accès par rôle ;
- audit logs ;
- masquage ou tokenisation des données sensibles ;
- monitoring des accès ;
- gestion centralisée des secrets ;
- respect de GDPR, PCI-DSS et CCPA.

## Performance et scalabilité

Pour supporter la croissance, l'architecture doit utiliser :

- indexation ;
- partitionnement ;
- réplication ;
- sharding si nécessaire ;
- vues matérialisées ;
- tables agrégées ;
- consumer groups Kafka ;
- supervision Airflow des jobs longs ou critiques.

## Synthèse

L'architecture proposée sépare clairement les usages :

- OLTP pour les transactions ;
- OLAP pour l'analyse ;
- NoSQL pour les données flexibles ;
- Kafka et CDC pour la synchronisation ;
- Data Lake pour l'historique ;
- Airflow pour l'orchestration batch, la qualité et les chargements ;
- ML pour la fraude et la personnalisation.

Cette approche permet de répondre aux besoins principaux de Stripe sans surcharger un seul système.

Elle rend aussi le projet plus maintenable : chaque besoin important dispose d'un composant adapté, et les échanges entre composants restent lisibles.
