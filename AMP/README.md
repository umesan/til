
# AMP（Accelerated Mobile Pages Project）

>AMP（Accelerated Mobile Pages Project）はモバイルウェブ高速化を目的としたプロジェクトです。  
>「AMP HTML」と呼ばれる独自のテンプレートに沿ってページを作成すると、  
>Google検索結果から記事の表示までを超高速で行う事が可能です。  
>http://creatorclip.info/2016/02/wordpress-accelerated-mobile-pages/  
  
>AMPに対応したページは検索順位が上昇するかもしれません。  
>https://www.suzukikenichi.com/blog/google-says-amp-may-be-a-ranking-factor/

## 参考URL
AMPをワードプレスにプラグイン無しで設定する方法  
https://q-az.net/amp-wordpress-without-plugin/  

【WordPress】プラグイン無しでAMP（Accelerated Mobile Pages）に対応にする手順  
http://creatorclip.info/2016/02/wordpress-accelerated-mobile-pages/  
  
AMP ページが Google 検索結果に出る基準の調査  
https://q-az.net/amp-google-carousel/  
  
朝日新聞の通常ページ  
http://www.asahi.com/articles/ASJ4L345XJ4LUTFK003.html  
  
朝日新聞のAMPページ  
http://www.asahi.com/amp/articles/ASJ4L345XJ4LUTFK003.html  


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


## AMPの実装超概要
記事ページとは別で、「AMP HTML」と呼ばれる独自のテンプレートに沿ってページを作成し、  
記事ページから <link rel="amphtml" href="ampページ">を指定することで紐付けを行う。


## [AMP仕様] htmlタグの設定
- DOCTYPE宣言の後は<html ⚡>
- canonical urlの設定
- linkにrel="amphtml"を追加
- viewportは決められた値で設定
- form系のタグ使用禁止
- 画像は <amp-img>～</amp-img>で記述すること
- iframeは <amp-iframe>～</amp-iframe>で記述すること

#### DOCTYPE宣言の後は`<html ⚡>`
normal
```
<!DOCTYPE html>
<html lang="ja">
<head prefix="og: http://ogp.me/ns# article: http://ogp.me/ns/article#">
```

amp
```
<html ⚡>
<head>
```


#### canonical urlの設定
amp
```
<link rel="canonical" href="http://xxxxxxxxxxxxx">
```



#### linkにrel="amphtml"を追加
amp
```
<link rel="amphtml" href="ampページのパス">
```


#### viewportは決められた値で設定
normal
```
<meta name="viewport" content="width=device-width, initial-scale=1">
```

amp
```
<meta name="viewport" content="width=device-width,minimum-scale=1,initial-scale=1">
```


#### form系のタグ使用禁止
form系（form,input,select,textarea）のタグがあるとエラーになるそうなので、削除すること。  
buttonタグだけはOK。


#### 画像は <amp-img>～</amp-img>で記述すること

```
<amp-img layout="responsive" src="hoge.png" width="100" height="100"></amp-img>
```

`layout="responsive"` をつけることで画像をレスポンシブに自動対応させてくれます。  
また AMP は画像の大きさ（width 値と height 値）を明記しないとエラーになるようです。


#### iframeは <amp-iframe>～</amp-iframe>で記述すること

```
<amp-iframe layout="responsive" src="hoge.html"></amp-iframe>
```

## JS / JSONの設定

- AMP用JSライブラリの追加
- schema.orgの設定
- その他JSは読み込みも不可


#### AMP用JSライブラリの追加
amp
```
<script async src="https://cdn.ampproject.org/v0.js"></script>
```

#### schema.orgの設定
```
<script type="application/ld+json">
{
    "@context": "http://schema.org",
    "@type": "BlogPosting",
    "headline": "記事のタイトル",
    "image": {
        "@type": "ImageObject",
        "url": "記事のサムネイル画像のURL",
        "height": 430,
        "width": 770
    },
    "datePublished": "2016-xx-xxTxx:xx:xx+00:00",
    "dateModified": "2016-xx-xxTxx:xx:xx+00:00",
    "author": {
        "@type": "Person",
        "name": "記事作成者の名前"
    },
    "publisher": {
        "@type": "Organization",
        "name": "サイト名",
        "logo": {
        "@type": "ImageObject",
            "url": "サイトのロゴ等",
            "width": 67,
            "height": 60
        }
    },
    "description": "記事の概要・詳細"
}
</script>
```

