# 07 - Security and Compliance Plan

## Objectif

Stripe manipule des données financières et personnelles. L'architecture doit donc protéger les données et respecter les exigences GDPR, PCI-DSS et CCPA.

## Mesures principales

| Risque | Mesure |
|---|---|
| Fuite de données | Chiffrement au repos et en transit |
| Accès non autorisé | RBAC et MFA |
| Exposition des cartes | Tokenisation des moyens de paiement |
| Modification non tracée | Audit logs |
| Données personnelles trop visibles | Masquage ou pseudonymisation |
| Perte de données | Sauvegardes et plan de reprise |

## Accès aux données

- Les analystes accèdent surtout à l'OLAP.
- Les données sensibles restent limitées dans l'OLTP.
- Les services techniques utilisent des comptes de service avec droits minimaux.
- Les accès aux données sensibles sont journalisés.

## Conformité

### GDPR / CCPA

- Minimiser les données stockées.
- Pseudonymiser les données personnelles dans l'analytics.
- Permettre l'export ou la suppression logique des données client.
- Définir des durées de rétention.

### PCI-DSS

- Ne pas stocker les numéros de carte en clair.
- Utiliser la tokenisation.
- Chiffrer les flux sensibles.
- Conserver des logs d'audit.
- Contrôler strictement les accès.

## Synthèse

La sécurité doit être présente dans chaque couche : OLTP, pipeline, NoSQL, OLAP et outils BI.
