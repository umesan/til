# CasperJS

>PhantomJS ですが、グラフィカルな画面のないブラウザで「ヘッドレスブラウザ」と呼ばれるブラウザです。コマンドラインからブラウザの機能を使うことができ、フォームの操作やページの要素を取得することができます。よく CI ツールと組み合わせて自動テストを行ったり Web スクレイピングで使われたりします。
>PhantomJS のレンダリングエンジンには Webkit が使われていますが、Gecko を使っている SlimerJS というのもあります。
>CasperJS は、その PhantomJS をより手軽に使えるようにするライブラリです。
>CasperJS は SlimerJS もサポートしているので、エンジンを切り替えることで両方のエンジンでレンダリングされた結果を検証することができます。  
>http://www.tam-tam.co.jp/tipsnote/javascript/post8686.html

## Mac環境へのインストール
```
sudo npm install -g phantomjs casperjs
```

## テスト用コード
sample.jsで保存
```
//casperオブジェクトを生成
var casper = require('casper').create();
 
//指定のURLへ遷移する
casper.start('http://yahoo.co.jp', function() {

    // 画面のキャプチャを取得
    this.capture('yahoo.png');

    // 要素をクリック
    this.click('#btn');

    // 6000秒待つ
    this.wait(6000,function(){
      console.log('wait end');
    });


});
 
//処理の実行
casper.run();
```

ターミナルで下記実行
```
casperjs sample.js
```

同階層に `yahoo.png` 画像が作成される



## RasberyPiへのインストール


#### 参考URL
https://quaintproject.wordpress.com/2015/04/26/how-to-install-casperjs-on-the-raspberry-pi/

#### インストール場所
インストール場所は /home/xx/ とする

#### インストールの流れ

##### 1.インストール場所へ移動
```
cd /home/xx/
```

##### 2.casperjsインストール
```
git clone git://github.com/n1k0/casperjs.git
```
（場合によっては sudoを頭に付加）

##### 3.インストール完了後移動
```
cd casperjs
```

##### 4.シンボリックリンクの作成
```
sudo ln -sf `pwd`/bin/casperjs /usr/local/bin/casperjs
```

##### 5.phantomjs-raspberrypiのインストール
```
git clone https://github.com/piksel/phantomjs-raspberrypi.git

```
（/home/xx/casperjs/にいること前提で上記を実行）


##### 6.ディレクトリ移動
```
cd phantomjs-raspberrypi/bin
```

##### 6.パーミッション変更
```
sudo chmod +x phantomjs
```

##### 7.シンボリックリンク作成
```
sudo ln -s /home/xx/casperjs/phantomjs-raspberrypi/bin/phantomjs /bin/phantomjs
```

##### 8.テスト実行
```
phantomjs --version

# 1.9.8

casperjs

# CasperJS version 1.1.1 at /home/ume/casperjs, using phantomjs version 1.9.8
# Usage: casperjs [options] script.[js|coffee] [script argument [script argument ...]]
#        casperjs [options] test [test path [test path ...]]
#        casperjs [options] selftest
#        casperjs [options] __selfcommandtest
# 
# Options:
# 
# --verbose   Prints log messages to the console
# --log-level Sets logging level
# --help      Prints this help
# --version   Prints out CasperJS version
# --engine=name Use the given engine. Current supported engine: phantomjs and slimerjs
# 
# Read the docs http://docs.casperjs.org/

```


## RasberyPiへのPhantomJSインストール（最新版 2.1.1）
PhantomJS 最新版をインストールするにはソースからビルドが必要となる

### 参考URL
http://blog.jiikko.com/36
http://blog.mursts.jp/entry/2015/10/16/build-phantomjs-on-raspberrypi2/


#### 1. swap領域を増やす
RasberyPiデフォルトのswap領域（100MB）のままだと、ビルド時にメモリが足りなくなりエラーになる。  
`/etc/dphys-swapfile`の設定ファイルを編集し、4GB(RAMが1GB,swapを3GB)くらいにする

1-1. 設定ファイルを開く
```
vi /etc/dphys-swapfile
```

1-2. `CONF_SWAPSIZE`と`CONF_MAXSWAP`を下記のように設定
```
CONF_SWAPSIZE=3048
CONF_MAXSWAP=3048
```

1-3. swap領域の再作成
```
sudo /etc/init.d/dphys-swapfile restart
```



#### 2. 必要パッケージを取得
```
sudo apt-get install build-essential g++ flex bison gperf ruby perl libsqlite3-dev libfontconfig1-dev libicu-dev libfreetype6 libssl-dev libpng-dev libjpeg-dev
```


#### 3. ソースをもってくる

インストール先は`/home/xx/casperjs/` 以下だったので、まずは移動。
```
cd /home/xx/casperjs/
```

git からクローン
```
git clone git://github.com/ariya/phantomjs.git
```

クローン完了したら`phantomjs`へ移動
```
cd phantomjs
```

最新バージョン(2.1.1)にチェックアウト
```
git checkout 2.1.1
```


#### 4. ビルドする
チェックアウト後ビルドする。
参考サイトだと`./build.sh`と書いてあるが、
最新版チェックアウトすると`./build.py`しかなかったのでそれを実行
```
./build.py
```

`ビルド完了するのに半日くらいかかるので待つ`

#### 5. バージョン確認
ビルドが完了すると`bin`ディレクトリができていて中に`phantomjs`があるのを確認。
```
cd bin
```

バージョンを確認してみる
```
phantomjs --version

# 1.9.8
```

古い場合は、シンボリックリンクを貼り直す必要あり。  
シンボリックリンク先へ移動して解除。
```
cd /bin/
sudo unlink phantomjs
```

リンボリックリンク貼り直し
```
sudo ln -s /home/xx/casperjs/phantomjs/bin/phantomjs /bin/phantomjs
```

もう一度バージョン確認。
```
phantomjs --version

# 2.1.1
```









