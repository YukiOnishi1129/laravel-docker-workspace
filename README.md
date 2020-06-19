# laravel_docker_development

簡単に Laravel 環境を構築できる docker の環境です。

## 環境構築条件

・php 7.2

・nginx

・mysql 5.7

・phpmyadmin

・npm (最新)

・yarn (最新)

## 注意点

### mysql の DATABASE 名、user 名、password は各自変更してください。

docker-compose.yml の 24~27 行目に記述

### ドキュメントルートの設定は各自変更してください。

docker->web->default.conf の 4 行目

root /var/www/html/プロジェクト名/public;

※/var/www/html/の直下に Laravel プロジェクトを作成する手順にしています。

## 環境構築方法

### 1.上記注意事項を実施

### 2.laravel-docker-workspace のディレクトリにて以下のコマンドを実施

docker-compose up -d --build (コンテナをビルドして立ち上げる)

### 3.app コンテナにログイン

docker-compose exec app bash

### 4./var/www/html/にいると思うので、そこで Laravel プロジェクトを作成

※ver5.8 は以下のコマンドになります。

composer create-project "laravel/laravel=5.8.\*" プロジェクト名

### 5.url の確認

この状態で以下の url に遷移すると正常に動作できてるか確認できます。

・Laravel トップページ

localhost:80

・phpMyAdmin

localhost:8081

### 6.migration の確認

・Laravel プロジェクトの.env ファイルを修正

DB_CONNECTION=mysql

DB_HOST=mysql

DB_PORT=3306

DB_DATABASE=docker-compose.yml の 24 行目

DB_USERNAME=docker-compose.yml の 25 行目

DB_PASSWORD=docker-compose.yml の 26 行目

・Laravel プロジェクト直下で以下のコマンドを実行し、migration が正常に動作できているか確認

php artisan migrate

### 7.npm の確認

・docker 上で以下のコマンドを確認し、node,npm が入っていることを確認

node -v

npm -v

・Laravel プロジェクト直下にて以下のコマンドを実行

npm install

上記が正常に動作できれば、構築完了です。

\*yarn も使用可能です。

### 8.ホットリロード環境の構築

・browser-sync、browser-sync-webpack-plugin をインストール

npm install browser-sync browser-sync-webpack-plugin
(yarn add browser-sync browser-sync-webpack-plugin)

・webpack.mix.js に以下の内容を追記

```javascript=

// react環境で構築する場合です。
mix.react("resources/js/app.jsx", "public/js")
    .sass("resources/sass/app.scss", "public/css")
    // 以下の内容を追記 (上記記載(mix.react~)はvue環境では異なりますが、以下のbrowserSyncからの記載を追記すれば問題ないです。)
    .browserSync({
        proxy: "nginx",
        open: false,
    });
```

・以下のコマンドを実行

npm run watch
(yarn watch)

以上でホットリロードが可能になります。

## docker コマンド

・docker 立ち上げ

docker-compose up -d

・docker リスタート

docker-compose restart

・docker 停止

docker-compose stop

・dockerのコンテナを停止

docker-compose down -v

・build して立ち上げる

docker-compose up -d --build

・app コンテナにログイン

docker-compose exec app bash

## mysql へのログイン方法(docker 上で実行)

docker exec -it コンテナ ID mysql -u root -p

※mysql のコンテナ ID は下記コマンドを実行して確認

docker ps
