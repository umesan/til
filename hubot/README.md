
# Hubotの導入方法

## Hubotとは
GitHub社が開発しMITライセンスで公開しているNode.jsでbotを作り動かすためのフレームワーク


## 参考URL
http://qiita.com/misopeso/items/1f418dd02e89234499b3  
http://qiita.com/acairojuni/items/dc4543aa5827d4c3211c  
https://iimuz.github.io/2015/11/11/hubotKeepalive.html
http://sota1235.hatenablog.com/entry/2015/06/10/130000

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

１０.  hubot-heroku-keepalive と scheduler の設定

Herokuは、現在のFree Plan のままだと下記の利用制限がある。  
  
- 30分以上サイトにアクセスがないとSleepする
- 1日6時間の強制Sleep時間が存在する
  
この制限のため、デフォルトのままだとBotを動かし続けることができない。  
  
そこで、hubot-heroku-keepalive と scheduler いう add-onを使ってこの制限を回避する。  

hubot-heroku-keepalive は30分以上アクセスがないとSleepする制限を一定間隔でサイトにリクエストを送ることで解消する。
ただし、hubot-heroku-keepaliveは、Sleepしていない時はリクエストを送ることができるが、一回スリープしてしまうと自力で起きることはできないため、
別途 scheduler という addon を利用してHerokuを起動してあげる。  
  
  
インストールは３の yo hubot にて既に追加されているので不要。
下記の手順で設定していく。

```
# 1.設定に必要な情報を取得
heroku apps:info

# 2.表示される情報の中から Web URL をコピー
Web URL:  https://xxxx.herokuapp.com

# 3.対象のURLを設定
heroku config:set HUBOT_HEROKU_KEEPALIVE_URL=https://xxxx.herokuapp.com/

# 4.Herokuを起こす時間を設定
heroku config:add HUBOT_HEROKU_WAKEUP_TIME=7:00 -a アプリケーション名

# 5.Herokuを眠らせる時間を設定（起きる時間と寝る時間は6時間以上あける必要あり）
heroku config:add HUBOT_HEROKU_SLEEP_TIME=1:00 -a アプリケーション名

# 6.Schedulerのインストール
heroku addons:create scheduler:standard -a アプリケーション名

# 7.SchedulerのWeb設定画面へ
heroku addons:open scheduler

# 8.Web画面にてnew job を登録
curl ${HUBOT_HEROKU_KEEPALIVE_URL}heroku/keepalive

# 9. 起動時間の設定
「NEXT DUE」 に入力する時間は、UTCのため日本時間より9時間前を設定する必要あり。
起動時間は、4.で設定した起こす時間（HUBOT_HEROKU_WAKEUP_TIME）にしたいので、
HUBOT_HEROKU_WAKEUP_TIME - 9時間 の値を設定する。

ここでは、起動したい時間が7:00（HUBOT_HEROKU_WAKEUP_TIMEが 7:00）なので、9時間前の22:00を指定する。

DYNO SIZE
「Free」

FREQUENCY
「Daily」

LAST RUN
Mar 15 0:01 UTC

NEXT DUE
Mar 16 「22:00」 UTC

```

１１. 定期処理のため cron と time もインストール
```
npm install cron time --save
```

１２. Heroku にデプロイ
```
git push heroku master
```

１３. Slackにて動作確認
botにつぶやかせたい CHANNELSに Invite others to this channel で、作ったbotアカウントを招待。
その後botにテスト投稿。
```
# アカウントに発言
@アカウント名:ping

# 返信返ってくればOK
PONG
```

１４. Timezonの設定
```
heroku config:add TZ=Asia/Tokyo
```

１５. cronの設定

/scripts/cron.coffee を作成し、下記を記述

```
###
Description:Cronテスト

cronTimeの設定方法
「秒(0-59)」「分(0-59)」「時(0-23)」「日(1-31)」「月(0-11)」「週(0:日,1:月,2:火,3:水,4:木,5:金,6:土)」

###


CronJob = require('cron').CronJob
module.exports = (robot) ->

  ###
  月曜の21:00 に通知
  ###
  new CronJob
    cronTime: "0 0 21 * * 1"
    onTick: ->
      robot.send {room: "trash"}, "明日は「燃えるゴミの日」"
      return
    start: true
    timeZone: 'Asia/Tokyo'

  ###
  火曜の 7:00 に通知
  ###
  new CronJob
    cronTime: "0 0 7 * * 2"
    onTick: ->
      robot.send {room: "trash"}, "今日は「燃えるゴミの日」"
      return
    start: true
    timeZone: 'Asia/Tokyo'

```

以上で設定完了。


## Hubot Tips

### hubotの名称を変更したい場合は？
  
yo hubot でつけた名前を後から変更したい場合は、  
package.jsonの name の部分を修正する。  
  
```
{
  "name": "ここに新しい名前",
  ・・・
  ・・・
```
  
修正後、Heroku にpush。  
  
Slack側のアカウントも合わせて変更。  
Browse Apps -> Hubot -> Configurations on xxxx -> Edit configuration  
  
のCustomize Nameで名前を変更。  




### Heroku が起きない
scheduler　の設定をしたのにHerokuが起きない。。。
代案を探していたら、Process SchedulerというAdd-onがありGUIもわかりやすいのでこちらを導入。

http://sota1235.hatenablog.com/entry/2015/06/10/130000
http://shokai.org/blog/archives/10108


