# 運用化

このディレクトリでは、異機種混在環境(Spark、GPUなど)で開発されたレコメンデーション システムがどのように運用可能かを示すノートブックを提供しています。

| ノートブック | 詳細 | 
| --- | --- | 
| [als_movie_o16n](als_movie_o16n.ipynb) | エンド ツー エンドの例では、[Databricks](https://azure.microsoft.com/ja-jp/services/databricks/)、[Cosmos DB](https://docs.microsoft.com/ja-jp/azure/cosmos-db/introduction)、[Kubernetes Services](https://azure.microsoft.com/ja-jp/services/kubernetes-service/) サービスなどの Azure サービスを使用して、Spark ALS ベースの映画レコメンダーを構築、評価、およびデプロイする方法を示します。

## ワークフロー

次の図は、レコメンデーション システム開発におけるワークフローの研究者/開発者に役立つベスト プラクティスの例を示しています。

![workflow](https://recodatasets.blob.core.windows.net/images/reco_workflow.png)

## リファレンス アーキテクチャ

スケーラブルなデータ ストレージ ([Azure Cosmos DB](https://docs.microsoft.com/ja-jp/azure/cosmos-db/introduction))、モデル開発 ([Azure Databricks](https://azure.microsoft.com/ja-jp/services/databricks/)、[Azure Data Science Virtual Machine](https://azure.microsoft.com/ja-jp/services/virtual-machines/data-science-virtual-machines/) (DSVM)、[Azure Machine Learning service](https://azure.microsoft.com/ja-jp/services/machine-learning-service/)、およびモデルの運用化 ([Azure Kubernetes Services](https://azure.microsoft.com/ja-jp/services/kubernetes-service/) (AKS)) には、いくつかの Azure サービスが推奨されます。

![architecture](https://recodatasets.blob.core.windows.net/images/reco-arch.png)