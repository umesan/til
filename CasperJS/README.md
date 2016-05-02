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
    //画面のキャプチャを取得
    this.capture('yahoo.png');
});
 
//処理の実行
casper.run();
```

ターミナルで下記実行
```
casperjs sample.js
```

同階層に `yahoo.png` 画像が作成される
