# 01 - Business Requirements

## Objectif

Ce projet consiste à concevoir une architecture data pour Stripe qui combine trois types de systèmes :

- OLTP pour les transactions en temps réel ;
- OLAP pour l'analyse et le reporting ;
- NoSQL pour les logs, événements et données semi-structurées.

L'objectif est de garantir des paiements fiables, des analyses rapides, une bonne gestion des données non structurées et le respect des contraintes de sécurité.

Le projet ne cherche pas seulement à stocker des données. Il vise à organiser les bons systèmes autour des bons usages : l'OLTP pour les opérations critiques, l'OLAP pour la décision, et le NoSQL pour les données plus flexibles utilisées par la fraude et le machine learning.

## Contexte

Stripe traite un très grand volume de paiements, remboursements, abonnements et litiges. Ces opérations doivent être rapides, fiables et cohérentes.

En parallèle, Stripe doit analyser ses données pour suivre les revenus, détecter la fraude, comprendre les clients et produire des rapports de conformité.

L'enjeu principal est donc de garder une donnée cohérente entre plusieurs systèmes sans ralentir les paiements. Les traitements analytiques et ML doivent être alimentés rapidement, mais sans faire porter toute la charge à la base transactionnelle.

## Besoins principaux

### Transactions

Le système OLTP doit :

- traiter les paiements avec une faible latence ;
- garantir les propriétés ACID ;
- éviter les doublons et incohérences ;
- supporter la réplication et le failover.

### Analytics

Le système OLAP doit permettre :

- l'analyse des revenus ;
- le suivi des performances des marchands ;
- l'analyse des remboursements et chargebacks ;
- la segmentation client ;
- le reporting de conformité.

### Données NoSQL

Le système NoSQL doit stocker :

- les logs applicatifs ;
- les données de session ;
- les événements clickstream ;
- les feedbacks clients ;
- les features utilisées pour la fraude et le machine learning.

## Données importantes

Les principales données à gérer sont :

- transactions ;
- marchands ;
- clients ;
- moyens de paiement ;
- remboursements ;
- chargebacks ;
- abonnements ;
- logs ;
- scores de fraude ;
- données de référence comme pays, devises et taux de change.

## Contraintes

L'architecture doit respecter les contraintes suivantes :

- haute disponibilité ;
- scalabilité horizontale ;
- synchronisation entre OLTP, OLAP et NoSQL ;
- chiffrement des données ;
- contrôle d'accès ;
- audit logging ;
- conformité GDPR, PCI-DSS et CCPA.

## Cas d'usage prioritaires

1. Traiter un paiement en temps réel.
2. Détecter une transaction suspecte.
3. Calculer le revenu par période, pays ou marchand.
4. Analyser les remboursements et chargebacks.
5. Produire des rapports de conformité.
6. Alimenter des modèles machine learning avec des données fiables.

## Synthèse

Stripe a besoin d'une architecture data capable de séparer les usages transactionnels, analytiques et semi-structurés, tout en gardant les données synchronisées. L'architecture doit être fiable, scalable, sécurisée et adaptée aux besoins de fraude, reporting et machine learning.
