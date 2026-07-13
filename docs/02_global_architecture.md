# 02 - Global Data Architecture

## Objectif

Ce document presente l'architecture data globale proposee pour Stripe. Elle relie les systemes OLTP, OLAP et NoSQL afin de traiter les transactions, produire des analyses et exploiter les donnees semi-structurees.

## Vue d'ensemble

Flux principal :

```text
Applications Stripe
  -> Base OLTP
  -> CDC / Kafka
  -> Data Lake / NoSQL / Data Warehouse
  -> BI / Reporting / Machine Learning / Fraud Detection
```

Le diagramme correspondant se trouve dans `diagrams/architecture.mmd`.

## Composants principaux

### OLTP

La base OLTP est la source principale des donnees transactionnelles. Elle stocke les paiements, remboursements, chargebacks, abonnements, clients et marchands.

Elle doit garantir :

- faible latence ;
- proprietes ACID ;
- coherence des transactions ;
- replication ;
- failover.

### CDC et Kafka

Le Change Data Capture recupere les changements depuis la base OLTP. Kafka diffuse ensuite ces evenements vers les autres systemes.

Ce mecanisme permet d'alimenter le data warehouse, le NoSQL, le data lake et les services de fraude presque en temps reel.

### Data Warehouse OLAP

Le data warehouse sert aux analyses et rapports. Il contient des tables de faits et des dimensions pour analyser les revenus, transactions, clients, marchands, devises et pays.

Il est optimise pour :

- les agregations ;
- les analyses historiques ;
- les tableaux de bord ;
- le reporting de conformite.

### NoSQL

Le systeme NoSQL stocke les donnees flexibles :

- logs ;
- sessions utilisateurs ;
- evenements clickstream ;
- feedbacks ;
- features machine learning ;
- scores de fraude.

Il permet d'ajouter de nouveaux champs facilement et de traiter des donnees non structurees.

### Data Lake

Le data lake conserve les donnees brutes et historiques. Il sert pour l'archivage, les traitements batch et l'entrainement des modeles machine learning.

### Airflow

Airflow orchestre les traitements batch :

- nettoyage ;
- transformation ;
- chargement vers l'OLAP ;
- creation de tables agregees ;
- preparation des datasets machine learning.

## Flux temps reel

1. Une transaction est creee dans l'application Stripe.
2. Elle est enregistree dans la base OLTP.
3. Le CDC capture le changement.
4. Kafka publie l'evenement.
5. Les services NoSQL, OLAP et ML consomment l'evenement.
6. Le modele de fraude peut calculer un score rapidement.

## Flux batch

1. Les donnees sont stockees dans le data lake.
2. Airflow lance les traitements planifies.
3. Les donnees sont nettoyees et transformees.
4. Le data warehouse est mis a jour.
5. Les rapports et dashboards utilisent les donnees preparees.

## Securite et conformite

L'architecture doit inclure :

- chiffrement au repos et en transit ;
- controle d'acces par role ;
- audit logs ;
- masquage ou tokenisation des donnees sensibles ;
- monitoring des acces ;
- respect de GDPR, PCI-DSS et CCPA.

## Performance et scalabilite

Pour supporter la croissance, l'architecture doit utiliser :

- indexation ;
- partitionnement ;
- replication ;
- sharding si necessaire ;
- vues materialisees ;
- tables agregees ;
- consumer groups Kafka.

## Synthese

L'architecture proposee separe clairement les usages :

- OLTP pour les transactions ;
- OLAP pour l'analyse ;
- NoSQL pour les donnees flexibles ;
- Kafka et CDC pour la synchronisation ;
- Data Lake pour l'historique ;
- ML pour la fraude et la personnalisation.

Cette approche permet de repondre aux besoins principaux de Stripe sans surcharger un seul systeme.
