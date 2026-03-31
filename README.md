# PySpark Lakehouse : Architecture Medallion sur Databricks

## 📌 Contexte du Projet
Ce projet démontre la création d'un pipeline de traitement de données distribué (Big Data) utilisant **PySpark** sur l'environnement **Databricks (Serverless)**. L'objectif est de traiter les logs transactionnels d'une boutique en ligne (Online Retail) en appliquant le standard de l'industrie : l'architecture Medallion.

## 🛠️ Architecture & Technologies
* **Environnement de calcul :** Databricks (Compute Serverless)
* **Moteur de traitement :** Apache Spark (PySpark)
* **Format de stockage :** Delta Lake

## ⚙️ Déroulement du Pipeline (Architecture Medallion)

### 🥉 Couche Bronze (Ingestion Brute)
* Lecture des données brutes au format CSV (milliers de lignes transactionnelles).
* Sauvegarde immédiate au format **Delta** (`bronze_online_retail`) pour garantir un historique immuable (Data Lake).

### 🥈 Couche Silver (Nettoyage & Enrichissement)
* **Data Quality :** Filtrage des valeurs aberrantes (quantités et prix négatifs) et suppression des valeurs nulles (traitement des `CustomerID` manquants) via `dropna()` et `filter()`.
* **Feature Engineering :** Création de nouvelles colonnes métiers (ex: `TotalAmount`) calculées à la volée avec `withColumn()`.
* Sauvegarde au format Delta (`silver_online_retail`).

### 🥇 Couche Gold (Agrégation Métier pour la BI)
* Agrégation des données nettoyées pour répondre aux besoins analytiques (Business Intelligence).
* Utilisation des fonctions PySpark (`groupBy`, `agg`, `sum`, `countDistinct`) pour générer les KPIs (Revenu total et nombre de clients uniques par pays).
* Sauvegarde finale de la table métier (`gold_sales_by_country`) optimisée pour les requêtes SQL des analystes.
