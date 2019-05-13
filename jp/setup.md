#### 1.必要環境

Mypherを実行するためには、以下の環境が必要です。

* docker

Mypherをビルドするためには、上記に加え、以下の環境が必要です。

* git
* cmake
* EOS
* C++コンパイラ(clang等)

#### 2.ビルド

実行は、Mypherのdockerイメージを起動することで行います。
現在、開発段階のため、固定イメージを準備しておりません。
dockerイメージを作成するには、

1. github上のMypherプロジェクトをダウンロード
1. スマートコントラクトのビルド
1. Dockerイメージのビルド

を行う必要があります。
それぞれ、以下の手順で行います。

##### 2-1.ダウンロード

以下のコマンドで、任意の場所にプロジェクトをダウンロードします。

```
git clone https://github.com/mypher/mypher
```

##### 2-2.スマートコントラクトのビルド

以下のコマンドで、スマートコントラクトのリソース(abi,wasm)を生成します。

```
cd {プロジェクトルート}/smartcontract
mkdir build
cd build
cmake ..
make
```

##### 2-3.Dockerイメージのビルド

以下のコマンドで、Dockerイメージのビルドを行います。

```
cd {プロジェクトルート}/docker/image
./make.sh
```

完了すると、`mypher` という名前のdockerイメージが作成されます。

#### 3.実行

現状、ローカルでのテスト環境のみ準備しています。  
EOSのメインネット上には、スマートコントラクトはデプロイしていません。  

テスト環境は、３つのdockerコンテナ間でブロックチェインネットワークが構成され、ユーザー用コンテナに対し、ホスト端末からブラウザでアクセスする形としています。  

![](img/local_environment.png)

３つのコンテナは、それぞれ以下の名称としています。
* mypher_eosio
* mypher_myphersystem
* mypher_user

テスト環境の実行は、以下の手順で行います。

##### 3-1.ローカルネットの構築

以下のコマンドで、ローカルネットワークと３コンテナを作成します。

```shell
cd {プロジェクトルート}/docker/
./prepare_netowork.sh # create docker network
./prepare_container.sh # create docker containers 
```

##### 3-2.コンテナ実行

以下のコマンドで、それぞれのコンテナを開始します。  
フォアグラウンドで実行されますので、それぞれターミナルを起動して実行する必要があります。

**mypher_eosio**  
このコンテナは、メインネット上のMypher以外のピアを想定したものです。EOSのシステムアカウントの生成と初期設定を行うスクリプトが含まれます。  
そのため、一番最初に起動する必要があります。  

```shell
cd {プロジェクトルート}/docker/
./start.sh eosio
```

**mypher_myphersystem**  
このコンテナは、メインネット上にMypherプロジェクトが所有するピアを想定したものです。Mypherのシステムアカウントの管理、スマートコントラクトのデプロイを行うスクリプトが含まれます。  
mypher_eosioの次に起動する必要があります。

```shell
cd {プロジェクトルート}/docker/
./start.sh myphersystem
```

**mypher_user**  
このコンテナは、メインネット上のMypherユーザが所有するピアを想定したものです。
一番最後に起動する必要があります。

```shell
cd {プロジェクトルート}/docker/
./start.sh user
```

#### 4.使用

ホスト端末のブラウザを起動し、以下のURLにアクセスすると、Mypherの画面にアクセスすることができます。

```
http://127.0.0.1:8800/index.html
```

![](img/screen_search.gif)

デフォルトで以下の３ユーザを準備しています。
* testuser1111
* testuser2222
* testuser3333

右上のメニューの以下から、それぞれのユーザでログインすることができます。（開発用メニューです。）

* login with testuser1111
* login with testuser2222
* login with testuser3333

