# 07 - Security and Compliance Plan

## Objectif

Stripe manipule des donnees financieres et personnelles. L'architecture doit donc proteger les donnees et respecter les exigences GDPR, PCI-DSS et CCPA.

## Mesures principales

| Risque | Mesure |
|---|---|
| Fuite de donnees | Chiffrement au repos et en transit |
| Acces non autorise | RBAC et MFA |
| Exposition des cartes | Tokenisation des moyens de paiement |
| Modification non tracee | Audit logs |
| Donnees personnelles trop visibles | Masquage ou pseudonymisation |
| Perte de donnees | Sauvegardes et plan de reprise |

## Acces aux donnees

- Les analystes accedent surtout a l'OLAP.
- Les donnees sensibles restent limitees dans l'OLTP.
- Les services techniques utilisent des comptes de service avec droits minimaux.
- Les acces aux donnees sensibles sont journalises.

## Conformite

### GDPR / CCPA

- Minimiser les donnees stockees.
- Pseudonymiser les donnees personnelles dans l'analytics.
- Permettre l'export ou la suppression logique des donnees client.
- Definir des durees de retention.

### PCI-DSS

- Ne pas stocker les numeros de carte en clair.
- Utiliser la tokenisation.
- Chiffrer les flux sensibles.
- Conserver des logs d'audit.
- Controler strictement les acces.

## Synthese

La securite doit etre presente dans chaque couche : OLTP, pipeline, NoSQL, OLAP et outils BI.
