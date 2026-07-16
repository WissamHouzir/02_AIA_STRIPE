# Glossaire — Projet AIA Stripe

Ce glossaire sert de référence pour le projet d'architecture data Stripe. Il explique les sigles, concepts et outils utilisés dans les documents `docs/`, le diagramme `diagrams/architecture.mmd` et les modèles OLTP, OLAP et NoSQL.

---

## 1. Architectures de données

| Terme | Signification | Explication dans le projet |
|---|---|---|
| **OLTP** | Online Transaction Processing | Système transactionnel utilisé pour enregistrer les paiements, remboursements, chargebacks, abonnements, clients et marchands. Il privilégie la faible latence, l'intégrité et les propriétés ACID. |
| **OLAP** | Online Analytical Processing | Système analytique utilisé pour le reporting, l'analyse des revenus, la fraude, la segmentation client et la conformité. Il privilégie les lectures rapides et les agrégations. |
| **NoSQL** | Not Only SQL | Base non relationnelle utilisée pour les données flexibles : logs, sessions, événements clickstream, feedbacks clients et scores de fraude. |
| **Data Lake** | Lac de données | Zone de stockage des données brutes et historiques. Il permet l'archivage, le rejeu, les traitements batch et l'entraînement des modèles ML. |
| **Data Warehouse** | Entrepôt de données | Base analytique structurée pour les tables de faits, dimensions, agrégats et vues matérialisées. |
| **Architecture data** | Architecture de données | Organisation globale des systèmes qui collectent, transportent, stockent, transforment, sécurisent et exploitent les données. |
| **Temps réel** | Real time | Traitement rapide des événements dès leur création, par exemple pour scorer une transaction suspecte. |
| **Batch** | Traitement par lots | Traitement planifié de volumes de données, par exemple pour construire les agrégats quotidiens ou les datasets ML. |

---

## 2. Modélisation des données

| Terme | Signification | Explication dans le projet |
|---|---|---|
| **ERD** | Entity-Relationship Diagram | Diagramme qui représente les tables OLTP, leurs colonnes et leurs relations. |
| **DBML** | Database Markup Language | Langage utilisé pour décrire des modèles de base de données et générer des diagrammes avec dbdiagram.io. |
| **Clé primaire** | Primary Key | Identifiant unique d'une ligne, par exemple `transaction_id` dans la table des transactions. |
| **Clé étrangère** | Foreign Key | Colonne qui référence une autre table, par exemple une transaction reliée à un client et à un marchand. |
| **Normalisation** | Normalization | Organisation des tables OLTP pour éviter les doublons et garder des relations cohérentes. |
| **Schéma en étoile** | Star Schema | Modèle OLAP retenu dans le projet : une table de faits centrale reliée à plusieurs dimensions. |
| **Table de faits** | Fact Table | Table centrale OLAP contenant les mesures à analyser. Dans le projet : `fact_transactions`. |
| **Dimension** | Dimension Table | Table descriptive qui donne le contexte d'analyse : date, client, marchand, devise, pays, moyen de paiement. |
| **Pré-agrégation** | Pre-aggregation | Table calculée à l'avance pour accélérer les requêtes fréquentes, comme `agg_daily_revenue`. |
| **Vue matérialisée** | Materialized View | Vue dont le résultat est stocké physiquement et rafraîchi régulièrement pour accélérer le reporting. |
| **Embedding** | Imbrication NoSQL | Stratégie MongoDB qui consiste à stocker des sous-objets dans le même document, par exemple les features dans `fraud_features`. |
| **Referencing** | Référencement NoSQL | Stratégie MongoDB qui consiste à stocker des identifiants comme `transaction_id`, `customer_id` ou `merchant_id` pour relier NoSQL aux autres systèmes. |
| **Index** | Index de base de données | Structure qui accélère les recherches sur des champs comme `transaction_id`, `merchant_id`, `created_at` ou `risk_level`. |

---

## 3. Pipeline et intégration

