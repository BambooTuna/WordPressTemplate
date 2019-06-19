# 【WordPress】一つのサーバーに複数のサイトを作りたい

## 関連
[【WordPress】docker-composeを使ってWordPress環境を作る](https://github.com/BambooTuna/WordPressTemplate/blob/master/README.md)

## 概要(About)
AWSやGCPでWordPressを使って複数サイトを構築したいが、お金がない！
出来るだけ出費を抑えたいという時に役にたちます＾＾

## ダウンロード(Download)
```
$ git clone https://github.com/BambooTuna/WordPressTemplate.git
```
[Githubリポジトリ](https://github.com/BambooTuna/WordPressTemplate.git)

## 設定(Settings)
このままでも動きますが、セキュリティ上よくないので、本番環境ではPassなどは変更してください！  
詳細な設定は、[【WordPress】docker-composeを使ってWordPress環境を作る](https://github.com/BambooTuna/WordPressTemplate/blob/master/README.md#%E8%A8%AD%E5%AE%9Asettings)を見てください。  
やってる事は、ポートを変えて単一のサーバーを二つたてているだけです。  

## 使い方(Usage)
```
$ cd ./WordPressTemplate/MultipleServers
$ docker-compose build
$ docker-compose up -d
```
`http://localhost:81`と`http://localhost:82`にアクセスして、それぞれしっかり動いていたら成功です！

## 独自ドメインとSSLに対応したい(More)
ここまできたら次は当然この二つ...  
長くなるので別ページで紹介します。
[編集中]()
