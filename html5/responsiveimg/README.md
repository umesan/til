
#レスポンシブ・イメージ

## 概要
これまでCSSでしかできなかった、  
画面幅・解像度に対しての画像出しわけを、  
htmlのタグだけで可能にするHTML5.1の仕様のこと。  
  
レスポンシブ・イメージは下記の課題を解決することができる。  

1. 幅に合わせた寸法での表示
2. 画像のアートディレクション
3. Retina対応
  
##### 1.幅に合わせた寸法での表示
この画面幅に対しては、この画像を呼び出すという指定が可能で、  
レスポンシブWebデザインでありがちな、  
本来であれば不要な画像の読み込みを行わないようにすることができる。  
  
##### 2.画像のアートディレクション
呼び出す画像を変えるだけでなく、表示サイズもコントロール可能。  

##### 3.Retina対応
画面幅だけでなく、解像度によっての切り替え指定が可能なため、  
HTMLだけでRetina対応が可能になる。



## 実装方法
レスポンシブ・イメージは img srcset か picture で実装する。  
  
pictureの例
```
<picture>
  <source srcset="http://dummyimage.com/200x100" media="(min-width: 480px)">
  <source srcset="http://dummyimage.com/400x200" media="(min-width: 768px)">
  <img src="http://dummyimage.com/100x50" alt="100x50">
</picture>
```
  
imgの例
```
<img src="http://dummyimage.com/100x50" sizes="100vw" srcset="http://dummyimage.com/200x100 480w,http://dummyimage.com/400x200 768w" alt="">
```



## srcset
srcsetを使うと、「あるブラウザ幅に対しては、この画像を呼び出す」といった指定が可能になる。  
また sizes属性と組み合わせることで、  
「あるブラウザ幅に対しては、この画像を呼び出しつつ、このサイズで表示させる」といったことも可能。  

```
// 説明のためありえない位置での改行になっています
<img 
  src="http://dummyimage.com/100x50"
  sizes="100vw"
  srcset="http://dummyimage.com/200x100 480w,
          http://dummyimage.com/400x200 768w"
  alt=""
>
```

#### srcset属性
「あるブラウザ幅に対しては、この画像を呼び出す」を指定する属性。  
新しい単位として w を用いる。  
wで指定すると、xxx未満・xxx以上の指定となる。  

```
// 画面幅 480未満では200x100の画像を呼び出す
srcset="http://dummyimage.com/200x100 480w"
```

複数指定する場合は, で区切る。  

```
// 画面幅 480未満では200x100の画像を呼び出す
// 画面幅 768未満かそれ以上では400x200の画像を呼び出す
srcset="http://dummyimage.com/200x100 480w,
        http://dummyimage.com/400x200 768w"
```


#### sizes属性
画像の表示サイズを指定する属性。  
  
メディアクエリと同じ書式(というかメディアクエリ)で、  
ある画面幅では画像を横幅いくつで表示するかを指定可能。  
  
新しい単位として vw(viewport width) でも指定可能。  
この属性のデフォルト値は 100vwとなっていて、省略した場合は 100vw指定になる。  
（ただし、Firefoxでバグがあるらしく、省略せず sizes="100vw" を指定しておくのが無難）  

~~50vw をした場合、viewport 2 の端末では、読み込まれた画像サイズの0.5倍のサイズで出力する。~~

ビューポートは、ブラウザ表示領域でスクロールバーを含んだサイズとなり、  
幅を表す時は vw という相対的な単位で指定します。スクロールバーを含んだ全幅サイズが 100vw となります。  
  
vw の値は、スクロールバーを含むので HTML の表示領域を % で示した値と若干異なるので % ではなく vw   という単位で明確に区別していると思われますが、％値より若干大きなサイズと思っていればほぼほぼ同じようなものです。  
  
  
```
// 横幅 50pxで表示
<img 
  src="http://dummyimage.com/100x50"
  sizes="50px"
  srcset="http://dummyimage.com/200x100 480w"
>

<img 
  src="http://dummyimage.com/100x50"
  sizes="50vw"
  srcset="http://dummyimage.com/200x100 480w"
>

```



## WordPressでの指定
```
<img 
  src="http://dummyimage.com/100x50"
  sizes="(max-width: 画像のオリジナル横幅px;) 100vw, 画像のオリジナル横幅px"
  srcset="http://dummyimage.com/200x100 画像のオリジナル横幅w"
>

```
>この属性値は、画像のオリジナル横幅以下の狭い画面で見ている場合は  
>画面の横幅以上で最も小さな横幅の画像を選択せよ、  
>画像のオリジナル横幅より大きな画面で見ている場合は、オリジナルの画像を表示せよ、という意味だ。  
>(Retinaディスプレイなど、高精細な画面の場合はブラウザによって、自動的に最適な画像に読み替えられる）  
>これはほとんどの場合で上手く動く。  
>  
>例えば、ブラウザ幅が1000ピクセルなら、横幅1024ピクセルの大サイズ画像が表示され、  
>Device pixel ratioが3のiPhone 6 Plusなら414*3つまり1242ピクセルなので、  
>オリジナル画像が表示されるはず。  
>レスポンシブ画像に求める期待通りの動作だろう。  
>  
> https://blog.srytk.com/aquei/168.html




## 参考サイト
http://parashuto.com/rriver/responsive-web/responsive-images-and-picturefill-2
http://celtislab.net/archives/20151215/wp-responsive-srcset-test/