#### その他JSは読み込みも不可
読み込んではだめ。

例外的に呼びだせるものあり



#### CSSの設定
- `<link>`タグでの外部ファイル読み込み・インラインstyle属性の使用不可
- !important と *zoom は使用禁止
- CSSは50,000Byte(50KB)の制限あり
- AMP 専用の style タグ amp-boilerplateの読み込み


#### `<link>`タグでの外部ファイル読み込み・インラインstyle属性の使用不可

AMPではスタイルシートを外部ファイルで読み込みが禁止されているので、  
`<link>`タグでスタイルを読み込むとエラーになる。

また、タグにインラインでstyle属性をつけるのも禁止。

スタイルは、`<style amp-custom>～</style>` の中に記載する。

```
<style amp-custom>
  .style{ display: block; }
</style>
```

例外として、Google Fontは`<link>`タグで読み込みが可能。
```
// http:// だとエラーになります。https:// で指定すること
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Alegreya">
```


#### `!important` と `*zoom` は使用禁止
スタイル内に - !important と *zoom があるとエラーになります。

```
<style amp-custom>
  .style{
    display: block !important;
    *zoom: 1;
  }
</style>
```

#### CSSは50,000Byte(50KB)の制限あり
CSSは50,000Byte(50KB)の制限あり、越えるとエラーになるとのこと。


#### AMP 専用の style タグ `amp-boilerplate`の読み込み
AMP 専用の style タグ amp-boilerplateを読み込むこと。  
これを入れないと AMP として認識されないとのこと。
```
<style amp-boilerplate>body{-webkit-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-moz-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-ms-animation:-amp-start 8s steps(1,end) 0s 1 normal both;animation:-amp-start 8s steps(1,end) 0s 1 normal both}@-webkit-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-moz-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-ms-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-o-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}</style><noscript><style amp-boilerplate>body{-webkit-animation:none;-moz-animation:none;-ms-animation:none;animation:none}</style></noscript>
```


## デバッグ作業
ChromeにAMPデバッグ機能があるようなのでこちらを利用。  
具体的にはURL末尾に`#development=1` をつけてインスペクタを開く。

```
http://xxxxxxxxx.html#development=1
```



## 実装してみて苦労したところ
画像の変換（`img`から`amp-img`）へのが一番やっかいだった。    
横幅・縦幅を指定していない画像に対して、調整しないといけない。  
  
その他、不要なタグ・Chromeのデバッグツールでエラーになる箇所を  
分岐して取り除いていけばよいので、そんなに手間はかからない。

  
尚、レンタルサーバによっては、`getimagesize` や `file_get_contents`が使えないことがあるので、代替処理が必要。


##### WPでの分岐処理
```
<?php
	//AMPチェック
	$amp = false;
	$string = $post->post_content;
	if($_GET['amp'] === '1' && strpos($string,'<script>') === false && is_single()){
		$amp = true;
	}
?>
```

##### WPでの外部CSSのインライン展開 (file_get_contents が使える場合)
``` php
<?php if($amp): ?>
<style amp-custom>
<?php 
  $url = get_bloginfo('url');
  echo $url;
  $css1 = file_get_contents($url."/css/style1.css");
  $css2 = file_get_contents($url."/css/style2.css");
  $css1 = str_replace(array('@charset "UTF-8";','*zoom: 1;',' !important'),'', $css1);
  $css2 = str_replace(array('@charset "UTF-8";','*zoom: 1;',' !important'),'', $css2);
  echo $css1;
  echo $css2;
?>
</style>
<?php else: ?>
<link rel="stylesheet" href="<?php bloginfo('url'); ?>/css/style1.css">
<link rel="stylesheet" href="<?php bloginfo('url'); ?>/css/style1.css">
<?php endif; ?>

```

