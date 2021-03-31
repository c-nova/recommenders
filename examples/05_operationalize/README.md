# 運用化

このディレクトリには、異機種環境(Spark、GPUなど)で開発されたレコメンデーション システムがどのように運用化できるかを示すノートブックを用意しております。

| Notebook | 説明 | 
| --- | --- | 
| [als_movie_o16n](als_movie_o16n.ipynb) | このエンド ツー エンドの例では、[Databricks](https://azure.microsoft.com/en-us/services/databricks/)、[Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction)、[Kubernetes Services](https://azure.microsoft.com/en-us/services/kubernetes-service/)などの Azure サービスを使用して Spark ALS ベースの映画のレコメンデーションを構築、評価、デプロイする方法を示します。|
| [aks_locust_load_test](aks_locust_load_test.ipynb) | AKS クラスターに展開されたレコメンデーション システムの負荷テストの例 | 
| [lightgbm_criteo_o16n](lightgbm_criteo_o16n.ipynb) | 広告クリック予測シナリオにおける、コンテンツベースのパーソナライズ化システムの展開 |