| Terme | Signification | Explication dans le projet |
|---|---|---|
| **ETL** | Extract, Transform, Load | Approche où les données sont extraites, transformées puis chargées dans la cible. |
| **ELT** | Extract, Load, Transform | Approche où les données sont chargées d'abord puis transformées dans l'entrepôt ou la plateforme analytique. |
| **CDC** | Change Data Capture | Capture des changements de la base OLTP pour les envoyer vers Kafka, NoSQL, Data Lake et OLAP. |
| **Debezium** | Outil CDC | Outil qui lit les changements PostgreSQL et les publie sous forme d'événements. |
| **Kafka** | Plateforme de streaming | Bus d'événements qui transporte les changements de données entre les systèmes. |
| **Topic Kafka** | Canal Kafka | Canal logique qui reçoit un type d'événement, par exemple les transactions ou les remboursements. |
| **Consumer group** | Groupe de consommateurs | Groupe de services qui lisent les messages Kafka en parallèle pour scaler la consommation. |
| **Schema Registry** | Registre de schémas | Composant qui versionne et valide les schémas des événements Kafka pour éviter les incompatibilités. |
| **Dead Letter Queue** | File d'erreurs | File où sont envoyés les messages invalides ou impossibles à traiter, afin de ne pas bloquer le pipeline. |
| **Airflow** | Orchestrateur de workflows | Outil qui planifie, déclenche, surveille et relance les traitements batch du projet. |
| **DAG Airflow** | Directed Acyclic Graph | Workflow Airflow composé de tâches ordonnées. Exemples : `build_fact_transactions`, `refresh_aggregates`, `prepare_ml_features`. |
| **Airflow Scheduler** | Planificateur Airflow | Composant qui déclenche les DAGs selon un calendrier ou une dépendance. |
| **Airflow Webserver** | Interface Airflow | Interface web utilisée pour suivre les exécutions, diagnostiquer les erreurs et relancer des tâches. |
| **Airflow Workers** | Exécuteurs Airflow | Composants qui exécutent réellement les tâches des DAGs. |
| **Airflow Metadata DB** | Base de métadonnées Airflow | Base qui stocke l'état des DAGs, tâches, exécutions, retries et logs d'orchestration. |
| **Data Quality Checks** | Contrôles de qualité | Tests sur les volumes, schémas, doublons, valeurs nulles et cohérence avant chargement analytique. |
| **Retry** | Nouvelle tentative | Relance automatique d'une tâche Airflow ou d'un traitement après un échec temporaire. |
| **Déduplication** | Suppression des doublons | Mécanisme qui évite d'insérer plusieurs fois la même transaction, souvent via `transaction_id`. |
| **Idempotence** | Exécution répétable sans effet indésirable | Propriété importante pour rejouer un message ou une tâche sans créer de doublon. |

---

## 4. Sécurité et conformité

| Terme | Signification | Explication dans le projet |
|---|---|---|
| **ACID** | Atomicity, Consistency, Isolation, Durability | Propriétés attendues de l'OLTP pour garantir qu'un paiement est traité de façon fiable. |
| **Atomicité** | Atomicity | Une transaction est entièrement appliquée ou annulée. |
| **Cohérence** | Consistency | La base passe d'un état valide à un autre état valide. |
| **Isolation** | Isolation | Les transactions concurrentes ne se perturbent pas. |
| **Durabilité** | Durability | Une transaction validée reste enregistrée même après une panne. |
| **GDPR / RGPD** | Règlement européen sur les données personnelles | Cadre de conformité pour les données personnelles des clients. |
| **PCI-DSS** | Payment Card Industry Data Security Standard | Norme de sécurité liée aux données de cartes de paiement. |
| **CCPA** | California Consumer Privacy Act | Réglementation californienne sur la protection des données personnelles. |
| **RBAC** | Role-Based Access Control | Contrôle d'accès basé sur les rôles : analyste, équipe fraude, administrateur, service technique. |
| **MFA** | Multi-Factor Authentication | Authentification multi-facteur pour réduire les accès non autorisés. |
| **Chiffrement au repos** | Encryption at rest | Protection des données stockées dans les bases, le data lake et les sauvegardes. |
| **Chiffrement en transit** | Encryption in transit | Protection des flux réseau avec TLS entre applications, bases, Kafka et services. |
| **Tokenisation** | Tokenization | Remplacement d'un numéro de carte ou d'une donnée sensible par un jeton non sensible. |
| **Pseudonymisation** | Pseudonymization | Remplacement des données identifiantes par des pseudonymes pour limiter l'exposition. |
| **Audit log** | Journal d'audit | Trace des actions sensibles : qui a consulté ou modifié quoi, quand et depuis quel service. |
| **Secrets Manager** | Gestionnaire de secrets | Composant qui stocke les mots de passe, clés API et certificats utilisés par Airflow, Kafka, CDC et les bases. |
| **Monitoring des accès** | Access monitoring | Surveillance des accès aux données sensibles et détection d'anomalies. |

---

## 5. Machine learning et fraude

