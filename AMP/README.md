
# AMP（Accelerated Mobile Pages Project）

>AMP（Accelerated Mobile Pages Project）はモバイルウェブ高速化を目的としたプロジェクトです。  
>「AMP HTML」と呼ばれる独自のテンプレートに沿ってページを作成すると、  
>Google検索結果から記事の表示までを超高速で行う事が可能です。  
>http://creatorclip.info/2016/02/wordpress-accelerated-mobile-pages/

>AMPに対応したページは検索順位が上昇するかもしれません。  
>https://www.suzukikenichi.com/blog/google-says-amp-may-be-a-ranking-factor/


## AMPの特徴

AMPを導入するとGoogle検索結果ページにてAMPアイコンが表示されたり、  
[検索結果の上部カルーセル枠](https://q-az.net/wp-content/uploads/2016/02/511.png)に取り上げられることがある。

AMPを実装導入したからといって、Googleの検索結果にすぐに反映されるというわけではない。  
現状だと下記のような特徴がある(2016/4/18)

- AMP対応していると、「ニュース性の高いキーワード」でのみ、Google検索結果上部カルーセル枠にピックアップ表示される
- Google検索時、上部カルーセルに表示されているコンテンツは公開から約3日以内のものが多い。
- ブログ・ニュース・メディア系のコンテンツページとは相性はよさそう。RSS的な感覚。
  逆に通常ページに導入するメリットはなさそう。
- スマホ向けの仕組みなので、PCメインのサイトにはメリットが今はなさそう。。。

>ニュース性の強い話題で、ビッククエリのみでカルーセルが出現しているようです。  
>AMP のプロジェクト自体ニュースメディアでの実装を  
>Google がオススメしていたので当たり前かもしれませんが、  
>ビッククエリでも直近でニュース性の話題がない場合は出ていないようです。  
>「政治」や「サッカー」ではAMP対応ページがないのかニュース性がないのわかりませんが、  
>今日現在カルーセルは出ていませんでした。  
>https://q-az.net/amp-google-carousel/


## AMPの仕様

#### 超概要
記事ページとは別で、「AMP HTML」と呼ばれる独自のテンプレートに沿ってページを作成し、  
記事ページから `<link rel="amphtml" href="ampページ">`を指定することで紐付けを行う。


#### head / metaタグの設定
- DOCTYPE宣言の後は<html ⚡>
- canonical urlの設定
- amphtmlの設定
- viewportの設定
- その他metaタグは今までどおり使用OK


#### JS / JSONの設定


#### CSSの設定


#### HTMLの設定


#### 



## 実装してみて

画像の変換が一番やっかい。
横幅・縦幅を指定していない画像に対して、調整しないといけない



