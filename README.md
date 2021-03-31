# Recommenders

[![Documentation Status](https://readthedocs.org/projects/microsoft-recommenders/badge/?version=latest)](https://microsoft-recommenders.readthedocs.io/en/latest/?badge=latest)

## What's New (2021年2月4日)
新しい [Recommenders 2021.2](https://github.com/microsoft/recommenders/releases/tag/2021.2) をリリースしました！

このリリースではいくつかのバグ修正と最適化を行い、3つの新しいアルゴリズム、GeoIMC、Standard VAEとMultinominal (多項) VAEを追加しています。また、マイクロソフト ニュース データセット (MIND) の使用を容易にするツールも追加しました。さらに、マイクロソフト アカデミック グラフを使用してCOVID論文のレコメンデーションを構築した KDD2020 チュートリアルをパブリッシュしました。

また、デフォルトのブランチを master から main に変更しました。現在このリポジトリをダウンロードすると、main ブランチを取得するようになっています。

過去のお知らせは [NEWS.md](NEWS.md) をご覧ください。

## イントロダクション

このリポジトリはレコメンデーション システムを構築するにあたってのサンプルコードとベストプラクティスを Jupyter ノートブック形式で提供します。私たちが提供するサンプルコードの詳細は、5 つの主要なタスクで構成されます:

- [データの準備](examples/01_prepare_data): それぞれのレコメンド アルゴリズムにデータを準備し、読み込みます
- [モデル](examples/00_quick_start): 古典的またはディープラーニングを使用したレコメンド アルゴリズムを使用してモデルを構築します。アルゴリズムには Alternating Least Squares ([ALS](https://spark.apache.org/docs/latest/api/python/_modules/pyspark/ml/recommendation.html#ALS)) や eXtreme Deep Factorization Machines ([xDeepFM](https://arxiv.org/abs/1803.05170)) などが含まれます
- [評価](examples/03_evaluate): オフライン メトリクスを使用してアルゴリズムを評価します
- [モデルの選択と最適化](examples/04_model_select_and_optimize): レコメンド モデルのハイパーパラメータのチューニングと最適化を行います
- [運用化](examples/05_operationalize): Azure 上の本番環境で動作するようモデルの運用化を行います

異なるアルゴリズムによって期待される形式でデータセットを読み込む、モデル出力の評価、トレーニングとテストデータを分割するような一般的なタスクをサポートするいくつかのユーティリティは、 [reco_utils](reco_utils) で提供されます。いくつかの最新のアルゴリズムの実装は、自習及び独自のアプリケーションとしてカスタマイズできるよう提供しています。[reco_utils ドキュメント](https://readthedocs.org/projects/microsoft-recommenders/)をご覧ください。

このリポジトリのより詳細な概要については、[wiki ページ](https://github.com/microsoft/recommenders/wiki/Documents-and-Presentations)上のドキュメントをご覧ください。

## 使用方法

ローカルマシン、[data science virtual machine (DSVM)](https://azure.microsoft.com/en-gb/services/virtual-machines/data-science-virtual-machines/)上、または[Azure Databricks](SETUP.md#setup-guide-for-azure-databricks) での詳細な設定方法については、[セットアップ ガイド](SETUP.md) をご覧ください。

ローカルマシンでセットアップするには:

1. Anaconda を使用して Python 3.6 以上をインストールします。[Miniconda](https://conda.io/miniconda.html) を使用することで簡単にインストールすることが可能です。
2. リポジトリをクローンします

```bash
git clone https://github.com/Microsoft/Recommenders
```

3. generate conda file スクリプトを実行して conda 環境を作成します: (これは基本的な python 環境を作成する場合です。 PySpark または GPU 環境でのセットアップは [SETUP.md](SETUP.md) を参照してください)

```bash
cd Recommenders
python tools/generate_conda_file.py
conda env create -f reco_base.yaml  
```

4. conda 環境をアクティベートし、Jupyter に登録します:

```bash
conda activate reco_base
python -m ipykernel install --user --name reco_base --display-name "Python (reco)"
```

5. Jupyter notebook server を起動します

```bash
jupyter notebook
```

6. `00_quick_start` フォルダの中にある [SAR Python CPU MovieLens](examples/00_quick_start/sar_movielens.ipynb) ノートブックを実行します。Python のカーネルが "Python (reco)" に変更されていることを確認します。

**注意** - [Alternating Least Squares (ALS)](examples/00_quick_start/als_movielens.ipynb) ノートブックを実行するには PySpark 環境が必要です。実行する際には[セットアップ ガイド](SETUP.md#dependencies-setup) の PySpark 環境のステップに従ってください。ディープラーニング アルゴリズムを利用するためには、GPUマシンの利用を推奨します

## アルゴリズム

以下の表は、このリポジトリ内で現在利用可能なアルゴリズムの一覧です。異なる実装が利用可能な場合には、「環境」列内のリンクから各ノートブックを開く事が可能です。

| アルゴリズム | 環境 | 形式 | 詳細 | 
| --- | --- | --- | --- |
| Alternating Least Squares (ALS) | [PySpark](examples/00_quick_start/als_movielens.ipynb) | 協調フィルタリング | スケーラビリティと分散コンピューティングが可能な Spark MLLib に最適化された、大規模なデータセットにおける明示的または暗黙的なフィードバックのための行列因子分解アルゴリズム | 
| Attentive Asynchronous Singular Value Decomposition (A2SVD)<sup>*</sup> | [Python CPU / Python GPU](examples/00_quick_start/sequential_recsys_amazondataset.ipynb) | 協調フィルタリング | アテンション メカニズムを用いて長期ユーザー設定と短期ユーザ設定の両方を獲得することを目的とした順序ベースのアルゴリズム |
| Cornac/Bayesian Personalized Ranking (BPR) | [Python CPU](examples/02_model_collaborative_filtering/cornac_bpr_deep_dive.ipynb) | 協調フィルタリング | 暗黙のフィードバックを伴う項目の順位を予測するための行列因子化アルゴリズム |
| Convolutional Sequence Embedding Recommendation (Caser) | [Python CPU / Python GPU](examples/00_quick_start/sequential_recsys_amazondataset.ipynb) | 協調フィルタリング | ユーザーの一般的な好みと順序パターンの両方をキャプチャすることを目的とした畳み込みベースのアルゴリズム |
| Deep Knowledge-Aware Network (DKN)<sup>*</sup> | [Python CPU / Python GPU](examples/00_quick_start/dkn_MIND.ipynb) | コンテンツベース フィルタリング | ナレッジグラフと記事の埋め込みを組み込んだディープラーニングアルゴリズムにより、強力なニュースや記事のレコメンデーションを提供 | 
| Extreme Deep Factorization Machine (xDeepFM)<sup>*</sup> | [Python CPU / Python GPU](examples/00_quick_start/xdeepfm_criteo.ipynb) | ハイブリッド | ユーザー/アイテムの特徴量を使用した暗黙的及び明示的なフィードバックのためのディープラーニングベースのアルゴリズム | 
| FastAI Embedding Dot Bias (FAST) | [Python CPU / Python GPU](examples/00_quick_start/fastai_movielens.ipynb) | 協調フィルタリング | ユーザーとアイテムの埋め込みとバイアスを含む汎用アルゴリズム |
| LightFM/Hybrid Matrix Factorization | [Python CPU](examples/02_model_hybrid/lightfm_deep_dive.ipynb) | ハイブリッド | 暗黙的および明示的フィードバックのハイブリッド行列分解アルゴリズム |
| LightGBM/Gradient Boosting Tree<sup>*</sup> | [Python CPU](examples/00_quick_start/lightgbm_tinycriteo.ipynb) / [PySpark](examples/02_model_content_based_filtering/mmlspark_lightgbm_criteo.ipynb) | コンテンツベース フィルタリング | コンテンツベースの問題における高速トレーニングと低メモリ使用量の勾配ブースティング ツリー アルゴリズム |
| LightGCN | [Python CPU / Python GPU](examples/02_model_collaborative_filtering/lightgcn_deep_dive.ipynb) | 協調フィルタリング | 暗黙のフィードバックを予測するためのGCNの設計を簡素化するディープラーニングアルゴリズム |
| GeoIMC | [Python CPU](examples/00_quick_start/geoimc_movielens.ipynb) | ハイブリッド | リーマン共役勾配最適化と幾何学的アプローチに従って、ユーザーと項目の特徴量を考慮に入れた行列補完アルゴリズム |
| GRU4Rec | [Python CPU / Python GPU](examples/00_quick_start/sequential_recsys_amazondataset.ipynb) | 協調フィルタリング | 回帰型ニューラルネットワークを用いて長期ユーザー設定と短期ユーザ嗜好の両方をキャプチャすることを目的とした順序ベースのアルゴリズム |
| Multinomial VAE | [Python CPU / Python GPU](examples/02_model_collaborative_filtering/multi_vae_deep_dive.ipynb) | 協調フィルタリング | ユーザー/項目の相互作用を予測するための生成モデル |
| Neural Recommendation with Long- and Short-term User Representations (LSTUR)<sup>*</sup> | [Python CPU / Python GPU](examples/00_quick_start/lstur_MIND.ipynb) | コンテンツベース フィルタリング | 長期および短期ユーザーの交互モデリングを用いたニューラル レコメンデーション アルゴリズム |
| Neural Recommendation with Attentive Multi-View Learning (NAML)<sup>*</sup> | [Python CPU / Python GPU](examples/00_quick_start/naml_MIND.ipynb) | コンテンツベース フィルタリング | Attentive multi-view learning（アテンションを利用したマルチビュー学習）を用いたニューラル レコメンデーション アルゴリズム |
| Neural Collaborative Filtering (NCF) | [Python CPU / Python GPU](examples/00_quick_start/ncf_movielens.ipynb) | 協調フィルタリング | 暗黙的なフィードバックの性能を強化したディープラーニングアルゴリズム | 
| Neural Recommendation with Personalized Attention (NPA)<sup>*</sup> | [Python CPU / Python GPU](examples/00_quick_start/npa_MIND.ipynb) | コンテンツベース フィルタリング | パーソナライズされたアテンション ネットワークを持つニューラル レコメンデーション アルゴリズム |
| Neural Recommendation with Multi-Head Self-Attention (NRMS)<sup>*</sup> | [Python CPU / Python GPU](examples/00_quick_start/nrms_MIND.ipynbb) | コンテンツベース フィルタリング | マルチヘッド 自己アテンション を伴うニューラル レコメンデーション アルゴリズム |
| Next Item Recommendation (NextItNet) | [Python CPU / Python GPU](examples/00_quick_start/sequential_recsys_amazondataset.ipynb) | 協調フィルタリング | 連続パターンのキャプチャを目的とした拡張畳み込みと残差ネットワークに基づくアルゴリズム |
| Restricted Boltzmann Machines (RBM) | [Python CPU / Python GPU](examples/00_quick_start/rbm_movielens.ipynb) | 協調フィルタリング | 明示的または暗黙的なフィードバックのための基礎となる確率分布を学習するためのニューラルネットワークベースのアルゴリズム | 
| Riemannian Low-rank Matrix Completion (RLRMC)<sup>*</sup> | [Python CPU](examples/00_quick_start/rlrmc_movielens.ipynb) | 協調フィルタリング | 低メモリ消費量に最適化されたリーマン共役勾配法を使用した行列因子アルゴリズム |
| Simple Algorithm for Recommendation (SAR)<sup>*</sup> | [Python CPU](examples/00_quick_start/sar_movielens.ipynb) | 協調フィルタリング | 暗黙的なフィードバック データセット用の類似性ベースのアルゴリズム |
| Short-term and Long-term preference Integrated Recommender (SLi-Rec)<sup>*</sup> | [Python CPU / Python GPU](examples/00_quick_start/sequential_recsys_amazondataset.ipynb) | 協調フィルタリング | アテンション メカニズム、時間対応コントローラ、コンテンツ対応コントローラを使用して、長期的なユーザー設定と短期的なユーザー設定の両方をキャプチャすることを目的とした順序ベースのアルゴリズム |
| Standard VAE | [Python CPU / Python GPU](examples/02_model_collaborative_filtering/standard_vae_deep_dive.ipynb) | 協調フィルタリング | ユーザー/項目の相互作用を予測するための生成モデル |
| Surprise/Singular Value Decomposition (SVD) | [Python CPU](examples/02_model_collaborative_filtering/surprise_svd_deep_dive.ipynb) | 協調フィルタリング | それほど大きくないデータセット内の明示的な評価フィードバックを予測するための行列因子化アルゴリズム | 
| Term Frequency - Inverse Document Frequency (TF-IDF) | [Python CPU](examples/00_quick_start/tfidf_covid.ipynb) | コンテンツベース フィルタリング | テキスト データセットを使用したコンテンツ ベースのレコメンデーション事項に対する単純な類似性ベースのアルゴリズム |
| Vowpal Wabbit Family (VW)<sup>*</sup> | [Python CPU (online training)](examples/02_model_content_based_filtering/vowpal_wabbit_deep_dive.ipynb) | コンテンツベース フィルタリング | ユーザーの特徴量/コンテキストが絶えず変化するシナリオに最適な高速オンライン学習アルゴリズム |
| Wide and Deep | [Python CPU / Python GPU](examples/00_quick_start/wide_deep_movielens.ipynb) | ハイブリッド | 特徴量の相互作用を記憶し、ユーザー特徴量を一般化できるディープラーニング アルゴリズム |
| xLearn/Factorization Machine (FM) & Field-Aware FM (FFM) | [Python CPU](examples/02_model_hybrid/fm_deep_dive.ipynb) | コンテンツベース フィルタリング | ユーザー/項目特徴量を備えたラベルを予測する高速でメモリ効率の高いアルゴリズム |


**注**: <sup>*</sup> 印のアルゴリズムは Microsoft によって開発/寄贈されたアルゴリズム。

採用候補の独立またはインキュベートされたアルゴリズムとユーティリティは、[contrib](contrib) フォルダにあります。これにより、コア リポジトリに簡単に収まらない場合や、コードをリファクタリングまたは成熟させ、必要なテストを追加する時間が必要になる可能性のある貢献品が格納されます。

| アルゴリズム | 環境 | 形式 | 詳細 |
| --- | --- | --- | --- |
| SARplus <sup>*</sup> | [PySpark](contrib/sarplus/README.md) | 協調フィルタリング | Spark 用に SAR の実装を最適化 |

**基礎的な比較**

異なるアルゴリズムを評価、比較した[ベンチマーク ノートブック](examples/06_benchmarks/movielens.ipynb)を提供します。
 このノートブックでは、MovieLens データセットは単純分割を使用して 75/25 の比率でトレーニング/テスト セットに分割しました。レコメンデーション モデルは、以下の各協調フィルタリング アルゴリズムを使用してトレーニングしました。経験的パラメータは[この](http://mymedialite.net/examples/datasets.html)文献で報告された値を利用しています。
 私たちが使用したランキング メトリクスは `k=10` (トップ 10 のレコメンド アイテム) です。比較の際にはスタンダード  NC6s_v2 [Azure DSVM](https://azure.microsoft.com/en-us/services/virtual-machines/data-science-virtual-machines/) (6 vCPUs, 112 GB メモリと 1つの P100 GPU) を使用しています。Spark ALS はローカル スタンドアローン モードで実行しました。 この表の結果は Movielens のデータ数 100k、アルゴリズムは 15 エポック実行したものです。

| Algo | 平均的適合率平均(MAP) | 正規化減損累積利得(nDCG)@k | 適合率(Precision)@k | 再現率(Recall)@k | 平均二乗平方根誤差(RMSE) | 平均絶対誤差(MAE) | 決定係数 (R<sup>2</sup>) | 説明分散 | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| [ALS](examples/00_quick_start/als_movielens.ipynb) | 0.004732 |	0.044239 |	0.048462 |	0.017796 | 0.965038 |	0.753001 |	0.255647 |	0.251648 |
| [BPR](examples/02_model_collaborative_filtering/cornac_bpr_deep_dive.ipynb) | 0.105365	| 0.389948 |	0.349841 |	0.181807 | N/A |	N/A |	N/A |	N/A |
| [FastAI](examples/00_quick_start/fastai_movielens.ipynb) | 0.025503 |	0.147866 |	0.130329 |	0.053824 | 0.943084 |	0.744337 |	0.285308 |	0.287671 |
| [LightGCN](examples/02_model_collaborative_filtering/lightgcn_deep_dive.ipynb) | 0.088526 | 0.419846 | 0.379626 | 0.144336 | N/A | N/A | N/A | N/A |
| [NCF](examples/02_model_hybrid/ncf_deep_dive.ipynb) | 0.107720	| 0.396118 |	0.347296 |	0.180775 | N/A | N/A | N/A | N/A |
| [SAR](examples/00_quick_start/sar_movielens.ipynb) | 0.110591 |	0.382461 | 	0.330753 | 0.176385 | 1.253805 | 1.048484 |	-0.569363 |	0.030474 |
| [SVD](examples/02_model_collaborative_filtering/surprise_svd_deep_dive.ipynb) | 0.012873	| 0.095930 |	0.091198 |	0.032783 | 0.938681 | 0.742690 | 0.291967 | 0.291971 |


## 貢献するには

このプロジェクトは貢献と提案を歓迎しています。貢献を行う前には [貢献のガイドライン](CONTRIBUTING.md) を始めにご覧ください。


## ビルド状態

これらのテストは、smoke の計算と統合テストを実行する nightly ビルドに対して行われています。`main` は主要ブランチであり、`staging` は我々の開発ブランチです。[reco_utils](reco_utils)内の Python ユーティリティをテストテストするために `pytest` を使用し、[ノートブック](notebooks)のテストには `papermill` を使用しています。テスト パイプラインの詳細については、[テスト ドキュメント](tests/README.md)を参照してください。

### DSVM ビルド状態

以下のテストは毎日 Windows 及び Linux DSVM 上で行われています。これらのマシンは24時間365日稼働しています。

| ビルド形式 | ブランチ | 状態 |  | ブランチ | 状態 |
| --- | --- | --- | --- | --- | --- |
| **Linux CPU** | main | [![Build Status](https://dev.azure.com/best-practices/recommenders/_apis/build/status/linux-tests/dsvm_nightly_linux_cpu?branchName=main)](https://dev.azure.com/best-practices/recommenders/_build/latest?definitionId=162&branchName=main) | | staging | [![Build Status](https://dev.azure.com/best-practices/recommenders/_apis/build/status/linux-tests/dsvm_nightly_linux_cpu?branchName=staging)](https://dev.azure.com/best-practices/recommenders/_build/latest?definitionId=162&branchName=staging) |
| **Linux GPU** | main | [![Build Status](https://dev.azure.com/best-practices/recommenders/_apis/build/status/linux-tests/dsvm_nightly_linux_gpu?branchName=main)](https://dev.azure.com/best-practices/recommenders/_build/latest?definitionId=163&branchName=main) | | staging | [![Build Status](https://dev.azure.com/best-practices/recommenders/_apis/build/status/linux-tests/dsvm_nightly_linux_gpu?branchName=staging)](https://dev.azure.com/best-practices/recommenders/_build/latest?definitionId=163&branchName=staging) |
| **Linux Spark** | main | [![Build Status](https://dev.azure.com/best-practices/recommenders/_apis/build/status/linux-tests/dsvm_nightly_linux_pyspark?branchName=main)](https://dev.azure.com/best-practices/recommenders/_build/latest?definitionId=164&branchName=main) | | staging | [![Build Status](https://dev.azure.com/best-practices/recommenders/_apis/build/status/linux-tests/dsvm_nightly_linux_pyspark?branchName=staging)](https://dev.azure.com/best-practices/recommenders/_build/latest?definitionId=164&branchName=staging) |
<!--
| **Windows CPU** | main | [![Build Status](https://dev.azure.com/best-practices/recommenders/_apis/build/status/windows-tests/dsvm_nightly_win_cpu?branchName=main)](https://dev.azure.com/best-practices/recommenders/_build/latest?definitionId=101&branchName=main) | | staging | [![Build Status](https://dev.azure.com/best-practices/recommenders/_apis/build/status/windows-tests/dsvm_nightly_win_cpu?branchName=staging)](https://dev.azure.com/best-practices/recommenders/_build/latest?definitionId=101&branchName=staging) |
| **Windows GPU** | main | [![Build Status](https://dev.azure.com/best-practices/recommenders/_apis/build/status/windows-tests/dsvm_nightly_win_gpu?branchName=main)](https://dev.azure.com/best-practices/recommenders/_build/latest?definitionId=102&branchName=main) | | staging | [![Build Status](https://dev.azure.com/best-practices/recommenders/_apis/build/status/windows-tests/dsvm_nightly_win_gpu?branchName=staging)](https://dev.azure.com/best-practices/recommenders/_build/latest?definitionId=102&branchName=staging) |
| **Windows Spark** | main | [![Build Status](https://dev.azure.com/best-practices/recommenders/_apis/build/status/windows-tests/dsvm_nightly_win_pyspark?branchName=main)](https://dev.azure.com/best-practices/recommenders/_build/latest?definitionId=103&branchName=main) | | staging | [![Build Status](https://dev.azure.com/best-practices/recommenders/_apis/build/status/windows-tests/dsvm_nightly_win_pyspark?branchName=staging)](https://dev.azure.com/best-practices/recommenders/_build/latest?definitionId=103&branchName=staging) |
-->

## 関連するプロジェクト

- [Microsoft AI Github](https://github.com/microsoft/ai): その他のベスト プラクティス プロジェクト、および Azure AI デザインパターンをセントラル リポジトリで探すことができます。
- [NLP best practices](https://github.com/microsoft/nlp-recipes): NLP のベストプラクティスとサンプルがあります。
- [Computer vision best practices](https://github.com/microsoft/computervision-recipes): コンピュータ ビジョンのベストプラクティスとサンプルがあります。
- [Forecasting best practices](https://github.com/microsoft/forecasting): 時系列予測のベストプラクティスとサンプルがあります。

## 参考論文

- A. Argyriou, M. González-Fierro, and L. Zhang, "Microsoft Recommenders: Best Practices for Production-Ready Recommendation Systems", *WWW 2020: International World Wide Web Conference Taipei*, 2020. Available online: https://dl.acm.org/doi/abs/10.1145/3366424.3382692
- L. Zhang, T. Wu, X. Xie, A. Argyriou, M. González-Fierro and J. Lian, "Building Production-Ready Recommendation System at Scale", *ACM SIGKDD Conference on Knowledge Discovery and Data Mining 2019 (KDD 2019)*, 2019.
- S. Graham,  J.K. Min, T. Wu, "Microsoft recommenders: tools to accelerate developing recommender systems", *RecSys '19: Proceedings of the 13th ACM Conference on Recommender Systems*, 2019. Available online: https://dl.acm.org/doi/10.1145/3298689.3346967
