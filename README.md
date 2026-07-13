# Stripe Data Architecture Project

## Objectif

Ce projet consiste a concevoir une architecture data complete pour Stripe, capable d'integrer des systemes OLTP, OLAP et NoSQL.

L'architecture doit repondre aux besoins suivants :

- traiter un grand volume de transactions financieres avec une faible latence ;
- garantir l'integrite transactionnelle et les proprietes ACID ;
- fournir des analyses avancees et quasi temps reel ;
- stocker et exploiter des donnees non structurees et semi-structurees ;
- supporter des cas d'usage de machine learning comme la detection de fraude ;
- respecter les exigences de securite et de conformite comme GDPR et PCI-DSS.

## Structure du projet

- `docs/01_business_requirements.md` : analyse du besoin metier.
- `docs/02_global_architecture.md` : architecture data globale.
- `docs/03_oltp_model.md` : modele transactionnel OLTP.
- `docs/04_olap_model.md` : modele analytique OLAP.
- `docs/05_nosql_model.md` : modele NoSQL.
- `docs/06_data_pipeline.md` : architecture des pipelines data.
- `docs/07_security_compliance.md` : securite et conformite.
- `docs/08_machine_learning.md` : integration machine learning.
- `docs/09_queries.md` : exemples de requetes SQL et NoSQL.
- `diagrams/architecture.mmd` : diagramme global d'architecture.
- `diagrams/oltp_erd.mmd` : ERD du systeme OLTP.
- `diagrams/olap_schema.mmd` : schema OLAP.
- `diagrams/architecture_dbdiagram.dbml` : architecture globale compatible dbdiagram.
- `diagrams/oltp_dbdiagram.dbml` : modele OLTP compatible dbdiagram.
- `diagrams/oltp_simple_dbdiagram.dbml` : modele OLTP simplifie compatible dbdiagram.
- `diagrams/olap_dbdiagram.dbml` : modele OLAP compatible dbdiagram.

## Livrables attendus

1. Diagramme global d'architecture data.
2. ERD pour le systeme OLTP.
3. Schema OLAP detaille.
4. Modele NoSQL detaille.
5. Document d'architecture des pipelines data.
6. Plan securite et conformite.
7. Strategie d'integration machine learning.
8. Requetes SQL et NoSQL demonstratives.
