**********************************
Installing Elasticsearch
**********************************
Elasticsearchは、以下のパッケージフォーマットで提供されている。

.. list-table::
   :widths: 20 140
   :header-rows: 0

   * - zip/tar.gz
     - どのシステムでも適していて、そして始めるのがとても簡単。
   * - deb
     - Debian,Ubuntu,Debianベースのディストリに適している。ElasticsearchのWebサイト、もしくはDebianリポジトリからダウンロードされる。
   * - rpm
     - RHEL,CentOS,SLES,OpenSUSEとかに適している。ElasticsearchのWebサイト、もしくはRPMリポジトリからダウンロードされる。
   * - msi
     - Windows 64-bit版にインストールするのに使う。.NETFramework 4.5が必要だ。Windowsで動かすための選択である。MSIは、ElasticsearchのWebサイトからダウンロードされる。
   * - docker
     - DockerコンテナとしてElasticsearchを動かすのにイメージを利用できる。ElasticのDockerレジストリからダウンロードされる。デフォルトのイメージは、X-Packがすでに入っている。

with .zip or .tar.gz
==============================
Elasticsearchは、.zipと.tar.gzパッケージを提供している。

Elasitcsearchの最新の安定バージョンは、 `Donwload Elasticsearch <https://www.elastic.co/downloads/elasticsearch>`_ のページで見つけることができる。

.. note::

   ElasticsearchはJava 8以降を要求する。Oracle JDKもしくは、OpenJDKを使おう。

Download and install
----------------------------

zipの場合

.. code-block:: console

   # wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.2.zip
   # unzip elasticsearch-6.2.2.zip
   # cd elasticsearch-6.2.2/

- このディレクトリは、$ES_HOMEとして使われる。

tar.gzの場合

.. code-block:: console

   # wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.2.tar.gz
   # tar -xzf elasticsearch-6.2.2.tar.gz
   # cd elasticsearch-6.2.2/

- このディレクトリは、$ES_HOMEとして使われる。

Running Elasticsearch from the command line
--------------------------------------------------

.. code-block:: console

   # ./bin/elasticsearch

デフォルトでは、フォアグラウンドで実行する。そのログを標準出力(stdout)に表示する。止めるには、Ctrl-Cで。

Checking that Elasticsearch is running
--------------------------------------------------
localhostの9200番ポートにHTTPリクエストを送って、Elasticsearchノードが実行していることをテストできる。

.. code-block:: console

   # GET /

   {
     "name" : "Cp8oag6"
     "cluster_name" : "elasticsearch"
     ...
   }

標準出力へのログ表示は、コマンドラインでの実行時に、 -q または --quiet オプションをつけることで無効にできる。

Running as a daemon
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

Configuring Elasticsearch on the command line
---------------------------------------------------
デフォルトでは、 $ES_HOME/config/elasticsearch.yml ファイルから設定をロードする。
このconfigファイルのフォーマットは、Configuring Elasticsearchで説明されている。

configで指定されるいくつかの設定は、コマンドラインで指定することができる。以下のように、-E syntaxを使う。

.. code-block:: console

   # ./bin/elasticsearch -d -Ecluster.name=my_cluster -Enode.name=node_1

.. note::

   一般に、cluster.nameのような広範なクラスタ設定は、elastcisearch.ymlファイルに追加して、node.nameのようなnode指定の設定は、コマンドラインで指定することになるだろう。




Configuration Management Tools
======================================
大規模なデプロイを手助けするために、以下のConfiguration Management Toolsを提供している。

- Puppet
- Chef
- Ansible
