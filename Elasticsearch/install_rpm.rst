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
使っているシステムが SysV init なのか、systemd（新しいディストリで採用されている）かによる。
次のコマンドを実行することで使われているのが何なのかを知ることができる。

.. code-block:: console

   # ps -p 1

SysV init でElasticsearchを実行
---------------------------------------------
システム起動時に、自動的に開始するためには、chkconfigコマンドを使う。

.. code-block:: console

   # sudo chkconfig --add elasticsearch

serviceコマンドを使った起動と停止

.. code-block:: console

   # sudo -i service elasticsearch start
   # sudo -i service elasticsearch stop

なんらかの理由で起動に失敗した場合、STDOUTに失敗の理由を表示する。ログファイルは、/var/log/elasticsearchに置かれる。

systemd でElasticsearchを実行
---------------------------------------------
システム起動時に、自動的に開始するためには、chkconfigコマンドを使う。

.. code-block:: console

   # sudo /bin/systemctl daemon-reload
   # sudo /bin/systemctl enable elasticsearch.service

serviceコマンドを使った起動と停止

.. code-block:: console

   # sudo systemctl start elasticsearch.service
   # sudo systemctl stop elasticsearch.service

これらのコマンドは、Elasticsearchの起動が成功したのかどうか、フィードバックしてくれない。
代わりに、この情報は /var/log/elasticsearc/ におかれたログファイルに書かれる。

デフォルトでは、Elasticsearchサービスは systemd journal で情報をログしていない。
journalctlのロギングを有効にするには、elasticsearch.serviceファイルのExecStartコマンドラインから --quiet オプションを削除する必要がある。

systemdロギングが有効になると、ロギング情報はjournalctlコマンドを使って利用可能になる。

journalをtailする：

.. code-block:: console

   # sudo journalctl -f

elasticsearch サービスに対するjournalのエントリをリストする：

.. code-block:: console

   # sudo journalctl --unit elasticsearch

与えられた時刻から、elasticsearch サービスに対するjournalのエントリをリストする：

.. code-block:: console

   # sudo journalctl --unit elasticsearch --since "2016-10-30 18:17:16"

コマンドラインのオプションについては、man journalctl、もしくは https://www.freedesktop.org/software/systemd/man/journalctl.html をチェックしてください。


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

Elasticsearchの設定（コマンドライン）
---------------------------------------------------
ランタイムの設定のために、デフォルトでは、/etc/elasticsearchを使用する。
このディレクトリと、ディレクトリ内のすべてのファイルの所有権は、パッケージインストール時に root:elasticsearchに設定される。
そして、そのディレクトリは、/etc/elasticsearch配下のどのファイルも、サブディレクトリにも同じ所有権で設定されるように、setgidフラグを持っている。
Elasticsearchのプロセスは、グループ権限経由でこのディレクトリ配下のファイルを読むことができることを期待されている。

Elasticsearchは、/etc/elasticsearch/elasticsearch.ymlファイルから設定をロードする。このconfigファイルのフォーマットは、
このconfigファイルのフォーマットは、Configuring Elasticsearchで説明されている。

RPMはシステム設定ファイル（/etc/sysconfig/elasticsearch）も持っている。ここには、以下のパラメータをセットできる。

.. list-table::
   :widths: 20 100

   * - JAVA_HOME
     - カスタムのJavaパスを設定
   * - MAX_OPEN_FILES
     - open filesの最大数。デフォルトは、65536
   * - MAX_LOCKED_MEMORY
     - ロックされるメモリサイズの最大値。elasticsearch.ymlでbootstrap.memory_lockオプションを使っているなら、unlimitedに設定する。
   * - MAX_MAP_COUNT
     - ...
   * - ES_PATH_CONF
     - 設定ファイルのディレクトリ（elasticsearch.yml、jvm.options、log4j2.propertiesファイルを含む必要がある）； デフォルトは /etc/elasticsearch
   * - ES_JAVA_OPTS
     - 適用したい追加のJVM システムプロパティ
   * - RESTART_ON_UPGRADE
     - | パッケージのアップグレード時に再起動する設定で、デフォルトは false。これが意味することは、手動でパッケージをインストールした後にElasticsearchインスタンスを再起動しなければならない、ということ。
       | この理由は、クラスターのアップグレードは、ネットワークが高トラフィックになったり、クラスタの応答時間を縮小する連続したシャードの再割り当てを行わないことを保証する。

.. note::

   ディストリビューションは、/etc/sysconfig/elasticsearchファイル経由よりも、systemd経由で設定されるシステムリソースの制限を要求するsystemdを使用する。もっと知りたいなら、Systemd configurationを参照してください。

RPMのディレクトリレイアウト
--------------------------------------
RPMは、configファイル、logsとdataディレクトリをRPMベースの最適な配置に置き換える。

.. list-table::
   :widths: 15 80 40 15
   :header-rows: 1

   * - 種類
     - 説明
     - デフォルトの場所
     - 設定
   * - home
     - Elasticsearchのホームディレクトリ or $ES_HOME
     - | /usr/share
       | /elasticsearch
     -
   * - bin
     - バイナリのスクリプト。ノードを開始するには、elasticsearch。プラグインをインストールするのは、elasticsearch-plugin
     - | /usr/share
       | /elasticsearch/bin
     -
   * - conf
     - 設定ファイル elasticsearch.yml
     - /etc/elasticsearch
     - ES_PATH_CONF
   * - conf
     - heap size、ファイルディスクリプタなどの環境変数
     - | /etc/sysconfig
       | /elastcisearch
     -
   * - data
     - 各インデックスやノードに割り当てられたシャードのデータファイルの場所。複数の場所に保持することができる。
     - | /var/lib
       | /elastcisearch
     - path.data
   * - logs
     - ログファイルの場所
     - | /var/log
       | /elasticsearch
     - path.logs
   * - plugins
     - プラグインファイルの場所。各プラグインは、サブディレクトリに含まれる。
     - | /usr/share
       | /elasticsearch
       | /plugins
     -
   * - repo
     - 共有ファイルシステムのリポジトリの場所。複数の場所に保持できる。ファイルシステムのリポジトリは、ここに指定したディレクトリのサブディレクトリに置かれる。
     - Not configured
     - path.repo


次のステップ
----------------------------
テスト用のElasticsearch環境をセットアップしていると思う。
これから開発を始めたり、商用で利用しようとする前に、いくつかの追加のセットアップをする必要があるだろう。

* Elasticsearchの設定について
* 重要な設定について
* システムの設定について
