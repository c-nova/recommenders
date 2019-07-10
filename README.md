# Recommenders

このリポジトリはレコメンデーション システムを構築するにあたってのサンプルコードとベストプラクティスを Jupyter ノートブック形式で提供します。私たちが提供するサンプルコードの詳細は、5 つの主要なタスクで構成されます:

- [データの準備](notebooks/01_prepare_data/README.md): それぞれのレコメンド アルゴリズムにデータを準備し、読み込みます
- [モデル](notebooks/02_model/README.md): 古典的またはディープラーニングを使用したレコメンド アルゴリズムを使用してモデルを構築します。アルゴリズムには Alternating Least Squares ([ALS](https://spark.apache.org/docs/latest/api/python/_modules/pyspark/ml/recommendation.html#ALS)) や eXtreme Deep Factorization Machines ([xDeepFM](https://arxiv.org/abs/1803.05170)) などが含まれます
- [評価](notebooks/03_evaluate/README.md): オフライン メトリクスを使用してアルゴリズムを評価します
- [モデルの選択と最適化](notebooks/04_model_select_and_optimize): レコメンド モデルのハイパーパラメータのチューニングと最適化を行います
- [運用化](notebooks/05_operationalize/README.md): Azure 上の本番環境で動作するようモデルの運用化を行います

異なるアルゴリズムによって期待される形式でデータセットを読み込む、モデル出力の評価、トレーニングとテストデータを分割するような一般的なタスクをサポートするいくつかのユーティリティは、 [reco_utils](reco_utils) で提供されます。いくつかの最新のアルゴリズムの実装は、自習及び独自のアプリケーションとしてカスタマイズできるよう提供しています。

## 使用方法

ローカルマシン、Spark上、[Azure Databricks](SETUP.md#setup-guide-for-azure-databricks) での詳細な設定方法については、[セットアップ ガイド](SETUP.md) をご覧ください。

ローカルマシンでセットアップするには:

1. Anaconda を使用して Python 3.6 以上をインストールします。[Miniconda](https://conda.io/miniconda.html) を使用することで簡単にインストールすることが可能です。
2. リポジトリをクローンします

    ``` bash
    git clone https://github.com/Microsoft/Recommenders
    ```

3. generate conda file スクリプトを実行して conda 環境を作成します:
   (これは基本的な python 環境を作成する場合です。 PySpark または GPU 環境でのセットアップは [SETUP.md](SETUP.md) を参照してください)

    ``` bash
    cd Recommenders
    python scripts/generate_conda_file.py
    conda env create -f reco_base.yaml  
    ```

4. conda 環境をアクティベートし、Jupyter に登録します:

    ``` bash
    conda activate reco_base
    python -m ipykernel install --user --name reco_base --display-name "Python (reco)"
    ```

5. Jupyter notebook server を起動します

    ``` bash
    cd notebooks
    jupyter notebook
    ```

6. 00_quick_start folder フォルダの中にある [SAR Python CPU MovieLens](notebooks/00_quick_start/sar_movielens.ipynb) ノートブックを実行します。Python のカーネルが "Python (reco)" に変更されていることを確認します。

**注意** - [Alternating Least Squares (ALS)](notebooks/00_quick_start/als_movielens.ipynb) ノートブックを実行するには PySpark 環境が必要です。実行する際には[セットアップ ガイド](SETUP.md#dependencies-setup) の PySpark 環境のステップに従ってください。

## アルゴリズム

以下の表は、このリポジトリ内で現在利用可能なアルゴリズムの一覧です。異なる実装が利用可能な場合には、「環境」列内のリンクから各ノートブックを開く事が可能です。

| アルゴリズム | 環境 | 形式 | 詳細 | 
| --- | --- | --- | --- |
| Alternating Least Squares (ALS) | [PySpark](notebooks/00_quick_start/als_movielens.ipynb) | 協調フィルタリング | スケーラビリティと分散コンピューティングが可能な Spark MLLib に最適化された、大規模なデータセットにおける明示的または暗黙的なフィードバックのための行列因子分解アルゴリズム | 
| Deep Knowledge-Aware Network (DKN)<sup>*</sup> | [Python CPU / Python GPU](notebooks/00_quick_start/dkn_synthetic.ipynb) | コンテンツベース フィルタリング | ナレッジグラフと記事の埋め込みを組み込んだディープラーニングアルゴリズムにより、強力なニュースや記事のレコメンデーションを提供 | 
| Extreme Deep Factorization Machine (xDeepFM)<sup>*</sup> | [Python CPU / Python GPU](notebooks/00_quick_start/xdeepfm_criteo.ipynb) | ハイブリッド | ユーザー/アイテムのフィーチャーを使用した暗黙的及び明示的なフィードバックのためのディープラーニングベースのアルゴリズム | 
| FastAI Embedding Dot Bias (FAST) | [Python CPU / Python GPU](notebooks/00_quick_start/fastai_movielens.ipynb) | 協調フィルタリング | ユーザーとアイテムの埋め込みとバイアスを含む汎用アルゴリズム |
| LightGBM/Gradient Boosting Tree<sup>*</sup> | [Python CPU](notebooks/00_quick_start/lightgbm_tinycriteo.ipynb) / [PySpark](notebooks/02_model/mmlspark_lightgbm_criteo.ipynb) | コンテンツベース フィルタリング | コンテンツベースの問題における高速トレーニングと低メモリ使用量の勾配ブースティング ツリー アルゴリズム |
| Neural Collaborative Filtering (NCF) | [Python CPU / Python GPU](notebooks/00_quick_start/ncf_movielens.ipynb) | 協調フィルタリング | 暗黙的なフィードバックの性能を強化したディープラーニングアルゴリズム | 
| Restricted Boltzmann Machines (RBM) | [Python CPU / Python GPU](notebooks/00_quick_start/rbm_movielens.ipynb) | 協調フィルタリング | 明示的または暗黙的なフィードバックのための基礎となる確率分布を学習するためのニューラルネットワークベースのアルゴリズム | 
| Riemannian Low-rank Matrix Completion (RLRMC)<sup>*</sup> | [Python CPU](notebooks/00_quick_start/rlrmc_movielens.ipynb) | 協調フィルタリング | 低メモリ消費量に最適化されたリーマン共役勾配法を使用した行列因子アルゴリズム |
| Simple Algorithm for Recommendation (SAR)<sup>*</sup> | [Python CPU](notebooks/00_quick_start/sar_movielens.ipynb) | 協調フィルタリング | 暗黙的なフィードバック データセット用の類似性ベースのアルゴリズム |
| Surprise/Singular Value Decomposition (SVD) | [Python CPU](notebooks/02_model/surprise_svd_deep_dive.ipynb) | 協調フィルタリング | それほど大きくないデータセット内の明示的な評価フィードバックを予測するための行列因子化アルゴリズム | 
| Vowpal Wabbit Family (VW)<sup>*</sup> | [Python CPU (online training)](notebooks/02_model/vowpal_wabbit_deep_dive.ipynb) | コンテンツベース フィルタリング | ユーザーのフィーチャー/コンテキストが絶えず変化するシナリオに最適な高速オンライン学習アルゴリズム |
| Wide and Deep | [Python CPU / Python GPU](notebooks/00_quick_start/wide_deep_movielens.ipynb) | ハイブリッド | フィーチャの相互作用を記憶し、ユーザーのフィーチャーを一般化できるディープラーニング アルゴリズム |


**注**: <sup>*</sup> 印のアルゴリズムは Microsoft によって開発/寄贈されたアルゴリズム。

**基礎的な比較**

異なるアルゴリズムを評価、比較した[ベンチマーク ノートブック](benchmark/movielens.ipynb)を提供します。
 このノートブックでは、MovieLens データセットは単純分割を使用して 75/25 の比率でトレーニング/テスト セットに分割しました。レコメンデーション モデルは、以下の各協調フィルタリング アルゴリズムを使用してトレーニングしました。経験的パラメータは[この](http://mymedialite.net/examples/datasets.html)文献で報告された値を利用しています。
 私たちが使用したランキング メトリクスは `k=10` ( トップ 10 のレコメンド アイテム) です。比較の際にはスタンダード  NC6s_v2 [Azure DSVM](https://azure.microsoft.com/en-us/services/virtual-machines/data-science-virtual-machines/) (6 vCPUs, 112 GB メモリと 1 P100 GPU) を使用しています。Spark ALS はローカル スタンドアローン モードで実行しました。 この表の結果は Movielens のデータ数 100k、アルゴリズムは 15 エポック実行したものです。

| Algo | 平均的適合率平均(MAP) | 正規化減損累積利得(nDCG)@k | 適合率(Precision)@k | 再現率(Recall)@k | 平均二乗平方根誤差(RMSE) | 平均絶対誤差(MAE) | 決定係数 (R<sup>2</sup>) | 説明分散 | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| [ALS](notebooks/00_quick_start/als_movielens.ipynb) | 0.004732 |	0.044239 |	0.048462 |	0.017796 | 0.965038 |	0.753001 |	0.255647 |	0.251648 | 
| [SVD](notebooks/02_model/surprise_svd_deep_dive.ipynb) | 0.012873	| 0.095930 |	0.091198 |	0.032783 | 0.938681 |	0.742690	| 0.291967 |	0.291971 |
| [SAR](notebooks/00_quick_start/sar_movielens.ipynb) | 0.113028 |	0.388321 | 	0.333828 | 0.183179 | N/A |	N/A |	N/A |	N/A |
| [NCF](notebooks/02_model/ncf_deep_dive.ipynb) | 0.107720	| 0.396118 |	0.347296 |	0.180775 | N/A |	N/A |	N/A |	N/A |
| [FastAI](notebooks/00_quick_start/fastai_movielens.ipynb) | 0.025503 |	0.147866 |	0.130329 |	0.053824 | 0.943084 |	0.744337 |	0.285308 |	0.287671 |





## 貢献するには

このプロジェクトは貢献と提案を歓迎しています。貢献を行う前には [貢献のガイドライン](CONTRIBUTING.md) を始めにご覧ください。


## ビルド状態

| Build Type | Branch | Status |  | Branch | Status | 
| --- | --- | --- | --- | --- | --- | 
| **Linux CPU** | master | [![Status](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_apis/build/status/nightly?branchName=master)](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_build/latest?definitionId=4792) | | staging | [![Status](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_apis/build/status/nightly_staging?branchName=staging)](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_build/latest?definitionId=4594) |
| **Linux GPU** | master | [![Status](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_apis/build/status/nightly_gpu?branchName=master)](https://msdata.visualstudio.com/DefaultCollection/AlgorithmsAndDataScience/_build/latest?definitionId=4997) | | staging | [![Status](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_apis/build/status/nightly_gpu_staging?branchName=staging)](https://msdata.visualstudio.com/DefaultCollection/AlgorithmsAndDataScience/_build/latest?definitionId=4998) |
| **Linux Spark** | master | [![Status](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_apis/build/status/nightly_spark?branchName=master)](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_build/latest?definitionId=4804) | | staging | [![Status](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_apis/build/status/Recommenders/nightly_spark_staging)](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_build/latest?definitionId=5186) |
| **Windows CPU** | master | [![Status](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_apis/build/status/nightly_win?branchName=master)](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_build/latest?definitionId=6743) | | staging | [![Status](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_apis/build/status/nightly_staging_win?branchName=staging)](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_build/latest?definitionId=6752) |
| **Windows GPU** | master | [![Status](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_apis/build/status/nightly_gpu_win?branchName=master)](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_build/latest?definitionId=6756) | | staging | [![Status](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_apis/build/status/nightly_gpu_staging_win?branchName=staging)](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_build/latest?definitionId=6761) |
| **Windows Spark** | master | [![Status](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_apis/build/status/nightly_spark_win?branchName=master)](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_build/latest?definitionId=6757) | | staging | [![Status](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_apis/build/status/nightly_spark_staging_win?branchName=staging)](https://msdata.visualstudio.com/AlgorithmsAndDataScience/_build/latest?definitionId=6754) |

**注** - これらのテストは、smoke の計算と統合テストを実行する nightly ビルドに対して行われています。Master は我々のメイン ブランチであり、Staging は我々の開発ブランチです。[reco_utils](reco_utils)内の Python ユーティリティをテストテストするために `pytest` を使用し、[ノートブック](notebooks)のテストには `papermill` を使用しています。テスト パイプラインの詳細については、[テスト ドキュメント](test/README.md)を参照してください。
