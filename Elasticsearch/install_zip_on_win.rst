 .with .zip on Windows
======================================================
Elasticsearchは、.zipパッケージを使って、Windowsにインストールすることができる。
elasticsearch-service.batを実行して、サービスとしてElasticsearchをセットアップする。

.. note::

   Elasticsearchは.zipアーカイブでWindowsにインストールされる。MSI Installerも利用できて、Windowsで体験したいならこちらもより簡単である。
   あなたが好むなら.zipでのアプローチでこのまま続ければよい。

.. notes::

   Java8をOracle JDKもしくはOpenJDKで入れておこう。

ダウンロードとインストール
-------------------------------------

ダウンロード

* https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.2.zip

解凍すると、elasticsearch-6.2.2 のフォルダが作られる。これは %ES_HOME% で参照される。
コマンドプロンプトを起動して、%ES_HOME%に移動しよう。


Elasticsearchの実行（コマンドライン）
---------------------------------------------

.. code-block:: console

   > cd %ES_HOME%
   > ./bin/elasticsearch.bat

デフォルトでは、フォアグラウンドで実行する。そのログを標準出力(stdout)に表示する。止めるには、Ctrl-Cで。

Elasticsearchの設定（コマンドライン）
---------------------------------------------------
デフォルトでは、%ES_HOME%/config/elasticsearch.yml ファイルから設定をロードする。
このconfigファイルのフォーマットは、Configuring Elasticsearchで説明されている。

configで指定されるいくつかの設定は、コマンドラインで指定することができる。以下のように、-E syntaxを使う。

.. code-block:: console

   > ./bin/elasticsearch.bat -Ecluster.name=my_cluster -Enode.name=node_1

.. note::

   スペース（空白）を含む値は、クォートで囲む必要がある。例えば、 -Epath.logs="C:\My Logs\logs"

.. tips::

   一般に、cluster.nameのような広範なクラスタ設定は、elastcisearch.ymlファイルに追加して、node.nameのようなnode指定の設定は、コマンドラインで指定することになるだろう。

実行確認
------------------
localhostの9200番ポートにHTTPリクエストを送って、Elasticsearchノードが実行していることをテストできる。

.. code-block:: console

   # GET /

   {
     "name" : "Cp8oag6"
     "cluster_name" : "elasticsearch"
     ...
   }

Windowsのサービスとしてインストール
-------------------------------------------
Elasticsearchは、バックグラウンドで実行するため、もしくは起動時に自動で開始するために、サービスとしてインストールすることができる。
これは、bin\elasticsearch-service.batスクリプトを通して達成できる。
このスクリプトは、install,remove,manage,configure,start,stopを実行できる。

.. code-block:: console

   C:\elasticsearch-6.2.2\bin> elasticsearch-service.bat

   Usage: elasticsearch-service.bat install|remove|start|stop|manager [SERVICE_ID]

スクリプトは1つのパラメータ（実行するためのコマンド）を要求する。さらにサービスIDを追加で続けることもできる（これは、Elasticsearchサービスが複数あるときに有効だ）

.. list-table::
   :widths: 20 100
   :hearder-rows: 1

   * - install
     - サービスとしてElasticsearchをインストールする
   * - remove
     - インストールされたElasticsearchサービスを削除する（起動中の場合、サービスを停止する）
   * - start
     - Elasticsearchサービスを開始する
   * - stop
     - Elasticsearchサービスを停止する
   * - manager
     - インストールされたサービスを管理するGUIを起動する

サービス名とJAVA_HOMEの値はインストール中に利用可能になるだろう。

.. code-block:: console

   c:\elasticsearch-6.2.2\bin> elasticsearch-service.bat install
   Installing service       : "elasticsearch-service-x64"
   Using JAVA_HOME (64-bit) : "c:\jvm\jdk1.8"
   The service 'elasticsearch-service-x64' has been installed.

.. note::

   ？？？

.. note::

   システム環境変数 JAVA_HOME は、利用したいJDK・パスを設定すべき。・・・

サービスの設定をカスタマイズ
---------------------------------


ディレクトリレイアウト
--------------------------------------
.zipと.tar.gzのパッケージは、完全に自己完結型である。すべてのファイルとディレクトリが $ES_HOME に含まれる。

これは、Elasticsearchを使い始めるのにディレクトリを作る必要がないため、とても便利なものである。アンインストールする際も$ES_HOMEを削除するだけなので、とても簡単だ。
しかし、configディレクトリ、dataディレクトリ、logディレクトリのデフォルト位置を変更するのをお勧めする。重要なデータを後で削除してしまわないようにね。

.. list-table::
   :widths: 15 80 40 15
   :header-rows: 1

   * - 種類
     - 説明
     - デフォルトの場所
     - 設定
   * - home
     - Elasticsearchのホームディレクトリ or $ES_HOME
     - 解凍時に生成されるディレクトリ
     -
   * - bin
     - バイナリのスクリプト（elasticsearch、elasticsearch-pluginなど）
     - $ES_HOME/bin
     -
   * - conf
     - 設定ファイル(elasticsearch.ymlなど)
     - $ES_HOME/config
     - ES_PATH_CONF
   * - data
     - 各インデックスやシャード割り当てのデータファイルの場所。複数の場所に保持することができる。
     - $ES_HOME/data
     - path.data
   * - logs
     - ログファイルの場所
     - $ES_HOME/logs
     - path.logs
   * - plugins
     - プラグインファイルの場所。各プラグインは、サブディレクトリに含まれる。
     - $ES_HOME/plugins
     -
   * - repo
     - 共有ファイルシステムのリポジトリの場所。複数の場所に保持できる。
     - Non configured
     - path.repo
   * - script
     - スクリプトファイルの場所
     - $ES_HOME/scripts
     - path.scripts

次のステップ
----------------------------
テスト用のElasticsearch環境をセットアップしていると思う。
これから開発を始めたり、商用で利用しようとする前に、いくつかの追加のセットアップをする必要があるだろう。

* Elasticsearchの設定について
* 重要な設定について
* システムの設定について
