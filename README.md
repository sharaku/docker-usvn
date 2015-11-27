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
    Default language: 日本語  
    Default timezone: Asia/Tokyo  
    次へ

3. License Agreement  
    ライセンスに同意して次へ

4. USVN Configuration  
    ページタイトル: 		USVN  
    Subversionレポジトリ: 		/usr/local/src/usvn-1.0.7/files/  
    Subversion password file: 	/usr/local/src/usvn-1.0.7/files/htpasswd  
    Subversionアクセスファイル: 	/usr/local/src/usvn-1.0.7/files/authz  
    SubversionのベースURL: 		http://192.168.24.101/svn  
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

