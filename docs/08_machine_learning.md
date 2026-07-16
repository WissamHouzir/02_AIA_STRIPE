# 08 - Machine Learning Integration

## Objectif

Le machine learning sert principalement à détecter la fraude, segmenter les clients et analyser les feedbacks.

Dans ce projet, le ML est un enrichissement de l'architecture data. Il utilise les données transactionnelles, les sessions et l'historique pour produire des scores utiles aux équipes fraude et aux analyses métier.

## Cas d'usage

| Cas d'usage | Type | Fréquence |
|---|---|---|
| Détection de fraude | Classification | Temps réel |
| Segmentation client | Clustering | Batch quotidien |
| Analyse de sentiment | NLP simple | Quasi temps réel |

## Détection de fraude

Le modèle utilise des signaux comme :

- montant de la transaction ;
- pays de l'adresse IP ;
- pays du client ;
- type d'appareil ;
- nombre de transactions récentes ;
- historique de remboursements ou chargebacks.

Le score produit est stocké dans `fraud_features` avec un niveau de risque : `low`, `medium`, `high` ou `critical`.

## Pipeline ML

```text
Transactions + sessions + historique
  -> extraction de features
  -> modèle ML
  -> score de fraude
  -> stockage NoSQL
  -> alerte ou revue manuelle
```

## Monitoring

- Suivi du taux de faux positifs.
- Suivi du nombre de transactions bloquées.
- Détection de dérive des données.
- Réentraînement régulier du modèle.

## Synthèse

Le ML ne remplace pas l'OLTP. Il enrichit l'architecture avec des scores et prédictions stockés dans NoSQL ou exploités par les équipes fraude.

Cette séparation permet de faire évoluer les modèles sans modifier le cœur transactionnel du système.
