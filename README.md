# Modélisation du Risque de Crédit : Estimation Conjointe PD, LGD, EAD et Calcul ECL IFRS 9 (pp. 1-2)

Ce projet présente une modélisation de bout en bout du risque de crédit appliquée aux données historiques de LendingClub (2007–2019), couvrant un portefeuille de 2,26 millions de prêts (p. 2). 

L'objectif est d'estimer conjointement les trois paramètres fondamentaux requis par les cadres réglementaires Bâle II/III et IFRS 9 : la probabilité de défaut (PD), la perte en cas de défaut (LGD) et l'exposition au moment du défaut (EAD), afin d'aboutir au calcul de la perte attendue (ECL) (p. 2).

Le pipeline a été entièrement optimisé pour s'exécuter sous contrainte de ressources computationnelles limitées (p. 2).

# Architecture du ProjetLe projet s'articule autour de trois grands modules successifs :

**1. Data Engineering et Feature StoreLe premier maillon de la chaîne prend en charge la préparation rigoureuse des données :**

* Ingestion et audit de qualité avec typage optimisé (réduction de l'empreinte mémoire de 50% sur les variables numériques en passant en Float32) (pp. 2, 5);

* Détection et traitement des valeurs aberrantes par une méthode IQR adaptative sur les variables financières (pp. 2, 11) ;

* Imputation supervisée et différenciée selon la distribution et le taux de valeurs manquantes (Seuil de suppression à 30%, création de la catégorie 'Unknown' pour les variables qualitatives) (pp. 2, 8, 10).Création de la variable cible PD conforme aux critères de Bâle II (90 jours de retard / DPD) à l'aide de proxies adaptés(pp. 2, 12) ;
  
* Feature Engineering avec la création de 30 variables enrichies (ratios de levier, capacité de remboursement, indicateurs de stress sur le crédit renouvelable) (pp. 2, 13, 18) ;

* Exclusion stricte des variables contaminantes pour éliminer tout risque de fuite de données (data leakage) (pp. 2, 7).
**2. Analyse Exploratoire et Sélection StatistiqueCette étape valide statistiquement les données avant la phase d'apprentissage :**

Analyse univariée complète comprenant des tests de normalité (D'Agostino-Pearson) et d'homogénéité des variances (Levene) (pp. 18-19).Analyse bivariée robuste via des tests d'association adaptés à la nature des distributions (T-test de Welch ou Mann-Whitney U, calcul des tailles d'effet et statistiques KS) (pp. 18, 27-28).

Tri par Weight of Evidence (WoE) et Information Value (IV) pour identifier le pouvoir prédictif. Cette analyse a permis de classer comme suspectes et d'exclure les variables post-octroi (ex: last_pymnt_amnt, out_prncp) qui auraient causé une fraude statistique (pp. 18, 24, 38).

Détection de la multicolinéarité sévère à l'aide du facteur d'inflation de la variance (VIF) et des matrices de corrélation (Pearson/Spearman) (pp. 18, 32, 35).

Scorecard consolidée pour ne retenir que les fonctionnalités les plus robustes et économes pour le modèle final (pp. 18, 36, 38).3. Modélisation Prédictive et Calcul de l'ECLL'architecture de calcul repose sur la formule standardisée : ECL = PD × LGD × EAD × DF (p. 39).

Modèle PD : Implémentation d'un classifieur LightGBM calibré (p. 39). La gestion du déséquilibre des classes (taux de défaut à 12,58%) est traitée par une stratégie combinée de SMOTE et d'undersampling (pp. 18, 40).

Modèle LGD : Approche de modélisation en deux étapes (Two-Stage) combinant une régression logit et un modèle LightGBM (p. 39).Modèle EAD : Estimation du facteur de conversion du crédit (CCF) dynamique par régression (p. 39).

Moteur IFRS 9 : Règles de staging basées sur l'augmentation significative du risque de crédit (SICR) pour classer les encours en Stage 1, Stage 2 ou Stage 3, suivies d'une projection des pertes sur la durée de vie (Lifetime projection) (p. 39).

Suivi et Monitoring : Intégration du calcul du Population Stability Index (PSI) et d'algorithmes de détection de dérive (drift) des données (p. 39).
Technologies Utilisées Langage : Python (p. 3) 
Manipulation de données : Pandas, NumPy (p. 3)
Analyse statistique : SciPy (Stats) (pp. 3, 19)
Machine Learning : Scikit-Learn, LightGBM, Imbalanced-Learn (pp. 39-40)
Visualisation : Matplotlib, Seaborn (pp. 3, 40)
Environnement : Google Colab / Google Drive (pp. 3-4)
Structure des Fichiers de DonnéesLe projet est configuré pour gérer les données de manière itérative à travers les répertoires suivants :DATA/RAW/ : Contient le jeu de données brut initial (loan.csv) (p. 4).DATA/INTERIM/ : Stocke les fichiers de transition nettoyés et exportés au format compressé Parquet (ex: loan_pd_features_VERSION.parquet) ainsi que la liste des variables validées (pp. 4, 17, 38).REPORTS/ : Centralise l'ensemble des figures d'analyse, matrices de corrélation et rapports de performance générés au cours du pipeline (pp. 4, 6, 38).AuteurAbdoulaye Tangara – Fintech Product Manager & Étudiant en MSc in Quantitative and Computable Economics (p. 1).Pour toute question ou suggestion concernant ce pipeline de modélisation, vous pouvez consulter les notebooks associés ou contacter directement l'auteur via l'adresse e-mail ou le profil LinkedIn spécifiés dans la documentation du projet (p. 1).
