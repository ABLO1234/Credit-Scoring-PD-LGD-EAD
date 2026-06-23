## 📌 À propos du projet

Ce dépôt héberge un pipeline industriel et reproductible de **modélisation du risque de crédit** appliqué au dataset historique de **LendingClub (2007–2019)**, englobant un portefeuille de **2,26 millions de prêts**. 

L'objectif principal est de concevoir un framework quantitatif robuste pour l'estimation conjointe des trois paramètres réglementaires des accords de **Bâle II/III** et de la norme **IFRS 9** :
*   **PD (Probability of Default)** : Modélisation par gradient boosting (LightGBM) calibré, intégrant la gestion du déséquilibre des classes (SMOTE + Undersampling).
*   **LGD (Loss Given Default)** : Approche *Two-Stage* combinant classification et régression logit pour capturer les distributions bimodales des taux de recouvrement.
*   **EAD (Exposure at Default)** : Estimation du facteur de conversion du crédit (*CCF Dynamique*) par régression.
*   **Calcul ECL (Expected Credit Loss)** : Moteur de staging automatique basé sur l'augmentation significative du risque de crédit (Règles SICR) pour la classification en Stage 1, 2 et 3 et projection *lifetime*.

### 🎯 Points clés & Rigueur Méthodologique

*   **Data Engineering & Optimisation Mémoire** : Réduction de ~50% de l'empreinte mémoire numérique (Float64 → Float32) permettant le traitement fluide de millions de lignes sur des infrastructures limitées (Google Colab).
*   **Exclusion du Data Leakage** : Audit qualité strict par l'analyse du *Weight of Evidence (WoE)* et de l'*Information Value (IV)*, menant à l'exclusion systématique de 89 variables contaminantes post-octroi (ex: encours résiduels, montants récupérés).
*   **Sélection de Features Économe** : Réduction drastique de 94 variables initiales à **5 features clés hautement prédictives** via une scorecard consolidée (IV + Effet bivarié + Pénalité de multicolinéarité VIF), garantissant un modèle parcimonieux et facilement auditable (Gouvernance MRM).
*   **Moteur de Monitoring intégré** : Calcul du *Population Stability Index (PSI)* et détection de dérive des données (*Drift*) pour le suivi de la performance des modèles dans le temps.
