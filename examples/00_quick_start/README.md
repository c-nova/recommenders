# クイック スタート

このディレクトリでは、交互最小二乗法 ([ALS](https://spark.apache.org/docs/latest/api/python/_modules/pyspark/ml/recommendation.html#ALS)) またはレコメンド用の簡易アルゴリズム ([SAR](https://github.com/Microsoft/Product-Recommendations/blob/master/doc/sar.md)) などの、さまざまなアルゴリズムの簡単なデモを実行するためにノートブックが用意されています。ノートブックでは、ユーティリティ関数 ([reco_utils](../../reco_utils)) を使用して、データ準備、モデル構築、およびモデル評価で構成されるエンドツーエンドのレコメンデーション パイプラインを確立する方法を示します。

| Notebook | データセット | 環境 | 説明 |
| --- | --- | --- | --- |
| [als](als_movielens.ipynb) | MovieLens | PySpark | ALS アルゴリズムを使用して、PySpark 環境で映画の評価を予測します。
| [dkn](dkn_MIND.ipynb) | MIND | Python CPU, GPU | Python GPU (TensorFlow) 環境で、ナレッジグラフからの情報を使用したニュースのレコメンドのためのディープ・ナレッジ認識ネットワーク (DKN) [2] アルゴリズムを利用します。
| [fastai](fastai_movielens.ipynb) | MovieLens | Python CPU, GPU | FastAI レコメンデーションを利用して、Python+GPU (PyTorch) 環境で映画の評価を予測します。
| [lightgbm](lightgbm_tinycriteo.ipynb) | Criteo | Python CPU | LightGBM ブースト ツリーを使用して、ユーザーがeコマースの広告をクリックしたかどうかを予測します
| [lstur](lstur_MIND.ipynb) | MIND | Python CPU, GPU | Python+GPU (Tensorflow) 環境で、ニュースのレコメンデーションのための長期および短期ユーザー表現 (LSTUR) [9] とニューラルニュース レコメンデーション を利用します。
| [naml](naml_MIND.ipynb) | MIND | Python CPU, GPU | Python+GPU (Tensorflow) 環境でニュースの主題、副題、タイトル、本文の情報を使用したニュース レコメンデーションのための Attentive multi-view learning（アテンションを利用したマルチビュー学習） (NAML) [7] とニューラルニュース レコメンデーションを利用します。
| [ncf](ncf_movielens.ipynb) | MovieLens | Python CPU, GPU |  Python+GPU (TensorFlow) 環境での映画の評価を予測するニューラル協調フィルタリング (NCF) [1] を利用します。
| [npa](npa_MIND.ipynb) | MIND | Python CPU, GPU | Python+GPU (Tensorflow) 環境でニュース レコメンデーションするために、パーソナライズされたアテンション (NPA)[10]でニューラルニュース レコメンデーションを利用します。
| [nrms](nrms_MIND.ipynb) | MIND | Python CPU, GPU | Python+GPU (Tensorflow) 環境でニュース レコメンデーションするために、マルチヘッド自己アテンション (NRMS) [8] でニューラルニュース レコメンデーションを利用します。
| [rbm](rbm_movielens.ipynb)| MovieLens | Python CPU, GPU | 制限付きボルツマンマシン(rbm)[4]を利用して、Python+GPU (TensorFlow) 環境で映画の評価を予測します。
| [rlrmc](rlrmc_movielens.ipynb) | Movielens | Python CPU | リーマン低ランク行列補完(RLRMC)[6]を利用して、Python+CPU環境で映画の評価を予測します
| [sar](sar_movielens.ipynb) | MovieLens | Python CPU | レコメンデーション 用簡易アルゴリズム (SAR) を使用して、Python+CPU 環境で映画の評価を予測します。
| [sar_azureml](sar_movielens_with_azureml.ipynb) | MovieLens | Python CPU |[Azure Machine Learning service](https://docs.microsoft.com/azure/machine-learning/service/overview-what-is-azure-ml) (AzureML) を使用して SAR を利用して評価する方法の例です。[sar quickstart notebook](sar_movielens.ipynb)の内容を取り入れ、クラウドのパワーを使用してデータを管理し、強力なGPUマシンに切り替え、モデルをトレーニングしながら実行を監視する方法を示します。
| [sar_azureml_designer](sar_movieratings_with_azureml_designer.ipynb) | MovieLens | Python CPU | [AzureML Designer](https://docs.microsoft.com/en-us/azure/machine-learning/concept-designer) で SAR を実装する方法の例を示します。
| [a2svd](sequential_recsys_amazondataset.ipynb) | Amazon | Python CPU, GPU | A2SVD [11] を使用して、ユーザーの短期間での操作から一連の映画を予測します。
| [caser](sequential_recsys_amazondataset.ipynb) | Amazon | Python CPU, GPU | Caser [12] を使用して、ユーザーの短期間での操作から一連の映画を予測します。
| [gru4rec](sequential_recsys_amazondataset.ipynb) | Amazon | Python CPU, GPU | GRU4Rec [13] を使用して、ユーザーの短期間での操作から一連の映画を予測します。
| [nextitnet](sequential_recsys_amazondataset.ipynb) | Amazon | Python CPU, GPU | NextItNet [14] を使用して、ユーザーの短期間での操作から一連の映画を予測します。
| [sli-rec](sequential_recsys_amazondataset.ipynb) | Amazon | Python CPU, GPU | SLi-Rec [11] を使用して、ユーザーの短期間での操作から一連の映画を予測します。
| [wide-and-deep](wide_deep_movielens.ipynb) | MovieLens | Python CPU, GPU | Wide-and-Deep モデル [5] を利用して Python+GPU (TensorFlow) 環境で映画の評価を予測します。
| [xdeepfm](xdeepfm_criteo.ipynb) | Criteo | Python CPU, GPU |  eXtreme ディープ ファクタリゼーションマシン(xDeepFM)[3]を利用して、Python+GPU (TensorFlow) 環境でCTRを予測するための低次と高次の両方のフィーチャーの相互作用を学習します。

[1] _Neural Collaborative Filtering_, Xiangnan He, Lizi Liao, Hanwang Zhang, Liqiang Nie, Xia Hu and Tat-Seng Chua. WWW 2017.<br>
[2] _DKN: Deep Knowledge-Aware Network for News Recommendation_, Hongwei Wang, Fuzheng Zhang, Xing Xie and Minyi Guo. WWW 2018.<br>
[3] _xDeepFM: Combining Explicit and Implicit Feature Interactions for Recommender Systems_, Jianxun Lian, Xiaohuan Zhou, Fuzheng Zhang, Zhongxia Chen, Xing Xie and Guangzhong Sun. KDD 2018.<br>
[4] _Restricted Boltzmann Machines for Collaborative Filtering_, Ruslan Salakhutdinov, Andriy Mnih and Geoffrey Hinton. ICML 2007.<br>
[5] _Wide & Deep Learning for Recommender Systems_, Heng-Tze Cheng et al., arXiv:1606.07792 2016. <br>
[6] _A unified framework for structured low-rank matrix learning_, Pratik Jawanpuria and Bamdev Mishra, In International Conference on Machine Learning, 2018. <br>
[7] _NAML: Neural News Recommendation with Attentive Multi-View Learning_, Chuhan Wu, Fangzhao Wu, Mingxiao An, Jianqiang Huang, Yongfeng Huang and Xing Xie. IJCAI 2019.<br>
[8] _NRMS: Neural News Recommendation with Multi-Head Self-Attention_, Chuhan Wu, Fangzhao Wu, Suyu Ge, Tao Qi, Yongfeng Huang, Xing Xie. in Proceedings of the 2019 Conference on Empirical Methods in Natural Language Processing and the 9th International Joint Conference on Natural Language Processing (EMNLP-IJCNLP).<br>
[9] _LSTUR: Neural News Recommendation with Long- and Short-term User Representations_, Mingxiao An, Fangzhao Wu, Chuhan Wu, Kun Zhang, Zheng Liu and Xing Xie. ACL 2019.<br>
[10] _NPA: Neural News Recommendation with Personalized Attention_, Chuhan Wu, Fangzhao Wu, Mingxiao An, Jianqiang Huang, Yongfeng Huang and Xing Xie. KDD 2019, ADS track.<br>
[11] _Adaptive User Modeling with Long and Short-Term Preferences for Personailzed Recommendation_, Zeping Yu, Jianxun Lian, Ahmad Mahmoody, Gongshen Liu and Xing Xie, IJCAI 2019.<br>
[12] _Personalized top-n sequential recommendation via convolutional sequence embedding_, Jiaxi Tang and Ke Wang, ACM WSDM 2018.<br>
[13] _Session-based Recommendations with Recurrent Neural Networks_, Balazs Hidasi, Alexandros Karatzoglou, Linas Baltrunas and Domonkos Tikk, ICLR 2016.<br>
[14] _A Simple Convolutional Generative Network for Next Item Recommendation_, Fajie Yuan, Alexandros Karatzoglou, Ioannis Arapakis, Joemon M. Jose and Xiangnan He, WSDM 2019. <br>
