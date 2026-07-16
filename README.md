# Stripe Data Architecture Project

## Objectif

Ce projet consiste à concevoir une architecture data complète pour Stripe, capable d'intégrer des systèmes OLTP, OLAP et NoSQL.

Le sens du projet est de montrer comment une plateforme de paiement peut séparer ses usages critiques : enregistrer les transactions sans erreur, analyser l'activité commerciale, détecter la fraude et respecter les obligations de conformité. L'architecture proposée cherche donc un équilibre entre fiabilité opérationnelle, exploitation analytique et sécurité des données.

L'architecture doit répondre aux besoins suivants :

- traiter un grand volume de transactions financières avec une faible latence ;
- garantir l'intégrité transactionnelle et les propriétés ACID ;
- fournir des analyses avancées et quasi temps réel ;
- stocker et exploiter des données non structurées et semi-structurées ;
- orchestrer les traitements batch avec Airflow et des contrôles de qualité ;
- supporter des cas d'usage de machine learning comme la détection de fraude ;
- respecter les exigences de sécurité et de conformité comme GDPR et PCI-DSS.

## Structure du projet

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
- `diagrams/oltp_erd.mmd` : ERD du système OLTP.
- `diagrams/olap_schema.mmd` : schéma OLAP.
- `diagrams/architecture_dbdiagram.dbml` : architecture globale compatible dbdiagram.
- `diagrams/oltp_dbdiagram.dbml` : modèle OLTP compatible dbdiagram.
- `diagrams/oltp_simple_dbdiagram.dbml` : modèle OLTP simplifié compatible dbdiagram.
- `diagrams/olap_dbdiagram.dbml` : modèle OLAP compatible dbdiagram.

## Livrables attendus

1. Diagramme global d'architecture data.
2. ERD pour le système OLTP.
3. Schéma OLAP détaillé.
4. Modèle NoSQL détaillé.
5. Document d'architecture des pipelines data avec Airflow, monitoring et gestion des erreurs.
6. Plan sécurité et conformité.
7. Stratégie d'intégration machine learning.
8. Requêtes SQL et NoSQL démonstratives.
