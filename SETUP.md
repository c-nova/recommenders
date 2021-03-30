# セットアップ ガイド

このドキュメントでは、このリポジトリに置かれているノートブックを次のプラットフォームで実行するための、すべての依存関係をセットアップする方法について説明します:

* ローカル (Linux, MacOS または Windows) または [DSVM](https://azure.microsoft.com/en-us/services/virtual-machines/data-science-virtual-machines/) (Linux または Windows)
* [Azure Databricks](https://azure.microsoft.com/en-us/services/databricks/)
* Docker コンテナ

## 目次

  - [コンピュート環境](#コンピュート環境)
  - [ローカル または DSVM 用のセットアップ ガイド](#ローカル-または-DSVM-用のセットアップ-ガイド)
    - [要件](#要件)
    - [依存関係のセットアップ](#依存関係のセットアップ)
    - [Jupyter 内でカーネルとして conda 環境を登録する](#Jupyter-内でカーネルとして-conda-環境を登録する)
    - [DSVM でのトラブルシューティング](#DSVM-でのトラブルシューティング)
  - [Azure Databricks 用のセットアップ ガイド](#Azure-Databricks-用のセットアップ-ガイド)
    - [Azure Databricks の要件](#requirements-of-azure-databricks)
    - [リポジトリのインストール](#リポジトリのインストール)
    - [インストールの確認](#インストールの確認)
    - [Azure Databricks でのインストールにおけるトラブルシューティング](#Azure-Databricks-でのインストールにおけるトラブルシューティング)
    - [Azure Databricks における運用化の準備](#Azure-Databricks-における運用化の準備)
  - [PIP経由でユーティリティをインストールする](#PIP経由でユーティリティをインストールする)
  - [Docker のセットアップガイド](#Docker-のセットアップガイド)

## コンピュート環境

レコメンド システムの種類と実行する必要があるノートブックに応じて、要求されるコンピュート環境が異なります。
現在、このリポジトリは **Python CPU**、**Python GPU**および**PySpark**をサポートしています。

## ローカル または DSVM 用のセットアップ ガイド

### 要件

* Linux,MacOS または Windows が動作しているマシン
* Anaconda と共にインストールされたバージョン 3.6 以上の Python
  * Azure DSVM のようなプレインストールされているコンピュート環境を利用する場合には、そのまま次のステップに進んでください。ローカルマシンにセットアップを行う場合には、以下のサイトで詳細をご確認ください。  
  [Miniconda](https://docs.conda.io/en/latest/miniconda.html) は簡単に始められる方法です。
* [Apache Spark](https://spark.apache.org/downloads.html) (これは PySpark 環境でのみ必要になります)

### 依存関係のセットアップ

Conda を使用した必要要件のインストールを行う際には、Anaconda と Conda パッケージ マネージャー が最新であることを確認してください:

```{shell}
conda update conda -n root
conda update anaconda        # use 'conda install anaconda' if the package is not installed
```

私たちは conda 環境用 yaml ファイルを生成するスクリプト [generate_conda_file.py](tools/generate_conda_file.py)を提供しています。これを使用することで、すべての正しい依存関係をと、Python バージョン 3.6 を使用してターゲット環境を作成することできます。

**注** `xlearn` パッケージは `cmake` に依存しています。`xlearn` 関連のノートブックまたはスクリプトを使用している場合は、`cmake` がシステムにインストールされていることを確認してください。Linux にインストールする最も簡単な方法は apt-get を利用して次のように実行します: `sudo apt-get install -y build-essential cmake`。ソースから `cmake` をインストールするための詳細な手順は、[こちら](https://cmake.org/install/)をご覧ください。

**注** PySpark v2.4.x には Java バージョン 8 が必要です。

<details> 
<summary><strong><em>MacOS に Java 8 をインストールする</em></strong></summary>
  
MacOS に Java 8 をインストールするは [asdf](https://github.com/halcyon/asdf-java) を使用します：

    brew install asdf
    asdf plugin add Java
    asdf install java adoptopenjdk-8.0.265+1
    asdf global java adoptopenjdk-8.0.265+1
    . ~/.asdf/plugins/java/set-java-home.zsh

</details>

ローカルシステムにクローンされたリポジトリが `Recommenders` として、**デフォルト (Python CPU) の環境** をインストールする場合には以下のように実行します:

    cd Recommenders
    python tools/generate_conda_file.py
    conda env create -f reco_base.yaml

環境名は `-n` フラグを使用することで指定することが可能です。

Python GPU および PySpark 環境のインストールについては、以下のメニューをクリックしてください:

<details>
<summary><strong><em>Python GPU 環境</em></strong></summary>

GPU マシンを持っている場合には、以下のように Python GPU 環境をインストールすることが可能です:

    cd Recommenders
    python tools/generate_conda_file.py --gpu
    conda env create -f reco_gpu.yaml

</details>

<details>
<summary><strong><em>PySpark 環境</em></strong></summary>

PySpark 環境のインストールは以下のように指定します:

    cd Recommenders
    python tools/generate_conda_file.py --pyspark
    conda env create -f reco_pyspark.yaml

> さらに、特定のバージョンの Spark をテストする場合は、--pyspark-version 引数を渡して実行できます：
>
>     python tools/generate_conda_file.py --pyspark-version 2.4.5

次に、環境変数 `PYSPARK_PYTHON` と `PYSPARK_DRIVER_PYTHON` をセットし、conda python 実行可能ファイルを指定する必要があります。

詳細を表示するには、次のメニューをクリックします。
<details>
<summary><strong><em>Linux または MacOS 上で PySpark 環境変数を設定する</em></strong></summary>

これらの環境変数を設定する環境がアクティブになるたびにこれらの変数を設定するには、この[ガイド](https://conda.io/docs/user-guide/tasks/manage-environments.html#macos-and-linux)の手順に従います。

まず、インストールされた `reco_pyspark` 環境のパスを取得します：

    RECO_ENV=$(conda env list | grep reco_pyspark | awk '{print $NF}')
    mkdir -p $RECO_ENV/etc/conda/activate.d
    mkdir -p $RECO_ENV/etc/conda/deactivate.d

また、Spark がインストールされている場所を見つけ、`SPARK_HOME` 変数にセットします。DSVMの場合には `SPARK_HOME=/dsvm/tools/spark/current` と設定します。

次に、`$RECO_ENV/etc/conda/activate.d/env_vars.sh` ファイルを作成し、次のように追加します：

```bash
#!/bin/sh
RECO_ENV=$(conda env list | grep reco_pyspark | awk '{print $NF}')
export PYSPARK_PYTHON=$RECO_ENV/bin/python
export PYSPARK_DRIVER_PYTHON=$RECO_ENV/bin/python
export SPARK_HOME=/dsvm/tools/spark/current
```

This will export the variables every time we do `conda activate reco_pyspark`. To unset these variables when we deactivate the environment, create the file `$RECO_ENV/etc/conda/deactivate.d/env_vars.sh` and add:

これにより、`conda activate reco_pyspark` が実行されるたびに変数がエクスポートされます。環境を非アクティブ化するときにこれらの変数を設定解除するには、`$RECO_ENV/etc/conda/deactivate.d/env_vars.sh` ファイルを作成し、次のように追加します：

```bash
#!/bin/sh
unset PYSPARK_PYTHON
unset PYSPARK_DRIVER_PYTHON
```

</details>

<details><summary><strong><em>Windows 上で PySpark 環境変数を設定する</em></strong></summary>

これらの環境変数を設定する環境がアクティブになるたびにこれらの変数を設定するには、この[ガイド](https://conda.io/docs/user-guide/tasks/manage-environments.html#windows)の手順に従います。

まず、インストールされた `reco_pyspark` 環境のパスを取得します：

    for /f "delims=" %A in ('conda env list ^| grep reco_pyspark ^| awk "{print $NF}"') do set "RECO_ENV=%A"

次に、`%RECO_ENV%\etc\conda\activate.d\env_vars.bat` ファイルを作成し、次のように追加します：

    @echo off
    for /f "delims=" %%A in ('conda env list ^| grep reco_pyspark ^| awk "{print $NF}"') do set "RECO_ENV=%%A"
    set PYSPARK_PYTHON=%RECO_ENV%\python.exe
    set PYSPARK_DRIVER_PYTHON=%RECO_ENV%\python.exe
    set SPARK_HOME_BACKUP=%SPARK_HOME%
    set SPARK_HOME=
    set PYTHONPATH_BACKUP=%PYTHONPATH%
    set PYTHONPATH=

これにより、`conda activate reco_pyspark` が実行されるたびに変数がエクスポートされます。環境を非アクティブ化するときにこれらの変数を設定解除するには、`%RECO_ENV%\etc\conda\deactivate.d\env_vars.bat` ファイルを作成し、次のように追加します：

    @echo off
    set PYSPARK_PYTHON=
    set PYSPARK_DRIVER_PYTHON=
    set SPARK_HOME=%SPARK_HOME_BACKUP%
    set SPARK_HOME_BACKUP=
    set PYTHONPATH=%PYTHONPATH_BACKUP%
    set PYTHONPATH_BACKUP=

</details>

</details>

<details>
<summary><strong><em>完全 (PySpark と Python GPU) 環境</em></strong></summary>

この環境では、このリポジトリ内にある PySpark 及び Python GPU 用のノートブックのどちらも動作させることが可能です。
この環境をインストールするには以下のように実行します:

    cd Recommenders
    python tools/generate_conda_file.py --gpu --pyspark
    conda env create -f reco_full.yaml

次に、環境変数 `PYSPARK_PYTHON` と `PYSPARK_DRIVER_PYTHON` をセットし、conda python 実行可能ファイルを指定する必要があります。
これらの変数の設定方法の詳細については、 **PySpark 環境**のセットアップ セクションを参照してください。コマンド内の `reco_pyspark` 文字列は `reco_full` に変更して実行する必要があります。
</details>


### Jupyter 内でカーネルとして conda 環境を登録する

作成した conda 環境は、Jupyter ノートブックに表示されるように登録することが可能です。

    conda activate my_env_name
    python -m ipykernel install --user --name my_env_name --display-name "Python (my_env_name)"
    
DSVM を使用している場合には、Web ブラウザで `https://your-vm-ip:8000` にアクセスし、[JupyterHub に接続](https://docs.microsoft.com/en-us/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro#jupyterhub-and-jupyterlab) することが可能です。

### DSVM でのトラブルシューティング

* マシンの Spark バージョンが conda ファイルのバージョンと同じでない場合、問題が発生する可能性があることがわかりました。`--pyspark-version` オプションを使用することでこの問題を回避することが可能です。

* 単一のローカル ノードで Spark を実行すると、一時ファイルがユーザーのホーム ディレクトリに書き込まれるため、ディスク領域が不足する可能性があります。DSVM でこれを回避するためには、DSVM に追加のディスクを接続し、Spark 構成を変更します。これを行うには、`/dsvm/tools/spark/current/conf/spark-env.sh` のファイルに以下の内容を追記します。

```{shell}
SPARK_LOCAL_DIRS="/mnt"
SPARK_WORKER_DIR="/mnt"
SPARK_WORKER_OPTS="-Dspark.worker.cleanup.enabled=true, -Dspark.worker.cleanup.appDataTtl=3600, -Dspark.worker.cleanup.interval=300, -Dspark.storage.cleanupFilesAfterExecutorExit=true"
```

* もう 1 つの問題の原因は、変数 `SPARK_HOME` が正しく設定されていない場合です。Azure DSVM では、`SPARK_HOME` が `/dsvm/tools/spark/current` 内で指定されている必要があります。

* Java 11 環境では、ノートブックの実行時にエラーが発生することがあります。Java 8 に変更するには以下の内容を実行します:

```
sudo apt install openjdk-8-jdk
sudo update-alternatives --config java
```

* DSVM で利用可能な現在の MMLSpark jar と、ライブラリで使用されている Jar との間に競合する可能性があります。その場合は、これらの jar を削除し、Maven または MMLSpark チームが提供する他のリポジトリからそれらをロードすることをお勧めします。

```
cd /dsvm/tools/spark/current/jars
sudo rm -rf Azure_mmlspark-0.12.jar com.microsoft.cntk_cntk-2.4.jar com.microsoft.ml.lightgbm_lightgbmlib-2.0.120.jar
```

## Azure Databricks 用のセットアップ ガイド

### 要件

* Databricks のランタイム バージョンが 4.3 (Apache Spark 2.3.1, Scala 2.11) 以上及び 5.5 (Apache Spark 2.4.3, Scala 2.11) 以下であること
* Python 3

ワークスペース内に Azure Databricks ワークスペースと Apache Spark クラスターを作成する方法の例については、[こちら](https://docs.microsoft.com/en-us/azure/azure-databricks/quickstart-create-databricks-workspace-portal) を参照してください。ディープラーニング モデルと GPU を使用するには、GPU 対応クラスターをセットアップします。このトピックの詳細については、[Azure Databricks ディープラーニング ガイド](https://docs.azuredatabricks.net/applications/deep-learning/index.html)を参照してください。  

### 依存関係のセットアップ
リポジトリを Databricks のライブラリとして手動でセットアップするか、[インストール スクリプト](tools/databricks_install.py)を実行して設定します。どちらのオプションも、プロビジョニングされた Databricks ワークスペースとクラスターにアクセスでき、ライブラリをインストールするための適切なアクセス許可があることを前提としています。

<details>
<summary><strong><em>クイック インストール</em></strong></summary>

このオプションはセットアップを実行するためにインストール スクリプトを使用し、スクリプトを実行することで必要な追加の依存関係をしてに使用される環境で追加の依存関係をインストールします。

> スクリプトを実行するには、以下の**前提条件**が必要になります:
> * [Azure Databricks CLI (コマンドライン インターフェース)](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli)用の CLI 認証のセットアップ。[ここ](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#set-up-authentication)でトークンの作成と認証の設定方法の詳細を確認してください（訳注：ここの方法を使用し、実際にポータル上で認証トークンの事前作成が必要です）。簡潔に言うと、次のコマンドを使用して環境をインストールおよび構成可能です。
>
>     ```{shell}
>     conda activate reco_pyspark
>     databricks configure --token
>     ```
>
> * 状態が *TERMINATED* の場合には、ターゲットの **cluster id** を使用して クラスタの **start** を実施する必要があります。
>   * CLI でクラスタ ID を確認するには、以下の内容を実行します:
>        ```{shell}
>        databricks clusters list
>        ```
>   * If required, you can start the cluster with:
>        ```{shell}
>        databricks clusters start --cluster-id <CLUSTER_ID>`
>        ```

インストール スクリプトには、さまざまな databricks-cli プロファイルを処理したり、mmlspark ライブラリのバージョンをインストールしたり、ライブラリを上書きしたり、クラスターを操作用に準備したりできるオプションが多数用意されています。すべてのオプションについては、以下を実行して参照してください:

```{shell}
python tools/databricks_install.py -h
```

databricks クラスターが *RUNNING* であることを確認したら、次のコマンドを使用してこのリポジトリ内のモジュールをインストールします。

```{shell}
cd Recommenders
python tools/databricks_install.py <CLUSTER_ID>
```

**注** 運用化のための[この](examples/05_operationalize/als_movie_o16n.ipynb)サンプル コードを実行する予定がある場合には、運用化のためにクラスターを準備する必要があります。これを行うには、スクリプトの実行に追加のオプションを追加します。<CLUSTER_ID> は前述の <CLUSTER_ID> と同じで、`databricks clusters list` を実行し、適切なクラスターを選択することで識別できます。

```{shell}
python tools/databricks_install.py --prepare-o16n <CLUSTER_ID>
```

詳細は以下を参照してください。
</details>

<details>
<summary><strong><em>手動セットアップ</em></strong></summary>

リポジトリを Databricks に手動でインストールするには、次の手順に従います:

1. Microsoft Recommenders リポジトリをローカル コンピュータにクローンします。
2. Recommenders フォルダ内のコンテンツを圧縮します(Azure Databricks では、圧縮フォルダに '.egg' サフィックスが必要なので、標準の '.zip' を使用しません)。

    ```{shell}
    cd Recommenders
    zip -r Recommenders.egg .
    ```

3. クラスターが起動したら、databricks ワークスペースに移動し、`Home` ボタンを選択します。
4. `Home` ディレクトリがパネルに表示されます。ディレクトリ内を右クリックし、`Import` を選択します。
5. ポップアップ ウィンドウには、ライブラリをインポートするオプションがあり、`(To import a library, such as a jar or egg, click here)`と表示されます。`click here` を選択します。
6. 次の画面で、最初のメニューで `Upload Python Egg or PyPI` オプションを選択します。
7. 次に、`Drop library egg here to upload` というテキストが含まれているボックスをクリックし、ファイルセレクタを使用して作成した `Recommenders.egg` ファイルを選択し、`Open` を選択します。
8. `Create library` をクリックします。これにより、Eggファイルがアップロードされ、ワークスペースで使用できるようになります。
9. 最後に、次のメニューで、ライブラリをクラスタにアタッチします。

</details>

### インストールの確認

インストール後、新しいノートブックを作成し、Databricks からユーティリティをインポートして、インポートが機能したことを確認できるようになります。

```{python}
import reco_utils
```

### Azure Databricks でのインストールにおけるトラブルシューティング

* [reco_utils](reco_utils)をインポートして Databricks で動作させるには、コンテンツを正しく圧縮することが重要です。zip は Recommenders フォルダ内で実行する必要があり、Recommenders フォルダ自体を zip を入れても動作しません。

## Azure Databricks における運用化の準備

このリポジトリには、Azure Databricks を使用して最小二乗法を用いた行列因子化を利用したレコメンデーション モデルを推定し、事前に計算されたレコメンデーション アイテムを Azure Cosmos DB に書き込み、Cosmos DBからレコメンデーション アイテムを取得するリアルタイムスコアリングサービスを作成する方法が全て記載されたノートブックが含まれています。この [ノートブック](examples/05_operationalize/als_movie_o16n.ipynb)を実行するには、(前述のように) Recommenders リポジトリをライブラリとしてインストールする必要があり、 **かつ** いくつかの追加の依存関係をインストールする必要があります。*クイックインストール* を使用すると、[インストールスクリプト](tools/databricks_install.py)にいくつかの追加オプションを渡すだけで構成が可能です。

<details>
<summary><strong><em>クイック インストール</em></strong></summary>

このオプションは、インストール スクリプトを使用してセットアップを行います。追加のオプションを使用してインストール スクリプトを実行するだけです。`Recommenders.egg` ライブラリをアップロードしてインストールするためにスクリプトを既に 1 回実行している場合は、`--overwrite` オプションを追加することもできます:

```{shell}
python tools/databricks_install.py --overwrite --prepare-o16n <CLUSTER_ID>
```

このスクリプトは、以下の *手動セットアップ* セクションで説明されているすべての手順を実行します。

</details>

<details>
<summary><strong><em>手動セットアップ</em></strong></summary>

PyPI からライブラリとして、以下の 3 つのパッケージをインストールする必要があります:

* `azure-cli==2.0.56`
* `azureml-sdk[databricks]==1.0.8`
* `pydocumentdb==2.3.3`

PyPI からパッケージをインストールする方法の詳細については、[こちら](https://docs.azuredatabricks.net/user-guide/libraries.html#install-a-library-on-a-cluster)の手順に従って実施します。

さらに、クラスターに [spark-cosmosdb connector](https://docs.databricks.com/spark/latest/data-sources/azure/cosmosdb-connector.html) をインストールする必要があります。手動で行う最も簡単な方法は次のとおりです:

1. [適切な jar ファイル](https://search.maven.org/remotecontent?filepath=com/microsoft/azure/azure/azure-cosmosdb-spark_2.3.0_2.11/1.2.2/azure-cosmosdb-spark_2.3.0_2.11-1.2.2-uber.jar)を MAVEN からダウンロードしてください。**注** これは Spark バージョン '2.3.x' に適した jar であり、上記で詳しく説明した推奨の Azure Databricks ランタイムに適したバージョンです。
2. jar をアップロードしてインストールします。
   1. `Azure Databrics`ワークスペースにログインする
   2. 左側の `Clusters` ボタンを選択します。
   3. ライブラリをインポートするクラスターを選択します。
   4. `Upload` と `Jar` オプションを選択し、その中に `Drop JAR here` というテキストが入っているボックスをクリックします。
   5. ダウンロードした `.jar` ファイルに移動し、それを選択し、`Open` をクリックします。
   6. `Install`をクリックします。
   7. クラスタを再起動します。

</details>

## PIP経由でユーティリティをインストールする

このリポジトリ内のユーティリティをメインディレクトリから簡単にインストールできるようにするために、[setup.py](setup.py) ファイルを提供しています。

この場合でも、上記のように conda 環境をインストールする必要があります。必要な依存関係をインストールしたら、次のコマンドを使用して `reco_utils` を Python パッケージとしてインストールできます。

    pip install -e .

GitHub から直接インストールすることもできます。または特定のブランチからも同様です。

    pip install -e git+https://github.com/microsoft/recommenders/#egg=pkg
    pip install -e git+https://github.com/microsoft/recommenders/@staging#egg=pkg

**注** - pip インストールでは、必要なパッケージの依存関係はインストールされず、conda は使用されているユーティリティの環境を設定するために上記のように使用されると推定します。

## Docker のセットアップガイド

[Dockerfile](tools/docker/Dockerfile) は、さまざまな環境のセットアップを簡素化するために、リポジトリのイメージを構築するために提供されます。システムに Docker エンジンがインストールされている必要があります。

*注: Docker は Azure Data Science Virtual Machine では既定で利用可能です*

さまざまな環境でイメージをビルドして実行する方法の詳細については、Docker の [README](tools/docker/README.md) のガイドラインを参照してください。

以下は基本の CPU 環境で Docker イメージをビルドして実行するコマンドの例です。

```{shell}
DOCKER_BUILDKIT=1 docker build -t recommenders:cpu --build-arg ENV="cpu" .
docker run -p 8888:8888 -d recommenders:cpu
```

実行後、Jupyter Notebook サーバーを http://localhost:8888 から開くことが可能です。