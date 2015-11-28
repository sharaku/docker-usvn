usvn
================

# はじめに
dockerにてsubversion、usvn(web管理)を提供するコンテナです。  
コンテナ内のファイルはホスト側からは隔離されます。永続的なファイルの保存が必要な場合は-vオプションを使用してホスト側のディレクトリを以下へマウントしてください。
+ /var/lib/svn

使い方
------
# Installation
以下のようにdocker imageをpullします。

    docker pull sharaku/usvn


Docker imageを自分で構築することもできます。

    git clone https://github.com/sharaku/docker-usvn.git
    cd docker-usvn
    docker build --tag="$USER/usvn" .

# Quick Start
ldapのimageを実行します。

    docker run -d \
      --name usvn \
      -v /path/to/svn/:/var/lib/svn:rw \
      -p 80:80 \
      sharaku/usvn

# usvn install
起動後、以下へアクセスし、インストールを行います。
http://サーバIP/install.php
設定は、以下の通り行ってください。これ以外では設定は失敗します。

1. System Check  
    次へ

2. Language Selection  
    任意
    次へ

3. License Agreement  
    ライセンスに同意して次へ

4. USVN Configuration  
    ※ 全てデフォルト値
    次へ

5. Database Installation  
    データベースタイプ: PDO SqLite  
    データベース: /usr/local/src/usvn-1.0.7/files/usvn.db  
    次へ

6. Administrator User Creation  
    任意の設定  
    次へ

7. Check for a Newer Version  
    任意の設定  
    次へ

8. インストールが終了しました  
    USVNに接続する

## Argument

+   `USVN_SUBDIR` :  
    URLのサブディレクトリを指定します。デフォルトではサブディレクトリはありません。  
    指定は、`USVN_SUBDIR=/usvn`のように/で始まり、終わりには/をつけないようにしてください。  
    `USVN_SUBDIR=/usvn`とした場合は、`http://サーバIP/usvn/`がカレントディレクトリになります。  
    リバースプロキシを使用してサブディレクトリとして運用する場合にご利用ください。

## サブディレクトリとして運用する場合

以下の条件で動作させる場合の例です。

+ サーバIP 192.168.1.100
+ 運用ディレクトリ http://192.168.1.100/usvn
+ 永続化するディレクトリ /var/lib/usvn

    docker run -d \
      --name usvn \
      -v /var/lib/usvn:/var/lib/svn:rw \
      -e USVN_SUBDIR=/usvn \
      -p 80:80 \
      sharaku/usvn

# 制限事項
+ 一度実行した際に使用していた永続データは別サブディレクトリへ移行できない
永続化するディレクトリの中には、どのサブディレクトリで運用するかの設定が記載されています。  
現状、コンバート機能を持たないため、そのままでは別サブディレクトリで運用できません。  
別サブディレクトリにするには永続化するディレクトリ中のconfigディレクトリ内の設定情報の変更をする必要があります。

# TODO
+ LDAP接続が成功しないので要調査

