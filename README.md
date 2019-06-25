# 【WordPress】docker-composeを使ってWordPress環境を作る

## 概要(About)
AWSやGCPでWordPressを使ってサイトを構築したい時にDocker-composeを使うと一発で出来るようなので、テンプレとしてメモを残します。
全くファイルを変更しなくても動くはずです！

[ブログ記事](https://blog.bambootuna.com/wordpress/27/)

## ダウンロード(Download)
```
$ git clone https://github.com/BambooTuna/WordPressTemplate.git
```
[Githubリポジトリ](https://github.com/BambooTuna/WordPressTemplate.git)

## 設定(Settings)
- 基本的に変更する箇所は`.env`のみで、DBのパスワードなどを厳重なものに変更してください。
```
MYSQL_DATABASE=wordpress
MYSQL_ROOT_PASSWORD=rootpass
MYSQL_USER=admin
MYSQL_PASSWORD=pass

WORDPRESS_DB_NAME=wordpress
WORDPRESS_DB_USER=admin
WORDPRESS_DB_PASSWORD=pass
```

- コンテナ内のデータをローカルの`./volume/contents` `./volume/backup` `./volume/log`に紐付けしています、独自のディレクトリを指定したい場合は、変更してください。
```
volumes:
  - ./volume/contents:/var/www/html
  - ./volume/backup:/tmp/backup
  - ./volume/log:/tmp/log
```

- ファイルアップロードサイズ制限を引き上げたい方は、`./volume/contents/php.ini`を編集してください。
```
memory_limit = 30M
post_max_size = 30M
upload_max_filesize = 30M
max_input_time = 120
```

## 使い方(Usage)
```
$ cd ./WordPressTemplate/SingleServer
$ docker-compose build
$ docker-compose up -d
```
`http://localhost`にアクセスして管理画面が表示されたら成功です！
VPS上で起動した場合は、そちらの外部IPでアクセスしてください。

## その他(Other)
### httpへのアクセスをhttpsにリダイレクトする
`./volume/contents/.htaccess`or`/var/www/html/.htaccess`の先頭に以下を追記  
普通に検索して出ているコードだと、リダイレクトループで落ちます。
ロードバランサーを使っている場合は以下を使いましょう。  
```
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteCond %{HTTP:X-Forwarded-Proto} !=https
RewriteRule ^/?(.*) https://%{HTTP_HOST}/$1 [R,L]
</IfModule>
```
### GCPでdocker-composeをインストール
GCPで通常のインストールしようとするとなぜか上手くいかないので、別の方法でインストールする。
```
$ docker run docker/compose:1.22.0 version
```
docker-composeだと長いので、dcで呼べるようにしています。
```
$ echo alias dc="'"'docker run --rm \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v "$PWD:/$PWD" \
    -w="/$PWD" \
    docker/compose:1.22.0'"'" >> ~/.bashrc
```
リフレッシュ
```
$ source ~/.bashrc
```

## 一つのサーバーに複数のサイトを作りたい（追記）
[一つのサーバーに複数のサイトを作りたい](https://github.com/BambooTuna/WordPressTemplate/blob/master/MultipleServers/README.md)
