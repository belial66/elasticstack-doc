Install Elasticsearch with .zip or .tar.gz
======================================================
Elasticsearchは、.zipと.tar.gzのパッケージを提供している。
これらのパッケージはどのシステムにもインストールするのに使えて、Elastcisearchを試してみるのにとても簡単な方法である。

Elasitcsearchの最新の安定バージョンは、 `Donwload Elasticsearch <https://www.elastic.co/downloads/elasticsearch>`_ のページで見つけることができる。

.. note::

   ElasticsearchはJava 8以降を要求する。Oracle JDKもしくは、OpenJDKを使おう。

ダウンロードとインストール
-------------------------------------

zipの場合

.. code-block:: console

   # wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.2.zip
   # wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.2.zip.sha512
   # shasum -a 512 -c elasticsearch-6.2.2.zip.sha512
   # unzip elasticsearch-6.2.2.zip
   # cd elasticsearch-6.2.2/

tar.gzの場合

.. code-block:: console

   # wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.2.tar.gz
   # wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.2.tar.gz.sha512
   # shasum -a 512 -c elasticsearch-6.2.2.tar.gz
   # tar -xzf elasticsearch-6.2.2.tar.gz
   # cd elasticsearch-6.2.2/

- いずれの場合も、SHAを比較した場合は、「elasticsearch-{version}.zip（もしくは、.tar.gz拡張子）: OK」と出力されればOK。
- このディレクトリは、$ES_HOMEとして使われる。

Elasticsearchの実行（コマンドライン）
---------------------------------------------

.. code-block:: console

   # ./bin/elasticsearch

デフォルトでは、フォアグラウンドで実行する。そのログを標準出力(stdout)に表示する。止めるには、Ctrl-Cで。

実行確認
------------------
localhostの9200番ポートにHTTPリクエストを送って、Elasticsearchノードが実行していることをテストできる。

.. code-block:: console

   # GET /

   {
     "name" : "Cp8oag6"
     "cluster_name" : "elasticsearch"
     "cluster_uuid" : "AT69_T_DTp-1qgIJlatQqA",
     "version" : {
       "number" : "6.2.2",
       "build_hash" : "f27399d",
       "build_date" : "2016-03-30T09:51:41.449Z",
       "build_snapshot" : false,
       "lucene_version" : "7.2.1",
       "minimum_wire_compatibility_version" : "1.2.3",
       "minimum_index_compatibility_version" : "1.2.3"
     },
     "tagline" : "You Know, for Search"
   }

標準出力へのログ表示は、コマンドラインでの実行時に、 -q または --quiet オプションをつけることで無効にできる。

デーモンとして実行
----------------------------
Elasticsearchをデーモンとして実行するには、-d オプションを指定し、また -p オプションをつけて、プロセスIDをファイルに書くようにする。
下記例のpidは、プロセスIDを書き込むファイルの名前。

.. code-block:: console

   # ./bin/elasticsearch -d -p pid

ログメッセージは、$ES_HOME/logs/ ディレクトリに出力される。

Elasticsearchをシャットダウンするには、pidファイルに記録されたプロセスIDをkillする。

.. code-block:: console

   # kill `cat pid`

.. note::

   startup scriptはRPMとDebianのパッケージで提供されている。

Elasticsearchの設定（コマンドライン）
---------------------------------------------------
デフォルトでは、 $ES_HOME/config/elasticsearch.yml ファイルから設定をロードする。
このconfigファイルのフォーマットは、Configuring Elasticsearchで説明されている。

configで指定されるいくつかの設定は、コマンドラインで指定することができる。以下のように、-E syntaxを使う。

.. code-block:: console

   # ./bin/elasticsearch -d -Ecluster.name=my_cluster -Enode.name=node_1

.. note::

   一般に、cluster.nameのような広範なクラスタ設定は、elastcisearch.ymlファイルに追加して、node.nameのようなnode指定の設定は、コマンドラインで指定することになるだろう。

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
