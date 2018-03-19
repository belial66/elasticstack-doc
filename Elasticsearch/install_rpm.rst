Install Elasticsearch with RPM
======================================================
Elasticsearchに対するRPMは、websiteか、我々のRPMリポジトリからダウンロードされる。
OpenSUSE、SLES、CentOS、RHEL、OracleのようなRPMベースのシステムにElasticsearchをインストールできる。

.. note::

   RPMインストールは、SLES 11やCentOS 5のような古いバージョンのディストリビューションではサポートされていない。その場合は、.zip or .tag.gzでインストールする必要がある。

Elasitcsearchの最新の安定バージョンは、 `Donwload Elasticsearch <https://www.elastic.co/downloads/elasticsearch>`_ のページで見つけることができる。

.. note::

   ElasticsearchはJava 8以降を要求する。Oracle JDKもしくは、OpenJDKを使おう。

ElasticsearchのPGP Keyをインポート
------------------------------------------
我々は、ElasticsearchのSigning Key(PGP Key D88E42B4, https://pgp.mit.eduから利用可能)ですべてのパッケージを署名している。
フィンガープリントは、以下：

.. code-block:: text

   4609 5ACC 8548 582C 1A26 99A9 D27D 666C D88E 42B4

public signing keyのダウンロードとインストール

.. code-block:: console

   # rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch

RPMリポジトリからインストール
-------------------------------------
/etc/yum.repos.d ディレクトリにelasticsearch.repoファイルを作成する。OpenSuSEの場合は、/etc/zypp/repos.d

.. code-block:: config

   [elasticsearch-6.x]
   name=Elasticsearch repository for 6.x packages
   baseurl=https://artifacts.elastic.co/packages/6.x/yum
   gpgcheck=1
   gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
   enabled=1
   autorefresh=1
   type=rpm-md

リポジトリを使う準備ができたので、以下のコマンドでインストールする。

.. code-block:: console

   # sudo yum install elasticsearch   // CentOS, old RHEL
   # sudo dnf install Elasticsearch   // Fedora, new RHEL
   # zypper  // OpenSUSE

.. tips::

   dnfは、yumの後継。yumがpython2ベースなので、python3ベースの新しいパッケージマネージャが採用されることになっているらしい。

手動でRPMをダウンロードしてインストール
-------------------------------------
RPMをダウンロードして、以下のようにインストールする：

.. code-block:: console

   # wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.2.rpm
   # wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.2.rpm.sha512
   # shasum -a 512 -c elasticsearch-6.2.2.rpm.sha512
   # sudo rpm --install elasticsearch-6.2.2.rpm

- SHAを比較した場合は、「elasticsearch-{version}.zip（もしくは、.tar.gz拡張子）: OK」と出力されればOK。

.. note::

   インストールスクリプトは、カーネルパラメータ（vm.max_map_countなど）を設定しようとする；systemd-sysctl.service unitをマスキングすることでこれをスキップできる。

SysV init VS systemd
---------------------------------------------
Elasticsearchは、インストール後、自動的に開始されない。Elasticsearchの起動／停止の方法は、
使っているシステムが SysV init なのか、systemd（新しいディストリで採用）かによる。
次のコマンドを実行することで使われているのが何なのかを知ることができる。

.. code-block:: console

   # ps -p 1



SysV init で実行している場合
---------------------------------------------


systemd で実行している場合
---------------------------------------------



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
