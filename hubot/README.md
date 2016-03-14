# Hubotの導入方法

## Hubotとは
GitHub社が開発しMITライセンスで公開しているNode.jsでbotを作り動かすためのフレームワーク


## 参考URL
http://qiita.com/misopeso/items/1f418dd02e89234499b3  
http://qiita.com/acairojuni/items/dc4543aa5827d4c3211c

## 手順
１. 必要パッケージのインストール
```
sudo npm install -g hubot coffee-script generator-hubot
```

そもそもパッケージをインストールするのに必要なもの
- npm
- node

必要なパッケージ
- hubot
- coffee-script
- generator-hubot


>これまでは、hubot --create を使っていたが、非推奨になっており、Yeoman(http://yeoman.io/) >を使う方式に変わったようだ。そのため、generator-hubot のインストールが必要となっている。

２. npm の update  
npm はインストールされているが、バージョンが古いためか yo hubot でエラーが出たためアップデート
```
sudo npm install -g npm
```

３. yo hubot で hubotを作成
```
yo hubot
```

いろいろ聞かれるので、記入。  
slack連携する場合は、Bot adapterにslackを記入が必須。  
```
? Owner: Your Name <your@mail.address>
? Bot name: hubotの名前
? Description: 何をするhubotかの説明
? Bot adapter: slack
```

４.  ローカルでテスト実行
```
# これで起動
bin/hubot

# 自分がつけた名前をつけて実行
hubotの名前 > hubotの名前 ping

# hubotが返してくれれば成功
hubot> PONG

```

５. Gitリポジトリへpush
Github でリポジトリを作成、リポジトリをチェックアウトし、push


６. Herokuにアプリケーション作成

Herokuのアカウントとコマンドラインツールインストールがまだなら作成が必要。

```
# Herokuにログイン
heroku login

# Emailとパスワードを聞かれるので入力
Enter your Heroku credentials.
Email: xxxxxx@gmail.com
Password(typing will be hidden): xxxxxx

# アプリケーション作成
heroku create アプリケーション名
```

７. HerokuでRedisを利用できるようにAdd-onを入れる

HerokuにAdd-onを入れるには、  
HerokuのManage Accountにて、クレジットカードの登録が必要。  
  
Redisを利用できるようにするAdd-on
redistogo:nano
をインストール。

```
# addons:create でAdd-onを追加する
heroku addons:create redistogo:nano

# addons:addは現在非推奨
# heroku addons:add redistogo:nano
# と打つと下記WARNINGが返される
# WARNING: `heroku addons:add` has been deprecated. Please use `heroku addons:create` instead.

```

８. SlackのIntegration設定
下記にアクセス
http://my.slack.com/services/new/hubot

hubotの名前を決めて入力するとHUBOT_SLACK_TOKENが表示されるのでコピー。

```
HUBOT_SLACK_TOKEN=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

９. HUBOT_SLACK_TOKEN をHerokuに設定
```
heroku config:set HUBOT_SLACK_TOKEN=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

１０.  hubot-heroku-keepaliveの設定

Herokuは、現在のFree Plan のままだと 一定時間何もしないとスリープしてしまい、  
Botを動かし続けることができない。  
  
hubot-heroku-keepalive は自分自身を起こし続けるAdd-on。  
  
インストールは３のyo hubot にて追加されているので不要。
下記の手順で設定していく。

```
# 設定に必要な情報を取得
heroku apps:info

# 表示される情報の中から Web URL をコピー
Web URL:  https://xxxx.herokuapp.com

# hubot-heroku-keepaliveの設定
heroku config:set HUBOT_HEROKU_KEEPALIVE_URL=https://xxxx.herokuapp.com/

```

１１. 定期処理のため cronもインストール
```
npm install cron
```

１２. Heroku にデプロイ
```
git push heroku master
```









