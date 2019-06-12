# WordPressTemplate

## 概要(About)
AWSやGCPでWordPressを使ってサイトを構築したい時にDocker-composeを使うと一発で出来るようなので、テンプレとしてメモを残します。

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

- コンテナ内のデータをローカルの`./contents` `./backup` `./log`に紐付けしています、独自のディレクトリを指定したい場合は、変更してください。
```
volumes:
  - ./contents:/var/www/html
  - ./backup:/tmp/backup
  - ./log:/tmp/log
```

- ファイルアップロードサイズ制限を引き上げたい方は、`php.ini`を編集し、`build: .`行をコメントアウトしてください。
```
memory_limit = 30M
post_max_size = 30M
upload_max_filesize = 30M
max_input_time = 120
```

## 使い方(Usage)
```
$ cd ./WordPressTemplate
$ docker-compose build
$ docker-compose up -d
```
`http://localhost`にアクセスして管理画面が表示されたら成功です！
VPS上で起動した場合は、そちらの外部IPでアクセスしてください。

## その他(Other)
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
