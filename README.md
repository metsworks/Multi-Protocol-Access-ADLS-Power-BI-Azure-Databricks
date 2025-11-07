# Multi-Protocol-Access-ADLS-Power-BI-Azure-Databricks
* Pour des raisons de sécurité, les captures d'écran et les notebooks qui contiennent les noms de ressources Azure internes ne sont disponibles que sur un repo privé. Veuillez demander l'accès si besoin »

  
![Architecture du projet](docs/architecture.png)


### 🎯 Objectif

Cette implémentation présente une architecture d'accès aux données multiprotocole construite autour d'Azure Data Lake comme couche de stockage fondamentale et d'Azure Databricks comme environnement Data Lakehouse pour le traitement et le stockage des données.

Le workflow montre :

Comment les données stockées dans Azure Data Lake Storage Gen2 (ADLS) peuvent être accessibles via plusieurs protocoles (par exemple, wasb://, abfss://) et intégrées à la fois à Power BI et Azure Databricks.

Comment s'authentifier via une clé de compte et un principal de service (Microsoft Entra ID) à l'aide d'Azure Key Vault pour une gestion sécurisée des secrets et RBAC / ACL pour les autorisations des utilisateurs et des services.

Comment ingérer l'ensemble de données du stockage Blob dans Databricks et l'enregistrer en tant que table Delta dans le catalogue Hive Metastore intégré, fournissant des transactions ACID et des versions dans le cadre du framework Delta Lake.


### 🔧 Services et technologies utilisés

Azure Data Lake Storage Gen2 (ADLS) – Stockage hiérarchique

Azure Blob Storage – Conteneur de fichiers CSV publics (ex : NYPD Arrests dataset)

Power BI Desktop – Connexion via wasbs:// (Blob Storage REST endpoint)

Azure Databricks – Connexion via abfss:// (Data Lake endpoint) avec :

Account Key Authentication

Service Principal Authentication (via Microsoft Entra ID → App Registration)

Key Vault-backed Secret Scope

Azure Key Vault – Gestion sécurisée des secrets (client ID, client secret, account key)

Databricks Secret Scope – Stockage des clés pour l'accès aux ressources

Hive Metastore  – Enregistrement automatique de la table au format Delta

### 🔑 Méthodes d’authentification démontrées
 1. Account Key Authentication (clé d’accès directe du compte de stockage)

Utilisation de la propriété fs.azure.account.key.<STORAGE_ACCOUNT>.dfs.core.windows.net

Accès configuré depuis Databricks via spark.conf.set(...)

 2. Service Principal Authentication (recommandé pour production)

Création d’un App Registration dans Microsoft Entra ID

Attribution des rôles (Storage Blob Data Contributor, Storage Blob Data Owner)

Gestion des permissions via ACL (Access Control Lists) sur les chemins ADLS

### ✅ Résultats obtenus

Fichier CSV chargé depuis ADLS et injecté :

dans Power BI via wasbs://

dans Azure Databricks via abfss://

Table Delta créée dans Databricks et enregistrée dans le Hive Metastore

Accès sécurisé géré avec 2 méthodes d’authentification

Clés et secrets protégés via Azure Key Vault et Databricks Secret Scope
