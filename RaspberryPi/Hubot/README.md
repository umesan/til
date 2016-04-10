# RaspberryPiとHubot

Raspberry Pi へのHubotのインストールは、  
[Hubotの導入方法](https://github.com/umesan/til/tree/master/hubot)とは若干異なります。

特にルーターのポート設定を変更しないでSlackとRaspberry Pi にインストールしたHubotを連携させるには、  
[XMPP](https://ja.wikipedia.org/wiki/Extensible_Messaging_and_Presence_Protocol)というプロトコルが必要。  
Slackがこちらのプロトコルをサポートしているのでこちらを利用する。  
  
Slack がオフィシャルで提供している hubot-slack アダプタの場合、  
Webhook を使いメッセージの送受信をしているため、[ポートフォワーディング](http://e-words.jp/w/ポートフォワーディング.html)が必要なので今回は利用しない。


### 参考URL
http://blog.mursts.jp/entry/2015/08/01/hubot-slack-on-raspberrypi/  
http://www.sekailab.com/wp/2014/09/18/hubot-xmpp-slack/  
http://ja.ngs.io/2014/08/01/slack-hubot-xmpp/  
http://qiita.com/hakuta@github/items/a9e66ef97768793d5f9c  
http://blue-goheimochi.hatenablog.com/entry/2014/10/19/SlackとHubotを連携させてPRIVATE_GROUPでも動くようにしてみた  

foreverの設定  
http://qiita.com/KeitaMoromizato/items/d9130b3f6c04292c129d  
  


### インストールの流れ

##### 1. sshで Raspberry Pi ログイン
```
ssh xxx@xxx.pi
```

##### 2. Hubotとその他必要な物をインストール
```
sudo npm install -g hubot coffee-script generator-hubot
```

```
sudo npm install forever --save
```

coffee-script は最新版のhubotだと不要みたいだけど、  
forever する際に使うので、このタイミングでインストールしておくこと。  

>最新版のインストール方法だとcoffee-scriptをグローバルインストールしなくても動きます。  
>この方法でインストールしたら、foreverでデーモン化する際にハマってしまったのでメモ。  
>以下のコマンドで必ずcoffee-scriptをグローバルインストールしておくこと！
>http://qiita.com/kacky69/items/ee806b8c17bbd70ca55e


##### 3. Hubot作成
```
# Hubotをインストール・配置したい場所にディレクトリを作成
mkdir /home/xxxxxx/hubot

# 作成したディレクトリへ移動
cd /home/xxxxxx/hubot

# yo で hubotを作成
yo hubot

# いろいろ聞かれるので回答
? Owner: Your Name <your@mail.address>
? Bot name: hubotの名前
? Description: 何をするhubotかの説明
? Bot adapter: slack

```

ローカルでテスト実行
```
# これで起動
bin/hubot

# 自分がつけた名前をつけて実行
hubotの名前 > hubotの名前 ping

# hubotが返してくれれば成功
hubot> PONG
```


##### 4. bot用のSlackアカウントを作成

HerokuにHubotをインストールする場合は、  
SlackのHubotインテグレーション（integration）だけで、  
bot用アカウントが自動的に作成されていた。  
  
RasberryPiにインストールする場合は、  
別途ちゃんとした新規ユーザを作成する必要がある。  
  
まずは、通常フローでSlackの新規アカウントを作成。  
既存アカウントから、新規アカウントに対してOwner権限を割り当てる。  
  
  
Slackのアドミン管理画面から、[https://my.slack.com/admin/settings#change_gateways](Gatewaysの設定項目)にて、XMPP という項目を有効にする。  
  
  

次に、新規で作ったアカウントの[個人のGateway情報画面](https://my.slack.com/account/gateways) を開き、Host, User, Pass 情報を参照。  


##### 5. 環境
Raspberry PI 側にインストールした、Hubotのディレクトリの bin/hubot に環境変数を書き出します。  
  
```
# Raspberry PI にsshでログイン後、hubotをインストールしたディレクトリ(今回は/home/xxxxxx/hubot)の下記ファイルを編集
vi /home/xxxxxx/hubot
```
  
```
#!/bin/sh

set -e

# default settings
npm install
export PATH="node_modules/.bin:node_modules/hubot/node_modules/.bin:$PATH"

# Slack Settings
export HUBOT_XMPP_HOST=conference.グループ名.xmpp.slack.com
export HUBOT_XMPP_ROOMS=Slackチャンネル@$HUBOT_XMPP_HOST
export HUBOT_XMPP_USERNAME=hubot用ユーザー名@グループ名.xmpp.slack.com
export HUBOT_XMPP_PASSWORD=取得したパスワード

# Hubot起動時に永続化をする
forever start -c coffee node_modules/.bin/hubot --name "hubotユーザー名" "$@"

# こちらコメントアウト
#exec node_modules/.bin/hubot --name "hubotユーザー名" "$@"
```
  
  
##### 6. hubot-heroku-keepaliveを削除
hubot インストール時に自動的にインストールされる、
Heroku用のパッケージが不要なので削除。

```
npm uninstall hubot-heroku-keepalive --save
```

＆  

```
# 対象のファイルを表示
$ vi external-scripts.json
```
  
下記の行を編集
```
"hubot-heroku-keepalive" #この行を削除
```
  
  
##### 7. 実行
```
bin/hubot -a xmpp
```

これでsshを落としても永続化される。  
Slack側で 作ったユーザに対して @xxxx ping と打つと PONGと返って来れば成功。

