Install Kibana with .tar.gz
======================================================
Kibanaは、.tar.gzパッケージで提供される。Kibanaを試してみるには、とても簡単な形式である。

Kibanaの最新の安定バージョンは、 `Donwload Kibana <https://www.elastic.co/downloads/kibana>`_ のページで見つけることができる。

ダウンロードとインストール（Linux 64）
---------------------------------------------
Kibana v6.2.2に対するLinuxアーカイブは、以下のようにダウンロードして、インストールする：

.. code-block:: console

   # wget https://artifacts.elastic.co/downloads/kibana/kibana-6.2.2-linux-x86_64.tar.gz
   # sha1sum kibana-6.2.2-linux-x86_64.tar.gz  ...(1)
   # tar -xvf kibana-6.2.2-linux-x86_64.tar.gz
   # cd kibana-6.2.2.1-linux-x86_64/  ...(2)

* (1)published SHAと、sha1sumもしくはshasumで生成されるSHAを比較する
* (2)このディレクトリは、$KIBANA_HOMEとして識別される。

ダウンロードとインストール（Darwin）
---------------------------------------------

skip ...


Kibanaの実行（コマンドライン）
---------------------------------------

.. code-block:: console

   # ./bin/Kibana

デフォルトでは、Kibanaはフォアグラウンドで実行する。ログは標準出力（stdout）に表示される。止めるには Ctrl+C を使う。


configファイル経由でKibanaを設定
--------------------------------------------
Kibanaは、$KIBANA_HOME/config/kibana.yml から設定をロードする。このフォーマットは、Configuring Kibanaで説明されている。


.tar.gzアーカイブのディレクトリレイアウト
--------------------------------------------
.tar.gzパッケージは、完全に自己完結型である。すべてのファイルとディレクトリは$KIBANA_HOMEに含まれている。 - アーカイブの解凍時にディレクトリが生成される。






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

* :doc:`Elasticsearchの設定<configuring_elasticsearch>` について
* 重要な設定について
* システムの設定について
