Configuring Kibana
=============================================
Kibana サーバは、起動時に kibana.yml からプロパティを読み込む。
デフォルト設定では、localhost:5601 で実行するようにKibanaを設定している。
ホストまたはポート番号を変更する、また異なるマシンで実行しているElasticsearchに接続するには、kibana.ymlを更新する必要がある。
SSLを有効にしたり、その他のオプションを設定することもできる。

Config設定
--------------------------

.. glossary::
       console.enabled:
           Default: true
           
