# 01 - Business Requirements

## Objectif

Ce projet consiste a concevoir une architecture data pour Stripe qui combine trois types de systemes :

- OLTP pour les transactions en temps reel ;
- OLAP pour l'analyse et le reporting ;
- NoSQL pour les logs, evenements et donnees semi-structurees.

L'objectif est de garantir des paiements fiables, des analyses rapides, une bonne gestion des donnees non structurees et le respect des contraintes de securite.

## Contexte

Stripe traite un tres grand volume de paiements, remboursements, abonnements et litiges. Ces operations doivent etre rapides, fiables et coherentes.

En parallele, Stripe doit analyser ses donnees pour suivre les revenus, detecter la fraude, comprendre les clients et produire des rapports de conformite.

## Besoins principaux

### Transactions

Le systeme OLTP doit :

- traiter les paiements avec une faible latence ;
- garantir les proprietes ACID ;
- eviter les doublons et incoherences ;
- supporter la replication et le failover.

### Analytics

Le systeme OLAP doit permettre :

- l'analyse des revenus ;
- le suivi des performances des marchands ;
- l'analyse des remboursements et chargebacks ;
- la segmentation client ;
- le reporting de conformite.

### Donnees NoSQL

Le systeme NoSQL doit stocker :

- les logs applicatifs ;
- les donnees de session ;
- les evenements clickstream ;
- les feedbacks clients ;
- les features utilisees pour la fraude et le machine learning.

## Donnees importantes

Les principales donnees a gerer sont :

- transactions ;
- marchands ;
- clients ;
- moyens de paiement ;
- remboursements ;
- chargebacks ;
- abonnements ;
- logs ;
- scores de fraude ;
- donnees de reference comme pays, devises et taux de change.

## Contraintes

L'architecture doit respecter les contraintes suivantes :

- haute disponibilite ;
- scalabilite horizontale ;
- synchronisation entre OLTP, OLAP et NoSQL ;
- chiffrement des donnees ;
- controle d'acces ;
- audit logging ;
- conformite GDPR, PCI-DSS et CCPA.

## Cas d'usage prioritaires

1. Traiter un paiement en temps reel.
2. Detecter une transaction suspecte.
3. Calculer le revenu par periode, pays ou marchand.
4. Analyser les remboursements et chargebacks.
5. Produire des rapports de conformite.
6. Alimenter des modeles machine learning avec des donnees fiables.

## Synthese

Stripe a besoin d'une architecture data capable de separer les usages transactionnels, analytiques et semi-structures, tout en gardant les donnees synchronisees. L'architecture doit etre fiable, scalable, securisee et adaptee aux besoins de fraude, reporting et machine learning.