| Terme | Signification | Explication dans le projet |
|---|---|---|
| **Machine learning** | Apprentissage automatique | Utilisé pour détecter la fraude, segmenter les clients et analyser les feedbacks. |
| **Feature** | Variable d'entrée | Information utilisée par un modèle ML, par exemple montant, pays IP, appareil ou historique client. |
| **Feature engineering** | Ingénierie des variables | Création et transformation des données brutes en variables utiles pour les modèles. |
| **Score d'anomalie** | Anomaly score | Score indiquant le niveau de risque d'une transaction. |
| **Risk level** | Niveau de risque | Classement du risque : `low`, `medium`, `high` ou `critical`. |
| **Fraud detection** | Détection de fraude | Processus qui identifie les transactions suspectes en quasi temps réel. |
| **Segmentation client** | Customer segmentation | Regroupement des clients selon leurs comportements, volumes ou profils. |
| **Précision** | Precision | Part des transactions signalées comme frauduleuses qui sont réellement frauduleuses. |
| **Rappel** | Recall | Part des fraudes réelles effectivement détectées. |
| **Faux positif** | False positive | Transaction légitime signalée à tort comme suspecte. |
| **Data drift** | Dérive des données | Changement de distribution des données qui peut dégrader les performances du modèle. |
| **Réentraînement** | Retraining | Mise à jour régulière d'un modèle avec des données plus récentes. |
| **Revue manuelle** | Manual review | Étape où une équipe fraude vérifie les transactions à haut risque. |

---

## 6. Technologies du projet

| Outil | Catégorie | Rôle dans le projet |
|---|---|---|
| **PostgreSQL** | Base relationnelle | Moteur OLTP pour les transactions, clients, marchands, remboursements et abonnements. |
| **Debezium** | CDC | Capture les changements PostgreSQL et les publie vers Kafka. |
| **Apache Kafka** | Streaming | Transporte les événements entre OLTP, NoSQL, Data Lake, OLAP et ML. |
| **Schema Registry** | Gouvernance de schéma | Contrôle la compatibilité des messages Kafka. |
| **MongoDB** | Base NoSQL documentaire | Stocke les documents flexibles : sessions, logs, feedbacks et `fraud_features`. |
| **Data Lake** | Stockage brut | Conserve les données historiques et rejouables. |
| **Airflow** | Orchestration | Planifie et supervise les traitements batch, les contrôles qualité et les chargements OLAP. |
| **Data Warehouse OLAP** | Entrepôt analytique | Héberge `fact_transactions`, les dimensions et les agrégats. |
| **BI / Reporting** | Analyse métier | Permet aux équipes de suivre revenus, fraude, marchands, remboursements et conformité. |
| **Monitoring et alerting** | Observabilité | Suit les retards Kafka, échecs Airflow, performances warehouse et incidents de pipeline. |
| **dbdiagram.io** | Modélisation | Génère les diagrammes de bases à partir des fichiers DBML. |
| **Mermaid** | Diagramme textuel | Décrit le diagramme global d'architecture dans `diagrams/architecture.mmd`. |

---

## 7. Concepts transversaux

| Terme | Signification | Explication dans le projet |
|---|---|---|
| **Haute disponibilité** | High Availability | Capacité à continuer de fonctionner malgré une panne grâce à la réplication et au failover. |
| **Failover** | Basculement | Passage automatique vers une instance secondaire en cas de panne. |
| **Scalabilité horizontale** | Horizontal scaling | Ajout de machines ou de partitions pour absorber la croissance du volume de données. |
| **Sharding** | Partitionnement horizontal | Répartition d'une base en fragments distribués pour supporter plus de charge. |
| **Partitionnement** | Partitioning | Division d'une table ou d'un topic selon un critère, par exemple la date ou le marchand. |
| **Cohérence éventuelle** | Eventual consistency | Modèle où les données finissent par converger entre systèmes, acceptable pour certains usages NoSQL ou analytiques. |
| **Réconciliation** | Reconciliation | Comparaison entre OLTP et OLAP pour détecter les écarts de données. |
| **Latence** | Latency | Délai entre la création d'un événement et sa disponibilité dans un autre système. |
| **Observabilité** | Observability | Capacité à comprendre l'état du système grâce aux logs, métriques, traces et alertes. |
| **SLA** | Service Level Agreement | Engagement de niveau de service, par exemple disponibilité ou temps de traitement maximal. |

---

## 8. Termes métier Stripe

| Terme | Explication dans le projet |
|---|---|
| **Transaction** | Opération financière de base, généralement liée à un paiement. |
| **Paiement** | Exécution concrète d'une transaction via un moyen de paiement. |
| **Remboursement** | Retour total ou partiel d'un montant au client. |
| **Chargeback** | Contestation bancaire ou litige déclenché après une transaction. |
| **Marchand** | Entreprise qui utilise Stripe pour encaisser des paiements. |
| **Client** | Utilisateur final qui paie un marchand. |
| **Moyen de paiement** | Carte, portefeuille ou autre méthode utilisée pour payer. |
| **Abonnement** | Paiement récurrent géré pour un service ou produit périodique. |
| **Devise** | Monnaie utilisée dans la transaction, par exemple EUR ou USD. |
| **Taux de change** | Conversion utilisée pour comparer les montants entre devises. |
| **Conformité** | Ensemble des règles et contrôles nécessaires pour respecter GDPR, PCI-DSS et CCPA. |
