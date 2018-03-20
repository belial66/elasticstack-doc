**********************************
Installing Kibana
**********************************

.. note::

   v6.0.0から、Kibanaは、64-bit OSのみをサポートする

Kibanaは、以下のパッケージフォーマットで提供される。

.. list-table::
   :widths: 20 140
   :header-rows: 0

   * - tar.gz/zip
     - | tar.gz は、LinuxおよびDarwinのために提供されている。
       | zip は、Windows用。
       | :doc:`Install Kibana with .tar.gz<install_targz>`
       | Install Kibana on Windows
   * - deb
     - skip ...
   * - rpm
     - | rpm は、RHEL,CentOS,SLES,OpenSUSEおよびその他のRPMベースのシステムへのインストール用。Webサイト、もしくはRPMリポジトリからダウンロードする。
       | Install Kibana with RPM
   * - docker
     - skip ...

.. note::

   ElasticsearchのインストールがX-Pack Securityで保護されているなら、追加のセットアップのために Configuring Security in Kibana を参照してください。
