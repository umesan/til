# hubot導入

## 参考URL
http://qiita.com/misopeso/items/1f418dd02e89234499b3
http://qiita.com/acairojuni/items/dc4543aa5827d4c3211c

## 手順
1. 必要パッケージのインストール
```
sudo npm install -g hubot coffee-script generator-hubot
```

>これまでは、hubot --create を使っていたが、非推奨になっており、Yeoman(http://yeoman.io/) >を使う方式に変わったようだ。そのため、generator-hubot のインストールが必要となっている。

2. npm の update
npm はインストールされているが、バージョンが古い生か yo hubot でエラーが出たため
アップデートする。
```
sudo npm install -g npm
```

3. yo hubot で hubotを作成
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

4.  ローカルでテスト実行
```
# これで起動
bin/hubot

# 自分がつけた名前をつけて実行
hubotの名前 > hubotの名前 ping

# hubotが返してくれれば成功
hubot> PONG

```

