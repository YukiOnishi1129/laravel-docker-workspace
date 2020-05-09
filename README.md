# laravel_docker_development

簡単にLaravel環境を構築できるdockerの環境です。

## 環境構築条件

・php 7.2

・nginx

・mysql 5.7

・phpmyadmin

・npm (最新)


## 注意点

### mysqlのDATABASE名、user名、passwordは各自変更してください。

docker-compose.ymlの21~24行目に記述

### ドキュメントルートの設定は各自変更してください。

docker->web->default.confの4行目

root  /var/www/html/プロジェクト名/public;

※/var/www/html/の直下にLaravelプロジェクトを作成する手順にしています。

## 環境構築方法

### 1.上記注意事項を実施

### 2.laravel-docker-workspaceのディレクトリにて以下のコマンドを実施

docker-compose up -d --build (コンテナをビルドして立ち上げる)

### 3.appコンテナにログイン

docker-compose exec app bash

### 4./var/www/html/にいると思うので、そこでLaravelプロジェクトを作成

※ver5.8は以下のコマンドになります。

composer create-project "laravel/laravel=5.8.*" プロジェクト名

### 5.urlの確認

この状態で以下のurlに遷移すると正常に動作できてるか確認できます。

・Laravelトップページ

localhost:8000

・phpMyAdmin

localhost:8080

### 6.migrationの確認

・Laravelプロジェクトの.envファイルを修正

DB_CONNECTION=mysql

DB_HOST=mysql

DB_PORT=3306

DB_DATABASE=docker-compose.ymlの21行目

DB_USERNAME=docker-compose.ymlの22行目

DB_PASSWORD=docker-compose.ymlの23行目

・Laravelプロジェクト直下で以下のコマンドを実行し、migrationが正常に動作できているか確認

php artisan migrate

### 7.npmの確認

・docker上で以下のコマンドを確認し、node,npmが入っていることを確認

node -v

npm -v

・Laravelプロジェクト直下にて以下のコマンドを実行

npm install

上記が正常に動作できれば、構築完了です。

## dockerコマンド

・docker立ち上げ

docker-compose up -d

・docker リスタート

docker-compose restart

・docker 停止

docker-compose stop

・buildして立ち上げる

docker-compose up -d --build

・appコンテナにログイン

docker-compose exec app bash

## mysqlへのログイン方法(docker上で実行)

docker exec -it コンテナID mysql -u root -p

※mysqlのコンテナIDは下記コマンドを実行して確認

docker ps