##### WPでの外部CSSのインライン展開 (file_get_contents が使えない場合)
``` php
<?php if($amp): ?>
<style amp-custom>
<?php 
  function curl_get_contents( $url, $timeout = 60 ){
    $ch = curl_init();
    curl_setopt( $ch, CURLOPT_URL, $url );
    curl_setopt( $ch, CURLOPT_HEADER, false );
    curl_setopt( $ch, CURLOPT_RETURNTRANSFER, true );
    curl_setopt( $ch, CURLOPT_TIMEOUT, $timeout );
    $result = curl_exec( $ch );
    curl_close( $ch );
    return $result;
  }
  $url = get_bloginfo('url');
  echo $url;
  $css1 = curl_get_contents($url."/css/style1.css");
  $css2 = curl_get_contents($url."/css/style2.css");
  $css1 = str_replace(array('@charset "UTF-8";','*zoom: 1;',' !important'),'', $css1);
  $css2 = str_replace(array('@charset "UTF-8";','*zoom: 1;',' !important'),'', $css2);
  echo $css1;
  echo $css2;
?>
</style>
<?php else: ?>
<link rel="stylesheet" href="<?php bloginfo('url'); ?>/css/style1.css">
<link rel="stylesheet" href="<?php bloginfo('url'); ?>/css/style1.css">
<?php endif; ?>
```

##### WPでの画像置換処理 (getimagesizeが使える場合)
``` php
<?php if($amp): ?>
  <?php
    $content = apply_filters( 'the_content', get_the_content() );

    // コンテンツ内の画像を取得
    preg_match_all('/<img.*src\s*=\s*[\"|\'](.*?)[\"|\'].*>/i', $content, $img_path_list);
    $count = count($img_path_list[1]);
    for ($i = 0; $i < $count; $i++){

      // 画像の縦横幅取得
      $imginfo = getimagesize($img_path_list[1][$i]);

      // 画像の置換後にも必要な箇所を取得
      preg_match('/<img src="(.*?)"(.*?)\/>/', $img_path_list[0][$i],$match);

      // 置換後のamp形式のタグ
      $amp_img = '<amp-img layout="responsive" src="'.$match[1].'" width="'.$imginfo[0].'" height="'.$imginfo[1].'" '.$match[2].'></amp-img>';

      // imgタグをamp形式に置換する
      $content = str_replace($match[0], $amp_img, $content);

    }
    echo $content;
  ?>
<?php else: ?>
  <?php the_content(); ?>
<?php endif;?>
```


##### WPでの画像置換処理 (getimagesizeが使えない場合)
``` php
<?php if($amp): ?>
  <?php
    /**
     * 画像のURLからattachemnt_idを取得する
     *
     * @param string $url 画像のURL
     * @return int attachment_id
     */
    function get_attachment_id($url){
      global $wpdb;
      $sql = "SELECT ID FROM {$wpdb->posts} WHERE post_name = %s";
      preg_match('/([^\/]+?)(-e\d+)?(-\d+x\d+)?(\.\w+)?$/', $url, $matches);
      $post_name = $matches[1];
      return (int)$wpdb->get_var($wpdb->prepare($sql, $post_name));
    }

    $content = apply_filters( 'the_content', get_the_content() );

    // コンテンツ内の画像を取得
    preg_match_all('/<img.*src\s*=\s*[\"|\'](.*?)[\"|\'].*>/i', $content, $img_path_list);
    $count = count($img_path_list[1]);
    for ($i = 0; $i < $count; $i++){

      // 画像の縦横幅取得
      $imginfo = wp_get_attachment_image_src(get_attachment_id($img_path_list[1][$i]),"full");

      // 画像の置換後にも必要な箇所を取得
      preg_match('/<img src="(.*?)"(.*?)\/>/', $img_path_list[0][$i],$match);

      // 置換後のamp形式のタグ
      $amp_img = '<amp-img layout="responsive" src="'.$match[1].'" width="'.$imginfo[1].'" height="'.$imginfo[2].'" '.$match[2].'></amp-img>';

      // imgタグをamp形式に置換する
      $content = str_replace($match[0], $amp_img, $content);

    }
    echo $content;
  ?>
<?php else: ?>
  <?php the_content(); ?>
<?php endif;?>
```



