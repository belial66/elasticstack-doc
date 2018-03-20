**********************************
Installing Elasticsearch
**********************************
Elasticsearchは、以下のパッケージフォーマットで提供されている。

.. list-table::
   :widths: 20 140
   :header-rows: 0

   * - zip/tar.gz
     - | どのシステムでも適していて、そして始めるのがとても簡単。
       | :doc:`Install Elasticsearch with .zip or .tar.gz<install_zip_and_targz>`
       | :doc:`Install Elasticsearch with .zip on Windows<install_zip_on_win>`
   * - deb
     - | Debian,Ubuntu,Debianベースのディストリに適している。ElasticsearchのWebサイト、もしくはDebianリポジトリからダウンロードされる。
       | Install Elasticsearch with Debian Package
   * - rpm
     - | RHEL,CentOS,SLES,OpenSUSEとかに適している。ElasticsearchのWebサイト、もしくはRPMリポジトリからダウンロードされる。
       | :doc:`Install Elasticsearch with RPM<install_rpm>`
   * - msi
     - | Windows 64-bit版にインストールするのに使う。.NETFramework 4.5が必要だ。Windowsで動かすための選択である。MSIは、ElasticsearchのWebサイトからダウンロードされる。
       | この機能は、ベータ版であり、変更される可能性がある。
       | Install Elasticsearch with Windows MSI Installer
   * - docker
     - | DockerコンテナとしてElasticsearchを動かすのにイメージを利用できる。ElasticのDockerレジストリからダウンロードされる。デフォルトのイメージは、X-Packがすでに入っている。
       | Install Elasticsearch with Docker


Configuration Management Tools
======================================
大規模なデプロイを手助けするために、以下のConfiguration Management Toolsを提供している。

- Puppet
- Chef
- Ansible
