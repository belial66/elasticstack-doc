Configuring Elasticsearch
======================================================
Elasticsearchは、良好なデフォルトにて動くのと、非常に小さい設定を要求する。
ほとんどの設定は、Cluster Update Settings APIを使ってクラスタを実行することで変更される。

設定ファイルは、ノード特定（node.nameやpathsのような）の設定や、ノードがクラスタに参加できるようにするために要求する設定（cluster.nameやnetwork.hostのような）を含んでいる。


設定ファイルの場所
-------------------------------------
Elasticsearchには、3つの設定ファイルがある：

* elasticsearch.yml  --- Elasticsearchを設定する
* jvm.options  --- ElasticsearchのJVMを設定する
* log4j2.properties  --- Elasticsearchのロギングを設定する

これらのファイルはconfigディレクトリに置かれる。デフォルトの位置は、インストールがアーカイブを使うか（tar.gz or zip）、パッケージを使うか（RPM等）によって異なる。

アーカイブの場合は、configディレクトリは $ES_HOME/config になる。
以下のようにES_PATH_CONF 環境変数を指定することで変更することもできる。

.. code-block:: console

   # ES_PATH_CONF=/path/to/my/config ./bin/elasticsearch

コマンドラインか、shellのprofileにES_PATH_CONF 環境変数をexportして、設定する。

パッケージの場合、configディレクトリは、/etc/elasticsearchになる。ES_PATH_CONF環境変数で変更できるが、shellでこれを設定するだけでは十分ではないことに注意してください。
この変数は、/etc/default/elasticsearch（Debianの場合）、/etc/sysconfig/elasticsearch（RPMの場合）からソースされている。configディレクトリの場所の変更にともなって、これらのファイルの1つのエントリをES_PATH_CONF=/etc/elasticsearchに編集する必要がある。


configファイルのフォーマット
---------------------------------------------
フォーマットはYAML、ここではdataとlogsのディレクトリ・パスの変更例を示す。

.. code-block:: text

   path:
     data: /var/lib/elasticsearch
     logs: /var/log/elasticsearch

設定は、以下のようにフラットでもよい

.. code-block:: text

   path.data: /var/lib/elasticsearch
   path.logs: /var/log/elasticsearch


環境変数の代入
---------------------------------------------------
環境変数は、設定ファイル内では、${...} notationで参照される。例えば：

.. code-block:: text

   node.name: ${HOSTNAME}
   network.host: ${ES_NETWORK_HOST}

設定のためのプロンプト
-------------------------------------------
設定ファイルに保存したくない設定に対しては、${prompt.text} または ${prompt.secret}を使って、フォアグラウンドでElasticsearchを起動することになる。
${prompt.secret}は、echoが無効であり、入力された値はターミナルに表示されない；
${prompt.text}は、タイプした際に値を見せるようになる。例えば

.. code-block:: text

   node:
     name: ${prompt.text}

Elasticsearchの開始時に、このように実際の値を入力するように促す。

.. code-block:: console

   Enter value for [node.name]:


.. note::

   ${prompt.text}や${prompt.secret}が設定で使われていたり、プロセスがバックグラウンドでサービスとして実行されている場合、Elasticsearchは開始しない。
