# 08 - Machine Learning Integration

## Objectif

Le machine learning sert principalement a detecter la fraude, segmenter les clients et analyser les feedbacks.

## Cas d'usage

| Cas d'usage | Type | Frequence |
|---|---|---|
| Detection de fraude | Classification | Temps reel |
| Segmentation client | Clustering | Batch quotidien |
| Analyse de sentiment | NLP simple | Quasi temps reel |

## Detection de fraude

Le modele utilise des signaux comme :

- montant de la transaction ;
- pays de l'adresse IP ;
- pays du client ;
- type d'appareil ;
- nombre de transactions recentes ;
- historique de remboursements ou chargebacks.

Le score produit est stocke dans `fraud_features` avec un niveau de risque : `low`, `medium`, `high` ou `critical`.

## Pipeline ML

```text
Transactions + sessions + historique
  -> extraction de features
  -> modele ML
  -> score de fraude
  -> stockage NoSQL
  -> alerte ou revue manuelle
```

## Monitoring

- Suivi du taux de faux positifs.
- Suivi du nombre de transactions bloquees.
- Detection de derive des donnees.
- Reentrainement regulier du modele.

## Synthese

Le ML ne remplace pas l'OLTP. Il enrichit l'architecture avec des scores et predictions stockes dans NoSQL ou exploites par les equipes fraude.
