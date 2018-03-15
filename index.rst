.. ElasticStack Docs documentation master file, created by
   sphinx-quickstart on Thu Mar 15 13:12:45 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

#############################################
ElasticStack Docs
#############################################

概要
=============
Elastic Stackのプロダクトは一緒に利用されるように設計されていて、リリースはインストールやアップグレードのプロセスを簡単にするように同期されている。
- Beats 6.2
- Elasticsearch 6.2
- Elasticsearch Hadoop 6.2
- Kibana 6.2
- Logstash 6.2
- X-Pack 6.2

このガイドは、1つ以上のElastic Stackプロダクトを使いたいときに、インストールおよび、アップグレードについての情報を提供する。
インストールには推奨される順番がある。

Elastic Stackのインストール
==========================================
Elastic Stackをインストールするとき、全体のStackを横通しで同じバージョンを使わなければならない。
例えば、Elasticsearch 6.2.2を使っているなら、Beats 6.2.2、Elasticsearch Hadoop 6.2.2、
Kibana 6.2.2、Logstash 6.2.2、X-Pack 6.2.2をインストールする必要がある。

既存のものをアップグレードするには、「Upgrading the Elastic Stack」を参照してくれ。

インストール順序
--------------------------
以下の順番で、使いたいElastic Stackプロダクトをインストールする

1. Elasticsearch
2. X-Pack for Elasticsearch
3. Kibana
4. X-Pack for Kibana
5. Logstash
6. X-Pack for Logstash
7. Beats
8. Elasticsearch Hadoop

この順番でインストールすることは、各プロダクトが依存するコンポーネントが置かれていることを保証する。

Elastic Cloudのインストール
---------------------------------------
Elastic Cloudは、Elastic社から提供されている公式のホストされたElasticsearchとKibanaである。

...使わないので、SKIP...



.. toctree::
   :maxdepth: 2
   :caption: Contents:


##################
Indices and tables
##################

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
